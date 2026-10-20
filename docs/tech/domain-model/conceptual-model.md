# Peter Park Here — Conceptual Domain Model

**Document Status:** Final Draft v1.0  
**Project:** Peter Park Here  
**Document Type:** Conceptual Domain Model

---

## 1. Purpose

The conceptual domain model identifies the important business concepts in Peter Park Here, explains how they relate to one another, and defines the rules governing their interactions.

Peter Park Here is a city parking marketplace where parking owners can make their spaces available for direct booking, accept customer offers without enabling direct booking, or choose not to participate in customer-facing offers.

Customers can discover parking, check availability, compare prices, reserve parking, make payments, and manage bookings. Depending on the owner's participation type, customers may also submit offers and wait for the owner's decision.

This document establishes the business domain before defining database schemas, Java classes, APIs, or service architecture.

## 2. Scope

The initial domain model covers:

- Users and their roles.
- Parking spaces and marketplace participation.
- Parking listings and owner-defined availability.
- Vehicles, facilities, parking rules, and pricing.
- Customer offers and owner decisions.
- Bookings and booking lifecycle.
- Payments, refunds, and payment failures.
- Notifications and administrative oversight.

Advanced features such as reviews, loyalty programs, promotional campaigns, machine-learning pricing, recurring reservations, and complex dispute management are outside the initial scope.

## 3. Domain Participants

### 3.1 User

A User represents a person registered on the platform.

A user may participate as a customer, a parking owner, or both. Customer and owner are roles rather than necessarily separate user identities.

### 3.2 Customer

A Customer uses the platform to:

- Discover parking spaces.
- Review parking details, facilities, and rules.
- Check availability and calculate prices.
- Submit offers where the owner permits them.
- Create and manage bookings.
- Make payments and request eligible refunds.
- Receive booking and offer notifications.

### 3.3 Parking Owner

A Parking Owner controls the parking spaces they are authorized to manage.

The owner can:

- Register parking spaces.
- Choose how each space participates in the marketplace.
- Define availability, pricing, facilities, and rules.
- Review customer offers and accept or reject them.
- Manage listings and bookings.
- Respond to cancellation and refund-related requests where applicable.

### 3.4 Administrator

An Administrator oversees platform operations, including user and listing management, reported issues, and exceptional booking or payment cases.

Administrative actions must respect appropriate authorization and audit requirements.

## 4. Parking Participation Types

Peter Park Here supports three participation types in the initial version.

### Type 1 — Listed for Direct Booking

The owner publishes a parking listing and allows customers to book it directly when the requested time is available.

The owner defines the available times, pricing, facilities, and parking rules.

Customers can search for the space, check availability, calculate the price, and proceed through the booking and payment flow.

### Type 2 — Not Listed for Direct Booking, but Open to Offers

The owner does not offer direct booking for the space but allows customers to submit offers.

Customers can see that the space is not available for normal direct booking and can submit a proposed price and requested time.

The owner reviews the offer and decides whether to accept or reject it. Acceptance does not, by itself, guarantee a confirmed booking; the system must complete the required availability, booking, and payment steps.

### Type 3 — Not Listed and Not Accepting Offers

The owner does not offer the space for direct booking and does not accept customer offers through the platform.

The space cannot be booked or offered for through the normal customer workflow.

### Participation rules

- A parking space has one participation type at a time.
- The owner must be authorized to change the participation type.
- An inactive or unlisted space is not eligible for direct booking.
- An offer can be submitted only when the owner's participation settings permit it.
- Changing participation settings must not silently invalidate an existing confirmed booking.
- The platform can manage these three types only for spaces known to it. A space never registered or otherwise recorded on the platform cannot automatically be presented as an owner-managed space.

For the MVP, one parking listing represents one independently bookable parking space. Support for multiple independently bookable spaces under a single listing can be introduced in a future version.

## 5. Core Domain Concepts

### 5.1 User

Represents a registered platform user.

**Important information:**
- User identifier
- Name and contact details
- Account status
- Roles
- Registration details

A user may own parking spaces and book other spaces as a customer.

### 5.2 Parking Space

Represents the physical parking resource.

**Important information:**
- Parking space identifier
- Owner
- Location
- Vehicle compatibility
- Physical characteristics
- Ownership or management authorization

The parking space is the physical resource; its marketplace participation determines how customers can interact with it.

### 5.3 Parking Listing

Represents the marketplace representation of one independently bookable parking space.

