# STEP 9 — Business Workflows

**Product:** Peter Park Here  
**Release:** MVP  
**Primary market:** Bengaluru, India  
**Document purpose:** Define the end-to-end business processes and important exception paths derived from the PRD.

---

# 1. Purpose

The Product Requirements Document defines **what Peter Park Here must enable**.

This document defines **how the business process operates from beginning to end**.

It describes:

- Actors involved in each process.
- The sequence of business activities.
- Decisions and conditions.
- Business outcomes.
- Success and failure paths.
- Booking and payment state transitions.
- Important exception scenarios.
- Responsibilities between customer, owner, and platform.

This document intentionally remains at the **business/process level**.

It does not prescribe:

- Database implementation.
- REST APIs.
- Microservice boundaries.
- PostgreSQL transactions.
- Redis locks.
- Kafka events.
- Java classes.
- Kubernetes.
- AWS infrastructure.

Those decisions belong to HLD/LLD.

---

# 2. Workflow Modeling Approach

The workflows use three complementary representations.

## 2.1 Flow

Shows the chronological business process.

```text
Start
  ↓
Business activity
  ↓
Decision
  ├── Yes → next activity
  └── No  → exception/outcome
  ↓
End
```

## 2.2 Swimlane-style responsibility

Used when responsibility moves between:

- Customer.
- Parking Owner.
- Peter Park Here.
- Payment Provider.

The workflow describes **business responsibility**, not technical services.

## 2.3 State Transition

Used when an entity has a lifecycle.

For example:

```text
Booking Requested
       ↓
Payment Pending
       ↓
Confirmed
       ↓
Cancelled / Completed
```

This is particularly important for bookings and payments because many failures arise from interactions between their states.

---

# 3. Core Business Actors

| Actor | Responsibility |
|---|---|
| Customer | Searches, evaluates, reserves, pays for and manages parking |
| Parking Owner | Lists parking, controls availability and receives bookings |
| Peter Park Here | Operates marketplace rules, pricing, booking and marketplace processes |
| Payment Provider | Processes payment/refund in the eventual production flow |
| Administrator | Handles basic marketplace operations and exceptions |

For the MVP, payment may be represented using a mock/test payment flow.

---

# 4. Customer Parking Workflow

## 4.1 Business Objective

Allow a customer to move from an identified parking need to a successfully confirmed reservation.

## 4.2 Primary Flow

```text
Customer needs parking
        ↓
Search for parking
        ↓
Select parking
        ↓
Enter/select vehicle
        ↓
Select date and time
        ↓
Check availability
        ↓
Available?
   ┌────┴────┐
  No        Yes
   ↓          ↓
Show        Calculate
unavailable   price
              ↓
         Customer accepts?
          ┌────┴────┐
         No        Yes
          ↓          ↓
        End       Reserve
                      ↓
               Re-check availability
                      ↓
                Still available?
                  ┌───┴───┐
                 No      Yes
                  ↓        ↓
              Reservation  Create
                fails      booking
                            ↓
                       Pay / payment
                            ↓
                       Payment succeeds?
                       ┌────┴────┐
                      No        Yes
                       ↓          ↓
                 Payment failed  Confirm
                                  booking
                                    ↓
                              Show confirmation
                                    ↓
                                   End
```

---

# 5. Customer Workflow — Detailed Steps

## Step C1 — Identify Parking Need

The customer requires parking for a particular location and period.

### Input

- Destination/location.
- Required date.
- Required start time.
- Required end time.
- Vehicle.

### Output

Parking search criteria.

---

## Step C2 — Search

The customer searches for parking matching the requested criteria.

### Business rule

Search results should represent parking relevant to the requested location and requirements.

### Outcome

One of:

- Matching parking found.
- No suitable parking found.
- Invalid search request.

---

## Step C3 — Select Parking

The customer evaluates a parking option.

The customer can review relevant information such as:

- Location.
- Parking characteristics.
- Supported vehicle.
- Facilities.
- Rules.
- Pricing information.
- Availability information.

