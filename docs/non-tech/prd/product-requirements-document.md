# Product Requirements Document (PRD)

**Product:** Peter Park Here  
**Release:** MVP  
**Version:** 1.0  
**Primary market:** Bengaluru, India  
**Document status:** Product definition

## 1. Product Overview

Peter Park Here is a two-sided parking marketplace that connects people who need parking with owners who have unused or available parking capacity.

Customers should be able to discover suitable parking near their destination, understand whether it is available for the required period, see the applicable price, and reserve it.

Parking owners should be able to list available parking spaces, define when they are available, provide relevant information and pricing, and receive bookings.

The MVP exists to prove the fundamental marketplace loop:

> **Owner provides parking → customer discovers parking → customer checks availability and price → customer reserves → owner receives booking.**

The product initially focuses on Bengaluru and a controlled MVP scope rather than attempting to solve every parking use case.

---

# 2. Problem

Customers can have difficulty finding suitable parking close to their destination, particularly when parking demand is concentrated in specific locations and time periods.

Owners may have parking capacity that is unused for part of the day or week but lack a simple mechanism to make that capacity discoverable and bookable.

Peter Park Here aims to address this coordination problem by connecting parking demand with available private parking capacity.

The problem and assumptions behind this product are documented in the earlier Problem Discovery, Users, Value Proposition, Business Model, and Assumptions documents.

---

# 3. Product Goals

## 3.1 Customer Goal

Enable a customer to find and reserve suitable parking with less uncertainty about:

- Location.
- Availability.
- Price.
- Parking suitability.
- Reservation status.

## 3.2 Owner Goal

Enable a parking owner to make unused parking capacity available and manage bookings without excessive manual coordination.

## 3.3 Marketplace Goal

Create a functional connection between parking supply and parking demand.

The MVP should demonstrate that:

> **A customer can successfully reserve a parking space that an owner has made available.**

## 3.4 Learning Goal

The MVP should also allow us to learn whether the assumptions identified during discovery are reasonable.

This is important because the MVP is not only a software milestone; it is also a product-learning experiment.

---

# 4. Non-Goals

The MVP is not intended to solve:

- Dynamic/ML-based pricing.
- Advanced parking recommendations.
- Loyalty programs.
- Ratings and reviews.
- Corporate parking management.
- Multi-city optimization.
- Sophisticated fraud detection.
- Automated dispute resolution.
- Advanced analytics.
- Complex recurring bookings.
- AI parking assistant.
- Large-scale distributed infrastructure.
- Production-grade payment settlement.

These may be considered in later releases.

---

# 5. Target Users

## 5.1 Parking Customer

A person looking for parking near a destination for a specific period.

### Primary need

> "I need suitable parking near where I am going, at the time I need it, and I want to know what it will cost before I reserve."

---

## 5.2 Parking Owner

A person or organization with parking capacity that can be made available to other users.

### Primary need

> "I have parking capacity that I don't always use and want to make it available to customers while retaining control over when it can be used."

---

## 5.3 Administrator

A platform operator responsible for basic marketplace operations.

The administrator is not part of the primary marketplace transaction but is required for basic operational management.

---

# 6. Core User Journeys

## 6.1 Customer Journey

```text
Search
  ↓
Evaluate parking
  ↓
Select parking
  ↓
Specify date/time
  ↓
Check availability
  ↓
Calculate/view price
  ↓
Reserve
  ↓
View booking
```

---

## 6.2 Owner Journey

```text
Register
  ↓
Create parking listing
  ↓
Provide parking details
  ↓
Define supported vehicles
  ↓
Define availability
  ↓
Provide pricing
  ↓
Receive/manage bookings
```

---

# 7. MVP Scope

## Must Have

### Customer

- Registration/sign-in.
- Vehicle information.
- Parking search.
- Parking details.
- Date/time selection.
- Availability checking.
- Price calculation.
- Reservation.
- Booking viewing.
- Basic cancellation.

### Owner

- Registration/sign-in.
- Parking listing creation.
- Parking details.
- Supported vehicle information.
- Availability management.
- Pricing information.
- Booking viewing/management.

### Marketplace

- Match customers with suitable parking.
- Prevent successful conflicting reservations.
- Calculate deterministic prices.
- Maintain booking status.
- Make bookings visible to the relevant users.