**Important information:**
- Listing identifier
- Associated parking space
- Owner
- Participation type
- Publication or listing status
- Listing description
- Facilities
- Parking rules
- Pricing configuration

A listing may be published for direct booking, unlisted but open to offers, or unlisted and closed to offers.

### 5.4 Availability

Represents the times during which the owner permits the parking space to be used.

**Important information:**
- Parking space or listing
- Available time windows
- Owner-defined restrictions
- Exceptions or blocked periods

Availability is defined by the owner. A customer request must be checked against the applicable availability rules and existing bookings.

### 5.5 Vehicle

Represents a vehicle registered by a customer.

**Important information:**
- Vehicle identifier
- Customer
- Vehicle category
- Registration number, where required
- Relevant vehicle characteristics

The vehicle category can affect parking compatibility and pricing.

### 5.6 Pricing Rule

Represents the deterministic rules used to calculate the price of parking.

**Potential pricing factors:**
- Location
- Vehicle category
- Duration
- Facilities
- Day or time period
- Owner-defined pricing conditions

The initial implementation uses explicit rules rather than machine-learning-based pricing.

The calculated price and agreed terms must be preserved with the resulting booking so that subsequent pricing changes do not alter an existing reservation.

### 5.7 Offer

Represents a customer's request to use an offer-enabled parking space under proposed terms.

**Important information:**
- Offer identifier
- Customer
- Parking listing or space
- Requested start and end time
- Proposed price
- Submission time
- Owner response deadline
- Offer status
- Owner decision and decision time
- Customer withdrawal or refund-request details, where applicable

An offer is a request for the owner's decision, not a confirmed reservation.

### 5.8 Booking

Represents a reservation for a parking space during a specified period.

**Important information:**
- Booking identifier
- Customer
- Parking space or listing
- Vehicle
- Start and end time
- Agreed price and terms
- Booking status
- Confirmation time
- Cancellation details, where applicable

A booking must satisfy availability, authorization, and other applicable business rules before it is confirmed.

### 5.9 Payment

Represents a financial transaction associated with a booking.

**Important information:**
- Payment identifier
- Booking reference
- Amount and currency
- Payment status
- Payment provider reference
- Transaction timestamps
- Idempotency reference, where applicable

Payment status and booking status are separate. A successful payment does not automatically prove that the booking was confirmed.

### 5.10 Refund

Represents a return of money associated with an eligible payment.

**Important information:**
- Refund identifier
- Original payment
- Amount
- Reason
- Refund status
- Request and processing timestamps
- Payment provider reference

Refunds may be initiated under applicable cancellation rules or when a paid booking cannot be confirmed within the defined recovery window.

### 5.11 Notification

Represents a message sent to a user about an event or required action.

Examples include:
- Offer submitted or accepted.
- Owner response overdue.
- Booking confirmed or cancelled.
- Payment succeeded or failed.
- Refund requested, processing, or completed.

A notification informs a user; it does not itself change the authoritative business state.

## 6. Relationships Between Concepts

The main relationships are:

- A User can act as a customer, an owner, or both.
- A Parking Owner can manage multiple Parking Spaces.
- A Parking Space has one active marketplace participation configuration at a time.
- A Parking Listing represents one independently bookable Parking Space in the MVP.
- A Parking Listing has owner-defined Availability, applicable Pricing Rules, facilities, and parking rules.
- A Customer can register multiple Vehicles.
- A Customer can submit multiple Offers where permitted.
- An Offer relates to one customer, one target parking space, and one requested time window.
- An Offer can lead to a Booking after acceptance and completion of the required steps.
- A Booking relates to one customer, one parking space, and the vehicle intended for use.
- A Booking can have associated Payment records and, where applicable, Refund records.
- A User can receive multiple Notifications.

The model intentionally separates the physical parking space, its marketplace representation, the offer, the booking, and the financial transaction. They represent different business responsibilities and have different lifecycles.

## 7. Business Workflows and Lifecycles

### 7.1 Direct Booking Workflow

1. The customer searches for parking.
2. The customer selects a directly bookable listing.
3. The customer selects a vehicle and requested time.
4. The system validates vehicle compatibility and availability.
5. The pricing rules calculate the applicable price.
6. The customer proceeds with the booking and payment process.
7. The system confirms the booking when the required conditions are satisfied.
8. The customer receives confirmation.

The booking process must prevent two customers from obtaining confirmed reservations for overlapping periods on the same independently bookable space.

### 7.2 Offer Workflow