### Outcome

Customer either:

- Continues with the parking.
- Selects another parking option.
- Ends the journey.

---

## Step C4 — Check Availability

The customer requests availability for the desired period.

The marketplace evaluates:

```text
Listing active?
     AND
Vehicle compatible?
     AND
Owner availability permits period?
     AND
No conflicting confirmed booking?
```

If all conditions are satisfied:

> Parking is currently available.

Otherwise:

> Parking is unavailable for the requested period.

---

# 6. Price Calculation Workflow

If parking is available, the marketplace calculates the applicable price.

```text
Parking selected
      ↓
Read applicable pricing rules
      ↓
Evaluate:
  Location
  Vehicle type
  Duration
  Time
  Day
  Parking characteristics
      ↓
Calculate parking charge
      ↓
Calculate applicable marketplace fee
      ↓
Calculate total
      ↓
Display price
```

## Business Principle

The initial pricing model is **deterministic**.

Given the same relevant inputs and pricing rules, the resulting price should be the same.

No ML or dynamic demand-pricing model is part of the MVP.

---

# 7. Reservation Workflow

Price visibility does not itself create a reservation.

The customer must explicitly confirm the reservation.

```text
Customer accepts price
        ↓
Reservation requested
        ↓
Validate request
        ↓
Re-check availability
        ↓
Available?
   ┌────┴────┐
  No        Yes
   ↓          ↓
Reject      Create reservation
             ↓
        Booking created
             ↓
        Continue to payment
```

## Important Business Rule

Availability must be checked again when the customer attempts to reserve.

This protects against the following situation:

```text
Customer A checks availability
        ↓
Parking appears available

Customer B reserves parking
        ↓
Parking becomes unavailable

Customer A attempts reservation
        ↓
Customer A must NOT receive
a second successful reservation
```

This is one of the most important workflows in the system.

---

# 8. Payment Workflow

For the MVP, payment can be represented through a mock/test payment process.

```text
Reservation request
       ↓
Booking/payment process starts
       ↓
Payment attempted
       ↓
Payment successful?
   ┌────┴────┐
  No        Yes
   ↓          ↓
Payment     Confirm
failed      booking
   ↓          ↓
Failure     Customer receives
outcome     confirmation
```

The exact production payment behavior is intentionally not finalized yet.

---

# 9. Booking State Model

The business workflow should distinguish the **booking lifecycle** from the payment lifecycle.

A simplified booking lifecycle is:

```text
Requested
   ↓
Payment Pending
   ↓
Confirmed
   ↓
Completed
```

Alternative outcomes:

```text
Requested ─────────→ Rejected
     │
     └─────────────→ Cancelled

Payment Pending ───→ Payment Failed
```

A confirmed booking may later transition to:

```text
Confirmed
    ↓
Cancelled
```

or:

```text
Confirmed
    ↓
Completed
```

## Important Principle

A booking state must describe what happened to the reservation.

A payment state must describe what happened to the payment.

They are related but should not be treated as the same state.

---

# 10. Owner Workflow

## 10.1 Business Objective

Allow an owner to convert available parking capacity into a bookable marketplace listing.

## 10.2 Primary Flow

```text
Owner registers
      ↓
Create parking listing
      ↓
Provide parking details
      ↓
Configure supported vehicles
      ↓
Configure facilities/rules
      ↓
Define availability
      ↓
Define pricing
      ↓
Listing ready?
   ┌────┴────┐
  No        Yes
   ↓          ↓
Complete    Make listing
details     available
              ↓
        Customer discovers listing
              ↓
          Booking received
              ↓
        Owner views booking
              ↓
          Parking used
              ↓
       Owner becomes eligible
       for applicable payout
```

---

# 11. Owner Workflow — Detailed Steps

## Step O1 — Register

The owner creates an account.

The marketplace establishes the owner identity required to manage parking listings.

---

## Step O2 — Create Listing

The owner provides information about the parking space.

Examples:

- Location.
- Parking characteristics.
- Supported vehicles.
- Facilities.
- Rules.
- Relevant access information.