### Payment

A mock/test payment flow may be included to exercise the payment lifecycle.

Production payment processing is not required for the first engineering MVP.

---

## Should Have

- Search filters.
- Map-oriented search.
- Basic notifications.
- Booking history.
- Owner photos.
- Facilities.
- Parking rules.
- Basic administrative operations.
- Basic support/dispute handling.

---

## Could Have

- Ratings/reviews.
- Favourites.
- Recurring bookings.
- Recommendations.
- Coupons.
- Loyalty.
- Advanced owner analytics.
- Rich notification preferences.
- Advanced search ranking.

---

# 8. Functional Requirements

## FR-01 — Account Registration and Authentication

### Purpose

Allow customers and owners to create and access their accounts.

### User

Customer / Owner

### Trigger

User registers or attempts to sign in.

### Expected Behavior

The product allows a user to create an account and subsequently authenticate.

### Inputs

Required account information and authentication information.

### Outputs

Successful authenticated access or an appropriate failure message.

### Business Rules

- Required registration information must be valid.
- Accounts must satisfy applicable uniqueness rules.
- Protected actions require authentication.
- Users must only access information they are authorized to access.

### Acceptance Criteria

- A valid user can register.
- Invalid registration information is rejected.
- A registered user can sign in.
- An unauthenticated user cannot perform protected actions.

---

# 9. Vehicle Management

## FR-02 — Manage Vehicle

### Purpose

Allow customers to associate vehicles with parking reservations.

### User

Customer

### Trigger

Customer adds, updates, or selects a vehicle.

### Expected Behavior

The customer can maintain vehicle information and select a vehicle when making a reservation.

### Inputs

- Vehicle information.
- Vehicle type.

### Outputs

Stored vehicle information.

### Business Rules

- Vehicle information must be valid.
- The selected vehicle must be compatible with the selected parking space.

### Acceptance Criteria

- Customer can add a valid vehicle.
- Customer can view their vehicle.
- Customer can select a vehicle during booking.
- Incompatible vehicle/parking combinations are rejected.

---

# 10. Parking Listing

## FR-03 — Create Parking Listing

### Purpose

Allow an owner to make parking capacity discoverable through the marketplace.

### User

Owner

### Trigger

Owner creates a listing.

### Expected Behavior

The owner provides sufficient information for the parking space to be evaluated and potentially booked.

### Required information

- Location.
- Parking characteristics.
- Supported vehicle types.
- Availability.
- Pricing information.
- Applicable facilities/rules.

### Business Rules

- Only the owner can modify their listing.
- Required information must be present before a listing becomes bookable.
- An inactive listing cannot be booked.

### Acceptance Criteria

- Owner can create a valid listing.
- Owner can update the listing.
- Customer can view an active listing.
- An inactive listing cannot be reserved.

---

# 11. Parking Search

## FR-04 — Search for Parking

### Purpose

Allow customers to discover parking suitable for their needs.

### User

Customer

### Trigger

Customer performs a parking search.

### Inputs

At minimum:

- Desired location.
- Date.
- Start time.
- End time.
- Vehicle type.

### Expected Behavior

The product returns parking options relevant to the requested location, time period, and vehicle.

### Outputs

Search results containing enough information to evaluate each option.

### Business Rules

- Ineligible parking should not be presented as available for the requested requirements.
- The customer should be able to distinguish relevant available options from options that cannot satisfy the request.

### Failure Conditions

- Invalid search parameters.
- No suitable parking found.

### Acceptance Criteria

- Valid search parameters return matching parking when available.
- No-result searches communicate that no suitable parking was found.
- Search results provide enough information to select an option.

---

# 12. Parking Details

## FR-05 — View Parking Details

### Purpose

Allow a customer to evaluate a parking space before reserving it.

### User

Customer

### Trigger

Customer selects a search result.

### Expected Behavior

The product displays relevant parking information.

### Information

This may include:

- Location.
- Parking type.
- Supported vehicle types.
- Facilities.
- Rules.
- Availability.
- Pricing information.

### Acceptance Criteria

- Customer can view the selected parking information.
- Displayed information corresponds to the current listing.

---

# 13. Availability