1. The customer discovers a space that is not open for direct booking but accepts offers.
2. The customer submits a proposed price and requested time.
3. The offer enters **Awaiting Owner Decision**.
4. The owner accepts or rejects the offer.
5. If accepted, the system revalidates availability and finalizes the agreed terms.
6. The customer completes the required booking and payment steps.
7. The booking is confirmed only after the applicable confirmation conditions are satisfied.

If the owner rejects the offer, the offer ends without creating a confirmed booking.

### 7.3 Offer Lifecycle

An offer may move through the following states:

- **Submitted:** The customer has submitted the offer.
- **Awaiting Owner Decision:** The owner has not yet decided.
- **Accepted:** The owner accepts the proposed terms.
- **Rejected:** The owner declines the offer.
- **Expired:** The offer reaches its deadline without an accepted decision.
- **Withdrawn:** The customer withdraws the offer, where withdrawal is permitted.

The system must prevent conflicting transitions, such as accepting an offer that has already expired or been withdrawn.

Acceptance does not bypass availability checks or guarantee that payment and booking confirmation have succeeded.

### 7.4 Delayed Owner Response

When an owner has not responded within the configured response window, the platform notifies the customer and offers the applicable choices.

The customer may be allowed to:
- Continue waiting while the offer remains eligible.
- Withdraw the offer and request a refund if money has already been collected and the applicable conditions permit it.

The response window, offer expiry, and refund eligibility must be explicitly defined.

A delayed response must not automatically cancel a valid confirmed booking. The system must first establish the authoritative booking state before initiating a refund.

### 7.5 Booking Lifecycle

The initial booking states are:

- **Pending:** The booking is being processed and is not yet confirmed.
- **Confirmed:** The reservation has been successfully established.
- **Cancelled:** The reservation has been cancelled under the applicable rules.
- **Completed:** The reservation period has ended and the booking has been completed.

A pending booking may also expire or fail. These outcomes must be represented explicitly in the implementation if they are needed to distinguish failed processing from customer cancellation.

Only permitted transitions are allowed. For example, a cancelled booking cannot become confirmed merely because a delayed payment callback arrives.

### 7.6 Payment and Booking Confirmation Failure

Payment and booking confirmation are separate processes and must be reconciled.

If payment succeeds but the booking confirmation process fails or the host does not receive a timely response:

1. The system records the payment result.
2. It checks the authoritative booking state rather than assuming confirmation failed.
3. It retries or reconciles the operation within a defined recovery window.
4. If the booking cannot be confirmed within that window, the system makes the applicable refund option available or initiates the refund according to the agreed policy.
5. The customer is notified of the outcome.

Duplicate payment callbacks and retry attempts must not create duplicate bookings or duplicate refunds.

### 7.7 Refund Lifecycle

A refund may move through these states:

- **Requested:** A refund has been requested or initiated under an applicable rule.
- **Processing:** The refund is being processed.
- **Succeeded:** The refund has been completed successfully.
- **Failed:** The refund attempt failed and requires appropriate retry or exception handling.

A refund request is not the same as a completed refund. The system must track the provider's result and communicate the actual outcome.

The refund amount, cancellation eligibility, and timing depend on the applicable policy and the original payment.

## 8. Core Business Rules and Invariants

### Parking and participation

1. Every managed parking space must have an identifiable owner or authorized manager.
2. Only authorized owners or administrators may change participation settings.
3. A space closed to direct booking cannot be booked through the direct-booking workflow.
4. Offers can be submitted only for spaces whose participation settings permit offers.
5. In the MVP, one listing represents one independently bookable space.
6. Participation changes must not silently invalidate confirmed bookings.

### Availability and booking

7. The owner defines the permitted availability windows and restrictions.
8. A booking must pass the applicable availability and compatibility checks.
9. Two confirmed bookings must not overlap for the same independently bookable space.
10. Availability must be revalidated when finalizing an accepted offer.
11. Booking confirmation must be protected against concurrent requests, retries, and duplicate processing.
12. The agreed price and terms must be preserved with the booking.

### Offers

13. An offer is not a confirmed booking.
14. An offer must include the requested time window and proposed price.
15. Only valid offers can be accepted.
16. Owner acceptance must not bypass availability validation.
17. The system must enforce offer deadlines and valid state transitions.
18. A delayed owner response must not automatically imply that a booking has failed.
19. Customer withdrawal and refund eligibility must follow the applicable rules.

### Payments and refunds