---

## Step O3 — Configure Availability

The owner defines when the parking can be used by customers.

Example:

```text
Monday-Friday
09:00-18:00
```

The exact MVP availability model remains a product decision.

---

## Step O4 — Configure Pricing

The owner provides the pricing information used by the marketplace's deterministic pricing rules.

The owner does not directly bypass marketplace pricing rules.

---

## Step O5 — Make Available

Once required information is complete, the listing becomes eligible for customer discovery and booking.

```text
Draft
  ↓
Ready
  ↓
Active
```

An inactive listing cannot be successfully booked.

---

# 12. Owner Receives Booking

When a customer successfully reserves the parking:

```text
Customer reservation
       ↓
Booking confirmed
       ↓
Owner receives booking information
       ↓
Owner can view:
  Parking
  Customer/booking information
  Vehicle
  Date/time
  Booking status
       ↓
Parking is used
       ↓
Applicable owner earnings calculated
       ↓
Owner payout process
```

The exact payout timing remains a product decision.

---

# 13. Owner Earnings Workflow

The marketplace uses the previously selected **hybrid revenue model**.

Conceptually:

```text
Customer payment
       ↓
Gross booking value
       ↓
Applicable marketplace fees
       ↓
Owner's applicable amount
       ↓
Owner payout
```

Potential marketplace revenue includes:

- Transaction commission.
- Customer/platform service fee.
- Future owner premium/subscription services.

Exact percentages are **TBD**.

---

# 14. Cancellation Workflow

## 14.1 Customer Cancellation

```text
Customer requests cancellation
          ↓
Identify booking
          ↓
Check booking status
          ↓
Check cancellation eligibility
          ↓
Eligible?
      ┌────┴────┐
     No        Yes
      ↓          ↓
Reject       Cancel booking
request          ↓
             Calculate refund
                  ↓
             Process refund
                  ↓
             Update outcome
                  ↓
             Notify relevant
                parties
```

---

# 15. Cancellation Eligibility

Cancellation eligibility depends on business policy.

The decision may consider:

- Booking status.
- Time remaining before parking start.
- Whether parking has already been used.
- Applicable cancellation policy.

The exact cancellation window and refund percentages are **not yet finalized**.

Therefore:

```text
Cancellation requested
        ↓
Eligibility rules
        ↓
Eligible?
```

is part of the current workflow, while the exact rules will be finalized before production payment behavior.

---

# 16. Refund Workflow

If the cancellation is eligible for a refund:

```text
Cancelled booking
       ↓
Determine refundable amount
       ↓
Determine applicable fees
       ↓
Calculate refund
       ↓
Refund required?
   ┌────┴────┐
  No        Yes
   ↓          ↓
No refund   Initiate refund
             ↓
        Refund successful?
          ┌────┴────┐
         No        Yes
          ↓          ↓
      Refund issue  Refund recorded
```

The product should distinguish:

- Cancellation completed.
- Refund calculated.
- Refund requested.
- Refund succeeded.
- Refund failed.

This becomes important when production payments are introduced.

---

# 17. Failure Workflow — Parking Becomes Unavailable

## Scenario

Customer sees parking as available, but another customer reserves it before the first customer completes their booking.

```text
Customer A checks availability
          ↓
Parking appears available
          ↓
Customer B reserves parking
          ↓
Customer A confirms reservation
          ↓
Availability rechecked
          ↓
Parking available?
       ┌───┴───┐
      No      Yes
       ↓        ↓
Reservation    Continue
rejected       booking
```

## Business Outcome

Customer A does **not** receive a conflicting confirmed booking.

Customer A must be told that the parking is no longer available.

---

# 18. Failure Workflow — Payment Fails

```text
Booking/payment process
        ↓
Payment attempted
        ↓
Payment fails
        ↓
Payment marked failed
        ↓
Booking confirmation?
        ↓
No successful confirmation
        ↓
Customer informed
```

The exact reservation-hold behavior around payment is a later product/HLD decision.