## FR-06 — Check Parking Availability

### Purpose

Determine whether a parking space can satisfy a customer's requested parking period.

### User

Customer

### Trigger

Customer requests availability for a parking space.

### Inputs

- Parking space.
- Start date/time.
- End date/time.
- Vehicle type.

### Expected Behavior

The product determines whether the parking space can be reserved for the complete requested period.

### Business Rules

A parking space is eligible only when:

1. The listing is active.
2. The requested time period is valid.
3. The requested period is permitted by the owner's availability.
4. The vehicle is supported.
5. The space does not already have a conflicting confirmed booking.

### Important Rule

Two customers must never both successfully reserve the same parking space for overlapping periods.

### Acceptance Criteria

- Available parking is reported as available.
- Unavailable parking is reported as unavailable.
- Unsupported vehicles are rejected.
- Invalid time periods are rejected.
- Overlapping successful reservations are prevented.

---

# 14. Pricing

## FR-07 — Calculate Parking Price

### Purpose

Calculate the amount applicable to a customer's requested parking period.

### User

Customer

### Trigger

Customer requests a price for a parking option.

### Inputs

- Parking space.
- Vehicle type.
- Date.
- Start time.
- End time.
- Applicable pricing configuration.

### Expected Behavior

The product calculates the price using deterministic pricing rules.

### Initial pricing factors

The pricing model may consider:

- Location.
- Vehicle type.
- Duration.
- Time.
- Day.
- Facilities or applicable parking characteristics.

### Outputs

The customer receives:

- Parking charge.
- Applicable platform/customer fee.
- Total payable amount.

### Business Rules

- Pricing must be deterministic.
- The customer must see the applicable price before confirming the reservation.
- The pricing result must be explainable through its applicable components.

### Out of Scope

- ML pricing.
- Personalized pricing.
- Demand prediction.
- Real-time demand-based pricing.

### Acceptance Criteria

- Valid inputs produce a price.
- The same inputs under the same pricing rules produce the same result.
- The customer can see the total before confirming the booking.

---

# 15. Reservation

## FR-08 — Create Booking

### Purpose

Allow a customer to reserve an available parking space.

### User

Customer

### Trigger

Customer confirms a reservation.

### Expected Behavior

The product must:

1. Validate the request.
2. Re-check availability.
3. Determine the applicable price.
4. Create the booking.
5. Assign a booking reference.
6. Return booking confirmation information.
7. Make the booking visible to the owner.

### Inputs

- Customer.
- Vehicle.
- Parking space.
- Start time.
- End time.
- Applicable price/payment information.

### Outputs

- Booking reference.
- Booking status.
- Parking information.
- Vehicle information.
- Time period.
- Price.

### Critical Business Rule

The product must prevent two customers from successfully booking the same parking space for overlapping periods.

A simple availability result shown earlier in the journey is not sufficient; the reservation operation must protect against the parking becoming unavailable between search and confirmation.

### Duplicate Request Rule

Repeated submission of the same customer reservation request must not unintentionally produce multiple successful bookings.

### Failure Conditions

A reservation fails when:

- Required information is invalid.
- Parking is unavailable.
- Vehicle is incompatible.
- The requested time is invalid.
- Price cannot be determined.
- The reservation cannot be successfully created.

### Acceptance Criteria

- A valid request creates a booking.
- The customer receives a booking reference.
- The owner can see the booking.
- A conflicting booking cannot also succeed.
- A failed reservation is not presented as confirmed.

---

# 16. Booking Management

## FR-09 — View Booking

### Purpose

Allow customers and owners to view reservations relevant to them.

### Customer

Can view their own bookings.

### Owner

Can view bookings for their parking spaces.

### Booking information

- Booking reference.
- Parking.
- Vehicle.
- Date/time.
- Price.
- Status.
- Relevant cancellation information.

### Acceptance Criteria

- Authorized users can view their bookings.
- Unauthorized users cannot view private booking information.
- Booking information reflects the current booking state.

---

# 17. Cancellation

## FR-10 — Cancel Booking

### Purpose

Allow eligible bookings to be cancelled.

### User

Customer

### Trigger

Customer requests cancellation.

### Expected Behavior

The product determines whether the booking can be cancelled and updates its state accordingly.