20. Payment status and booking status must be tracked independently.
21. A successful payment must not be treated as proof of booking confirmation.
22. The system must reconcile uncertain booking outcomes before refunding a potentially valid reservation.
23. Payment retries and callbacks must be idempotent.
24. A refund must reference the original payment and follow the applicable eligibility rules.
25. Refund processing must be tracked until a final outcome is established.

### Security and ownership

26. Customers may manage only their own vehicles, offers, bookings, and eligible refund requests.
27. Owners may manage only parking spaces they are authorized to control.
28. Sensitive payment information must be handled according to the payment provider's security requirements.
29. Important state changes should be auditable.

## 9. Preliminary Domain Capability Boundaries

The domain suggests the following business capability boundaries. These are logical responsibilities, not a commitment to create separate microservices.

| Capability | Responsibility |
|---|---|
| User and Access Management | Accounts, roles, authentication, and authorization |
| Parking Management | Parking spaces, listings, participation settings, facilities, and rules |
| Availability | Owner schedules, restrictions, and booking-related availability checks |
| Search | Discovering and filtering parking spaces |
| Pricing | Deterministic price calculation |
| Offers | Customer proposals, owner decisions, deadlines, and withdrawal |
| Booking | Reservation lifecycle, confirmation, cancellation, and concurrency protection |
| Payments and Refunds | Payment tracking, reconciliation, refunds, and transaction idempotency |
| Notifications | Customer and owner communications |
| Administration | Operational oversight and exceptional-case management |

The initial implementation may keep these capabilities in one modular backend. Separate deployment units should be introduced only when justified by scaling, reliability, organizational, or other architectural requirements.

## 10. Assumptions and Decisions to Finalize Before Implementation

The following details remain policy or implementation decisions rather than unresolved domain concepts.

| Decision | Required clarification |
|---|---|
| Offer response deadline | How long should an owner have to respond? |
| Offer expiry | Does an unanswered offer expire automatically, and when? |
| Customer waiting option | How long can the customer continue waiting after the response deadline? |
| Payment timing | At what point is money collected for an offer-based reservation? |
| Confirmation recovery window | How long should the system retry and reconcile before presenting a refund option? |
| Cancellation and refunds | What are the cancellation windows, refund percentages, and exceptional cases? |
| Booking expiry | How long can a pending booking remain pending? |
| Participation changes | What happens to outstanding offers when an owner changes participation type? |
| Owner changes to availability | How should changes affect outstanding offers and confirmed bookings? |
| Vehicle categories | Which vehicle types and compatibility rules are supported in the MVP? |

These decisions should be documented before implementation of the corresponding workflows. Time windows and refund amounts should be configurable business policies where practical rather than scattered hard-coded values.

## 11. Explicitly Out of Scope for the MVP

The initial conceptual model does not require:

- Multiple independently bookable spaces under a single listing.
- Counteroffers and multi-round negotiation.
- Machine-learning-based pricing.
- Complex recurring reservations.
- Loyalty programs and promotions.
- Advanced dispute resolution.
- Multi-party settlement and sophisticated owner payout arrangements.

These features can be introduced later without combining the responsibilities of offers, bookings, payments, and refunds.

## 12. Relationship to Subsequent Technical Documents

This conceptual domain model defines the business vocabulary and rules. It does not prescribe the database schema, Java classes, APIs, or deployment architecture.

The next technical documents should be developed in this order:

1. **Domain Design / LLD:** Define domain objects, responsibilities, interfaces, invariants, and state transitions.
2. **High-Level Design (HLD):** Define modules, interactions, APIs, reliability requirements, and architectural boundaries.
3. **Database Design:** Map the domain to PostgreSQL and, where justified, other data stores.
4. **API Design:** Define request/response contracts, validation, error handling, and idempotency.
5. **Testing Strategy:** Define unit, integration, API, concurrency, and failure-recovery tests.

The design should evolve incrementally: build, test, understand, document, and then proceed to the next stage.

## 13. Completion Criteria

The conceptual domain model is ready for technical design when:

- The three parking participation types are unambiguous.
- Direct booking and offer-based booking are distinguishable.
- Offer, booking, payment, and refund lifecycles are separate.
- Owner response deadlines and customer withdrawal behavior are defined.
- Availability and double-booking invariants are explicit.
- Payment/booking reconciliation and refund behavior are understood.
- The business terminology is consistent across the PRD, business workflows, and technical documentation.

**End of Document**