The important business rule is:

> A failed payment must not be represented as a successful payment.

---

# 19. Failure Workflow — Customer Cancels

```text
Customer requests cancellation
          ↓
Check eligibility
          ↓
Eligible?
   ┌──────┴──────┐
  No            Yes
   ↓              ↓
Reject          Cancel booking
                  ↓
             Calculate refund
                  ↓
             Process applicable
                refund
                  ↓
             Update booking
                  ↓
             Inform customer
```

If the cancellation is not eligible, the booking remains governed by its existing state and applicable policy.

---

# 20. Failure Workflow — Owner Cancels

Owner cancellation is an important marketplace exception because the customer may have already paid.

```text
Owner requests cancellation
          ↓
Identify booking
          ↓
Check booking status
          ↓
Can owner cancel?
          ↓
       Yes
          ↓
Cancel booking
          ↓
Determine customer refund
          ↓
Process applicable refund
          ↓
Update booking
          ↓
Customer informed
```

## Business Considerations

The eventual policy must define:

- Whether owner cancellation is always permitted.
- Customer refund amount.
- Treatment of marketplace fees.
- Owner penalties, if any.
- Whether repeated owner cancellations affect listing status.

These are product decisions, not technical decisions.

---

# 21. Critical Failure — Payment Succeeds but Booking Confirmation Fails

This is one of the most important business workflows in the system.

## Scenario

The customer successfully pays, but the reservation cannot be confirmed.

```text
Customer requests booking
          ↓
Availability validated
          ↓
Payment attempted
          ↓
Payment succeeds
          ↓
Booking confirmation attempted
          ↓
Booking confirmation fails
          ↓
Booking NOT confirmed
          ↓
Determine payment recovery
          ↓
Refund/recovery required
          ↓
Customer informed
```

## Business Invariant

The customer must never be shown:

> "Booking confirmed"

when the marketplace does not actually have a confirmed reservation.

## Required Business Outcome

The eventual production workflow must define a recovery mechanism such as:

```text
Payment succeeded
       +
Booking failed
       ↓
Payment reconciliation
       ↓
Refund / recovery
       ↓
Customer informed
```

The exact mechanism is intentionally left for HLD/payment design.

---

# 22. Duplicate Booking Request

## Scenario

The customer presses the reservation button twice, retries after a timeout, or submits the same request again.

```text
Customer sends booking request
          ↓
Booking request received
          ↓
Same request already processed?
       ┌────┴────┐
      Yes        No
       ↓          ↓
Return existing  Process
booking/result   reservation
```

## Business Rule

Repeated submission must not unintentionally produce multiple successful bookings for the same customer request.

## Important Distinction

This requirement is about **business idempotency**.

The implementation mechanism will be determined during HLD/LLD.

---

# 23. Complete Marketplace Booking Flow

The main marketplace process can therefore be represented as:

```text
                         OWNER
                           │
                           ▼
                    Create Listing
                           │
                           ▼
                    Configure Space
                           │
                           ▼
                  Define Availability
                           │
                           ▼
                    Define Pricing
                           │
                           ▼
                      LISTING LIVE
                           │
                           │
                           ▼
CUSTOMER ────────► Search Parking
                           │
                           ▼
                    Select Parking
                           │
                           ▼
                   Check Availability
                           │
                           ▼
                    Calculate Price
                           │
                           ▼
                  Customer Confirms
                           │
                           ▼
                  Re-check Availability
                           │
                     ┌─────┴─────┐
                     │           │
                   FAIL        AVAILABLE
                     │           │
                     ▼           ▼
                  Reject      Reserve
                                 │
                                 ▼
                              Pay
                                 │
                          ┌──────┴──────┐
                          │             │
                        FAIL          SUCCESS
                          │             │
                          ▼             ▼
                    Payment Failed   Confirm
                                      Booking
                                        │
                         ┌──────────────┴──────────────┐
                         │                             │
                         ▼                             ▼
                   Customer sees                 Owner sees
                    confirmation                  booking
                         │                             │
                         └──────────────┬──────────────┘
                                        ▼
                                   Parking Used
                                        │
                                        ▼
                                  Owner Earnings
```