### Business Rules

The cancellation policy must define:

- Whether cancellation is allowed.
- Cancellation time window.
- Refund eligibility.
- Treatment of platform/service fees.
- Owner payout implications.

### Acceptance Criteria

- An eligible booking can be cancelled.
- An ineligible booking cannot be cancelled.
- A successfully cancelled booking is no longer treated as an active reservation.
- Applicable refund information is represented.

### Product Decision Status

Exact cancellation/refund rules remain a product decision to finalize before production payment processing.

---

# 18. Payment

## FR-11 — Payment Processing

### Purpose

Represent payment associated with a booking.

### User

Customer

### MVP

The MVP may use a mock/test payment flow.

This allows the project to learn payment state handling without making a paid payment provider a prerequisite.

### Payment Outcomes

At minimum:

- Payment initiated.
- Payment succeeded.
- Payment failed.

### Business Rules

- A successful payment must be associated with the appropriate booking.
- A failed payment must not be represented as successful.
- Payment state and booking state are distinct concepts.
- The final production flow must define how payment failure affects reservation confirmation.

### Important Failure Scenario

If a payment is reported as successful but the booking cannot ultimately be confirmed, the system must not present the booking as confirmed.

The eventual production product must define the appropriate recovery/refund behavior.

---

# 19. Marketplace Revenue

## FR-12 — Marketplace Fees

Peter Park Here will use a **hybrid revenue model**.

Potential revenue sources:

1. Transaction commission.
2. Customer service/platform fee.
3. Optional owner premium/subscription in a later phase.

### MVP

Exact percentages and amounts are not yet defined.

The product must therefore treat these as configurable business rules rather than embedding a particular percentage into the product requirements.

### Customer View

The customer should be able to distinguish:

```text
Parking charge
+ applicable platform/customer fee
= total payable amount
```

### Owner View

The owner should be able to understand the amount attributable to their booking after applicable marketplace charges.

---

# 20. Owner Availability

## FR-13 — Manage Availability

### Purpose

Allow owners to control when their parking can be booked.

### User

Owner

### Trigger

Owner creates or modifies availability.

### Expected Behavior

The owner can specify periods during which their parking can be booked.

### Business Rules

- Parking cannot be booked outside owner-defined availability.
- Future availability can be changed by the owner.
- Existing confirmed bookings must not be silently invalidated by an availability change.

### Acceptance Criteria

- Owner can define availability.
- Customers cannot reserve outside available periods.
- Existing bookings remain valid after subsequent availability changes unless explicitly cancelled under applicable rules.

---

# 21. Basic Booking Communication

## FR-14 — Booking Information

### Purpose

Ensure customers and owners know when important booking events occur.

### MVP Requirement

The product must make important booking information available to the relevant users.

Important events include:

- Booking created.
- Booking confirmed.
- Booking cancelled.
- Relevant payment outcome.

Complex notification infrastructure is not required for the first MVP.

---

# 22. Administration

## FR-15 — Basic Administration

### User

Administrator

### Purpose

Allow basic marketplace operations.

### Initial capabilities

- View users.
- View parking listings.
- View bookings.
- Manage listing status.
- Handle basic operational issues.

Advanced administration is outside MVP scope.

---

# 23. Core Business Rules

These rules apply across multiple requirements.

### BR-01 — No Double Booking

A parking space cannot have two successfully confirmed overlapping bookings.

### BR-02 — Availability Must Be Revalidated

A parking space that was available during search may become unavailable before booking. The final booking operation must therefore determine whether it is still available.

### BR-03 — Vehicle Compatibility

A vehicle must be compatible with the selected parking space.

### BR-04 — Active Listings Only

Only active/bookable listings can be reserved.

### BR-05 — Valid Time Period

The requested start time must precede the requested end time.

### BR-06 — Past Reservations

A customer cannot create a new reservation for a period that has already passed.

### BR-07 — Price Before Confirmation

The customer must know the applicable price before confirming a booking.

### BR-08 — Booking Access

Customers can access their own bookings.

Owners can access bookings associated with their parking.

### BR-09 — Listing Ownership

Only an authorized owner can modify their listing.

### BR-10 — Confirmed Bookings Are Not Silently Removed

Changing future availability must not silently invalidate an existing confirmed booking.

### BR-11 — Duplicate Submission

Repeated submission of the same booking action must not unintentionally create duplicate successful bookings.

---

# 24. User Stories

## Customer

### US-C01 — Find Parking

> As a customer, I want to search for parking near my destination so that I can find a suitable parking space.

### US-C02 — Evaluate Parking

> As a customer, I want to see parking details so that I can determine whether the space suits my needs.

### US-C03 — Check Availability

> As a customer, I want to check availability for a specific period so that I know whether I can reserve the space.

### US-C04 — Know Price

> As a customer, I want to see the applicable price before booking so that I can make an informed decision.

### US-C05 — Reserve Parking

> As a customer, I want to reserve available parking so that I know I have a parking space for my required period.

### US-C06 — View Booking

> As a customer, I want to view my booking so that I know the details of my reservation.

### US-C07 — Cancel Booking

> As a customer, I want to cancel an eligible booking so that I am not committed to a reservation I no longer need.

---

## Owner

### US-O01 — List Parking

> As a parking owner, I want to list my parking space so that customers can discover it.

### US-O02 — Control Availability

> As a parking owner, I want to define when my parking is available so that I retain control over its usage.

### US-O03 — Define Parking Information

> As a parking owner, I want to provide parking details and rules so that customers understand what I am offering.

### US-O04 — Receive Bookings

> As a parking owner, I want to see bookings for my parking so that I know when it is being used.

### US-O05 — Understand Earnings

> As a parking owner, I want to understand the amount attributable to my bookings so that I can evaluate the value of listing my parking.

---

# 25. Acceptance Criteria for the Core MVP

The following end-to-end scenarios represent the minimum product behavior.

## Scenario 1 — Successful Customer Reservation

**Given**

- An active parking listing exists.
- The parking supports the customer's vehicle.
- The requested period is within owner availability.
- No conflicting booking exists.

**When**

The customer selects the parking, requests the period, sees the price, and confirms the reservation.

**Then**

- The booking is successfully created.
- A booking reference is generated.
- The customer can view the booking.
- The owner can view the booking.

---

## Scenario 2 — Conflicting Reservation

**Given**

- A parking space already has a confirmed booking.
- A second customer requests an overlapping period.

**When**

The second customer attempts to reserve the space.

**Then**

- The second reservation does not succeed.
- The existing booking remains valid.
- The customer receives an appropriate unavailable result.

---

## Scenario 3 — Parking Becomes Unavailable

**Given**

A customer previously saw a parking space as available.

**When**

Another customer successfully reserves it before the first customer completes their reservation.

**Then**

The first customer's reservation attempt must not result in a second conflicting confirmed booking.

---

## Scenario 4 — Incompatible Vehicle

**Given**

A parking space does not support the customer's selected vehicle type.

**When**

The customer attempts to reserve it.

**Then**

The reservation is rejected.

---

## Scenario 5 — Invalid Time

**Given**

The requested start time is after or equal to the end time.

**When**

The customer searches or attempts to reserve.

**Then**

The request is rejected as invalid.

---

## Scenario 6 — Owner Availability

**Given**

An owner has made a parking space unavailable for a particular period.

**When**

A customer searches or attempts to reserve during that period.

**Then**

The parking cannot be successfully reserved for that period.

---

## Scenario 7 — Cancellation

**Given**

A booking satisfies the applicable cancellation policy.

**When**

The customer requests cancellation.

**Then**

The booking becomes cancelled and is no longer treated as an active reservation.

---

## Scenario 8 — Payment Failure

**Given**

The MVP payment simulation reports failure.

**When**

The customer attempts to complete the payment.

**Then**

The payment is represented as failed and the product must not represent the payment as successful.

---

# 26. Non-Functional Product Requirements

These are product expectations, not implementation prescriptions.

## Reliability

The product must not show a reservation as successfully confirmed when the reservation was not actually created.

## Security

The product must prevent unauthorized access to:

- User information.
- Bookings.
- Owner listings.
- Administrative functionality.

## Usability

The customer should be able to understand the core flow without knowledge of how the backend works.

## Performance

Search, availability, price calculation, and booking should provide a responsive user experience under expected MVP traffic.