---

# 24. Booking Lifecycle

The booking lifecycle is central to the marketplace.

```text
                    ┌──────────────┐
                    │   Requested  │
                    └──────┬───────┘
                           │
                validation/payment
                           │
             ┌─────────────┴─────────────┐
             │                           │
             ▼                           ▼
        ┌──────────┐                ┌──────────┐
        │ Rejected │                │ Confirmed│
        └──────────┘                └────┬─────┘
                                        │
                              ┌─────────┴─────────┐
                              │                   │
                              ▼                   ▼
                         ┌─────────┐         ┌─────────┐
                         │Cancelled│         │Completed│
                         └─────────┘         └─────────┘
```

Payment failure may prevent a booking from reaching `Confirmed`, depending on the final payment policy.

---

# 25. Payment Lifecycle

Payment should have its own conceptual lifecycle.

```text
Payment Not Started
        ↓
Payment Pending
        ↓
   ┌────┴────┐
   │         │
   ▼         ▼
Succeeded  Failed
   │
   ▼
Refund Required?
   │
 ┌─┴──┐
No   Yes
     ↓
Refund Pending
     ↓
┌────┴────┐
│         │
▼         ▼
Succeeded Failed
```

This separation is important because:

> **Booking status and payment status can temporarily disagree.**

For example:

```text
Payment = Succeeded
Booking = Not Confirmed
```

is an exceptional but possible business state that the system must be able to reconcile.

---

# 26. Owner Listing Lifecycle

```text
Draft
  ↓
Configured
  ↓
Active
  ↓
Inactive
```

Possible future states may include:

```text
Active
  ↓
Suspended
  ↓
Active
```

An inactive/suspended listing cannot accept new bookings.

Existing confirmed bookings must not be silently invalidated merely because future listing availability changes.

---

# 27. Exception Matrix

| Scenario                                   | Trigger                        | Business Decision               | Expected Outcome                      |
|--------------------------------------------|--------------------------------|---------------------------------|---------------------------------------|
| No parking found                           | Search produces no match       | No suitable supply              | Customer sees no-result state         |
| Parking unavailable                        | Requested period unavailable   | Reject reservation              | Customer selects another option       |
| Parking becomes unavailable during booking | Another customer books first   | Revalidate availability         | First booking fails                   |
| Invalid vehicle                            | Vehicle incompatible           | Reject                          | Customer selects compatible parking   |
| Invalid time                               | Start >= end                   | Reject                          | Customer corrects request             |
| Payment fails                              | Payment unsuccessful           | Do not confirm payment          | Customer receives failure             |
| Customer cancels                           | Customer requests cancellation | Check policy                    | Cancel/refund according to policy     |
| Owner cancels                              | Owner cancels booking          | Apply owner-cancellation policy | Booking cancelled/refund handled      |
| Payment succeeds, booking fails            | Confirmation cannot complete   | Reconcile payment               | No false booking confirmation         |
| Duplicate booking request                  | Same request repeated          | Treat repeat safely             | Do not create unintended duplicate    |
| Listing unavailable                        | Owner disables listing         | Prevent new bookings            | Existing confirmed bookings preserved |
| Refund fails                               | Refund cannot complete         | Record unresolved refund        | Operational follow-up required        |

---

# 28. Business Invariants

These are the most important rules that must remain true regardless of implementation.

### BI-01 — No Double Booking

Two customers cannot both successfully hold overlapping confirmed reservations for the same parking space.

### BI-02 — No False Confirmation

A customer must not be told a booking is confirmed unless the business process has actually confirmed the reservation.

### BI-03 — Payment Truth

A failed payment must not be represented as successful.

### BI-04 — Price Transparency

The customer must know the applicable price before confirming the reservation.

### BI-05 — Availability Revalidation

Availability observed during search does not guarantee availability at reservation time.

### BI-06 — Duplicate Safety

Repeated