Exact latency/throughput targets will be established during the HLD and performance-testing stages.

## Data Integrity

The product must maintain correct relationships between:

- Customers.
- Vehicles.
- Parking listings.
- Availability.
- Bookings.
- Payments.

---

# 27. Success Criteria

The first engineering MVP should demonstrate the following.

## Functional Success

A customer can:

> Search → evaluate → check availability → see price → reserve → view booking.

An owner can:

> Register → list parking → define availability → receive booking.

## Marketplace Success

The product can connect a customer to a valid parking space while preventing conflicting successful reservations.

## Learning Success

The MVP provides a mechanism to learn about:

- Customer demand.
- Pre-booking willingness.
- Owner willingness to list.
- Supply availability.
- Price acceptance.
- Marketplace trust.
- Marketplace liquidity.

These are hypotheses from the previous Assumptions & Validation document, not established facts.

---

# 28. Assumptions and Dependencies

## Product Assumptions

- Customers will value discovering parking before arrival.
- Customers will value knowing availability.
- Customers will accept pre-booking.
- Owners will be willing to list unused parking.
- Sufficient supply can be established in selected locations.
- Customers will accept the marketplace's pricing/fees.
- Owners will consider the resulting income worthwhile.

## Dependencies

The MVP depends on:

- User/account management.
- Parking listing data.
- Availability information.
- Pricing rules.
- Booking management.
- Eventually, payment processing.

The initial MVP should not depend on a production payment provider.

---

# 29. Decisions Not Yet Finalized

These are intentionally recorded instead of being invented by engineering.

### D-01 — Cancellation Policy

Need to finalize:

- Cancellation window.
- Refund percentage.
- Platform fee treatment.
- Owner payout treatment.
- No-show behavior.

### D-02 — Marketplace Fees

Need to finalize:

- Transaction commission.
- Customer service/platform fee.
- Owner premium/subscription conditions.

### D-03 — Vehicle Taxonomy

Need to finalize the exact supported vehicle categories.

### D-04 — Availability Model

Need to finalize whether MVP availability is:

- Individual date/time windows.
- Recurring schedules.
- Both.

### D-05 — Location Privacy

Need to finalize what location information is visible before and after booking.

### D-06 — Production Payment Behavior

Need to finalize the exact customer experience when:

- Payment succeeds but booking creation fails.
- Payment fails after an attempted booking.
- Booking is cancelled after payment.
- Owner cancels after payment.

These decisions will become particularly important during the payment/HLD stage.

---

# 30. Out of Scope for This PRD

The following belong to later engineering/design stages rather than this product document:

- PostgreSQL schema.
- MongoDB schema.
- Redis design.
- Kafka topics.
- REST endpoint design.
- Microservice boundaries.
- Distributed locking mechanism.
- Database locking strategy.
- Transaction isolation level.
- Cache invalidation strategy.
- Kubernetes architecture.
- AWS architecture.
- Java class design.
- Spring Boot package structure.
- API authentication mechanism.
- CI/CD implementation.

Those decisions will be derived from these product requirements during HLD and LLD.

---

# 31. Relationship to the Next Engineering Stage

This PRD is the bridge between product thinking and engineering design.

The sequence is:

```text
Problem Discovery
        ↓
Users & Jobs-to-Be-Done
        ↓
Value Proposition
        ↓
Business Model
        ↓
Assumptions & Validation
        ↓
MVP & Product Scope
        ↓
PRD
        ↓
High-Level Design
        ↓
Low-Level Design
        ↓
Implementation
```

The PRD answers:

> **What must Peter Park Here do?**

HLD will answer:

> **What system architecture can satisfy those requirements?**

LLD will answer:

> **How should the individual components/classes/modules implement that design?**

---

# 32. PRD Completion Criteria

This PRD is ready for HLD when:

- The target users are clear.
- The MVP goal is clear.
- Core journeys are clear.
- Functional requirements are defined.
- Important business rules are explicit.
- Critical success/failure behavior is defined.
- Acceptance criteria exist for the core journey.
- Non-goals are explicit.
- Product assumptions are documented.
- Unresolved business decisions are explicitly identified.
- Implementation choices have not been prematurely embedded in the product requirements.

The next document should therefore be the **High-Level Design (HLD)** for the MVP.