# Peter Park Here — Technical Domain Model

**Document Status:** Draft v1.0  
**Project:** Peter Park Here  
**Document Type:** Technical Domain Design  
**Implementation Direction:** Java, Spring Boot, modular monolith  
**Prerequisite:** Conceptual Domain Model

---

## 1. Purpose

The technical domain model defines how Peter Park Here's business concepts should be represented and protected in the backend application.

The conceptual domain model identified the business concepts and their relationships. This document takes the next step by defining:

- Domain entities and their identities.
- Aggregate roots and consistency boundaries.
- Value objects and domain behavior.
- Business invariants and state transitions.
- Repository interfaces and domain services.
- Application use cases and domain events.
- Concurrency and transaction responsibilities.
- Logical package organization.

The goal is to create a domain model that is understandable, testable, maintainable, and capable of evolving as the marketplace grows.

This document does not define the final database schema, REST API contracts, deployment architecture, or complete Java implementation.

## 2. Technical Design Principles

### 2.1 Start with a modular monolith

The MVP should be implemented as one Spring Boot application with clearly separated business modules.

The initial logical modules are:

- User and Access
- Parking Management
- Availability
- Search
- Pricing
- Offers
- Booking
- Payments and Refunds
- Notifications

These modules do not need to become separate deployable services. They can communicate through explicit interfaces and, where appropriate, internal domain events.

**Reason:** The project needs to establish correct business behavior and consistency before introducing the operational complexity of distributed services.

### 2.2 Keep business rules separate from infrastructure

The domain model should not depend directly on:

- Spring MVC controllers
- HTTP request or response classes
- PostgreSQL or MongoDB APIs
- Payment-provider SDKs
- Kafka clients
- Redis clients

Infrastructure and application code can use these technologies to support the domain, but the core business rules should remain independently testable.

### 2.3 Protect invariants at the correct boundary

An invariant is a rule that must remain true whenever a business operation completes successfully.

Examples:

- An offer cannot be accepted after it has expired.
- A confirmed booking must satisfy the required booking conditions.
- Two confirmed bookings cannot overlap on the same independently bookable space.
- A refund cannot exceed the refundable amount of the original payment.

Different invariants belong to different parts of the model. A booking's lifecycle rules belong in the Booking domain model; preventing overlapping bookings also requires coordinated persistence and transaction handling.

### 2.4 Separate domain state from technical representations

The following are different concerns:

- `Booking` — the domain object that represents a reservation.
- `BookingEntity` — a persistence representation, if a separate one is needed.
- `BookingResponse` — an API representation returned to a client.

They may contain some of the same information, but they have different responsibilities. They should not be treated as interchangeable merely because their fields overlap.

---

## 3. Proposed Domain Modules

| Module | Main responsibility |
|---|---|
| `user` | User identity, roles, and account status |
| `parking` | Parking-space details and marketplace participation |
| `availability` | Owner-defined schedules, restrictions, and availability checks |
| `search` | Discovering and filtering parking |
| `pricing` | Deterministic price calculation |
| `offer` | Customer proposals and owner decisions |
| `booking` | Reservations, lifecycle, and booking consistency |
| `payment` | Payments, reconciliation, and refunds |
| `notification` | Notifications to customers and owners |

These are logical boundaries. Search, notifications, and some supporting functionality may initially be simple application services rather than complex domain models.

## 4. Entities and Aggregate Roots

An entity has a stable identity that distinguishes it from other instances. An aggregate is a consistency boundary containing an aggregate root and the objects it controls.

For the MVP, the following aggregate roots are proposed.

### 4.1 User Aggregate

**Aggregate root:** `User`

Represents a registered platform user.

Possible domain state:

- `UserId`
- Name and contact details
- Account status
- Assigned roles

Possible behavior:

- `activate()`
- `deactivate()`
- `assignRole(role)`, subject to authorization rules

A user may be both a customer and a parking owner. We should not create separate customer and owner identities unless their business requirements genuinely differ.

**Boundary decision:** The User aggregate should not own every vehicle, parking space, booking, or payment belonging to the user. These concepts have their own lifecycles and consistency requirements.

### 4.2 Parking Aggregate

**Aggregate root:** `ParkingListing`

For the MVP, `ParkingListing` represents one independently bookable parking space. It contains the marketplace-facing details needed to manage that space.

Possible state:

- `ListingId`
- `OwnerId`
- Location
- Vehicle compatibility
- Facilities and parking rules
- `MarketplaceMode`
- Listing details and status

The `MarketplaceMode` enum should have three values:

- `DIRECT_BOOKING` — the space is offered for direct booking.
- `OFFERS_ONLY` — the space is not directly bookable but can receive customer offers.
- `CLOSED` — the space is not available for direct booking or offers.

The term `OFFERS_ONLY` means that the space may be discoverable through an appropriate marketplace view; it does not mean the owner has published a normal direct-booking listing.

Possible behavior:

- `enableDirectBooking()`
- `enableOffersOnly()`
- `closeMarketplaceParticipation()`
- `updateFacilities(...)`
- `updateParkingRules(...)`
- `updateLocation(...)`, subject to applicable restrictions

These methods must validate whether the requested transition is allowed and whether the caller is authorized. Authorization is enforced by the application/security layer, with ownership information checked against the authenticated user.

**Important distinction:** The physical parking resource and its marketplace participation are conceptually different. For the MVP, one aggregate can represent them together to reduce unnecessary complexity. If multiple spaces are later managed under one listing, the model can be refactored to separate a parking facility, physical spaces, and marketplace listings.

### 4.3 Availability Model

Availability represents the owner's permitted time windows and restrictions.

The initial design can use an `AvailabilityPolicy` or equivalent domain object associated with a parking listing.

Possible information:

- Owner-defined operating windows
- Blocked periods
- Date-specific exceptions
- Relevant restrictions

Possible behavior:

- `isWithinOwnerAvailability(timeRange)`
- `isBlocked(timeRange)`
- `canBeRequested(timeRange)`

This model alone cannot guarantee that a space is available. Existing bookings and concurrent reservation attempts must also be considered.

**Boundary decision:** Availability rules can be maintained by the Parking/Availability module, while Booking remains responsible for establishing the reservation. The authoritative availability check must coordinate with booking persistence to prevent conflicting reservations.

### 4.4 Vehicle Entity

A `Vehicle` has its own identity and belongs to a user.

Possible state:

- `VehicleId`
- `OwnerUserId`
- `VehicleCategory`
- Registration details, where required

A vehicle is managed independently from a booking because the same vehicle may be used for many bookings.

The Booking aggregate should store the relevant `VehicleId` and validate that the customer is authorized to use that vehicle.

For historical consistency, the booking may also retain the relevant vehicle category or other agreed characteristics used to validate the reservation.

### 4.5 Offer Aggregate

**Aggregate root:** `Offer`

Represents a customer's proposal to use an offer-enabled parking space at a proposed price and time.

Possible state:

- `OfferId`
- `CustomerId`
- `ListingId`
- Requested time range
- Proposed `Money`
- Submission time
- Response deadline
- Offer status
- Owner decision details

Proposed status enum:

- `AWAITING_OWNER_DECISION`
- `ACCEPTED`
- `REJECTED`
- `EXPIRED`
- `WITHDRAWN`

Possible behavior:

- `accept(ownerId, decisionTime)`
- `reject(ownerId, decisionTime)`
- `expire(currentTime)`
- `withdraw(customerId, currentTime)`

Each operation must verify the current state and enforce permitted transitions.

For example, `accept(...)` must reject an expired, withdrawn, or previously rejected offer. The application layer must also verify owner authorization and that the listing permits offers.

The Offer aggregate should not directly create and confirm a booking. Acceptance records the owner's decision; the application workflow subsequently rechecks availability and coordinates booking and payment.

### 4.6 Booking Aggregate

**Aggregate root:** `Booking`

Represents a reservation for one independently bookable parking space.

Possible state:

- `BookingId`
- `CustomerId`
- `ListingId`
- `VehicleId`
- Requested time range
- Agreed `Money`
- Booking status
- Confirmation timestamp
- Cancellation details

Proposed booking states:

- `PENDING`
- `CONFIRMED`
- `CANCELLED`
- `COMPLETED`
- `EXPIRED`, if required to represent an expired pending reservation
- `FAILED`, if required to distinguish failed processing from cancellation

Possible behavior:

- `confirm(...)`
- `cancel(...)`
- `complete(...)`
- `expire(...)`
- `markFailed(...)`

The final set of states should be limited to outcomes that the business and implementation genuinely need to distinguish.

The Booking aggregate should enforce local lifecycle rules. It should not independently decide payment-provider outcomes or issue refunds.

#### Booking consistency boundary

The following rule is critical:

**Two confirmed bookings must never overlap on the same independently bookable parking space.**

A Java method that checks availability and then creates a booking is not enough. Two concurrent requests could both pass the check before either transaction commits.

The implementation must combine the domain rule with an appropriate persistence-level concurrency strategy, such as:

- A PostgreSQL exclusion constraint for overlapping time ranges, where the chosen schema supports it.
- A transaction and locking strategy that serializes conflicting reservation attempts.
- A database-backed reservation mechanism with suitable uniqueness and concurrency controls.

The final choice belongs in the database design and HLD. The database must remain the final authority for preventing conflicting confirmed reservations.

### 4.7 Payment Aggregate

**Aggregate root:** `Payment`

Represents a payment attempt or transaction associated with a booking.

Possible state:

- `PaymentId`
- `BookingId`
- Amount and currency
- Payment status
- Provider reference
- Idempotency reference
- Transaction timestamps

Possible statuses include:

- `INITIATED`
- `PENDING`
- `SUCCEEDED`
- `FAILED`
- `REFUND_PENDING`
- `PARTIALLY_REFUNDED`
- `REFUNDED`

The final state model may separate payment outcome from refund outcome rather than putting every financial condition into one enum. The important requirement is that payment and refund processing remain distinguishable.

Possible behavior:

- `markPending(...)`
- `markSucceeded(...)`
- `markFailed(...)`

A Payment aggregate records verified payment outcomes. It should not trust a browser redirect or an unverified client request as proof that money was collected.

### 4.8 Refund Aggregate

**Aggregate root:** `Refund`, if refund processing becomes sufficiently complex to warrant an independent lifecycle.

Possible state:

- `RefundId`
- Original `PaymentId`
- Amount and currency
- Reason
- Refund status
- Provider reference
- Processing timestamps

Possible statuses:

- `REQUESTED`
- `PROCESSING`
- `SUCCEEDED`
- `FAILED`

The model must support partial refunds only if the business policy permits them. A refund must reference the original payment, and cumulative refunds must not exceed the eligible refundable amount.

For the earliest MVP, refund behavior could initially be coordinated by a Payment application service, with a dedicated Refund aggregate introduced when its lifecycle justifies it.

---

## 5. Value Objects

A value object represents a value defined by its attributes rather than a separate identity. Value objects should be immutable where practical and validate their own construction.

| Value object | Responsibility |
|---|---|
| `UserId` | Strongly typed user identifier |
| `ListingId` | Strongly typed parking listing identifier |
| `OfferId` | Strongly typed offer identifier |
| `BookingId` | Strongly typed booking identifier |
| `PaymentId` | Strongly typed payment identifier |
| `Money` | Amount and currency, with valid arithmetic rules |
| `TimeRange` | Start and end timestamps, with start before end |
| `GeoLocation` | Coordinates and applicable validation |
| `VehicleCategory` | Supported vehicle category |
| `PricingBreakdown` | Base amount and applicable price components |
| `OfferDecision` | Decision outcome and decision timestamp, if useful |
| `RefundAmount` | Refund amount constrained by applicable financial rules |

For example, `TimeRange` should reject a range whose end is not after its start. `Money` should avoid floating-point arithmetic for financial calculations.

Strongly typed identifiers are optional in the first implementation, but they can prevent accidentally passing a `UserId` where a `BookingId` is expected.

---

## 6. Domain Services

A domain service holds business logic that does not naturally belong to one entity or aggregate.

### 6.1 PricingService

Responsibilities:

- Calculate a price using deterministic pricing rules.
- Apply duration, vehicle category, location, facilities, and time/day factors where configured.
- Return a validated `PricingBreakdown`.

The service should be deterministic for the same inputs and pricing configuration.

The actual pricing policy may later use the Strategy pattern if multiple pricing strategies are needed. The pattern should not be introduced merely for demonstration.

### 6.2 AvailabilityChecker

Responsibilities:

- Evaluate owner-defined availability.
- Check blocked periods and restrictions.
- Coordinate with the booking workflow to determine whether a requested period can be reserved.

The checker provides a decision, but final booking consistency must be enforced by the transaction and persistence mechanism.

### 6.3 BookingPolicy

Responsibilities:

- Validate applicable booking conditions.
- Validate permitted booking transitions.
- Determine whether cancellation is allowed under the relevant policy.

A policy object is useful when rules become sufficiently complex. Simple rules may remain directly in the Booking aggregate.

### 6.4 RefundEligibilityPolicy

Responsibilities:

- Evaluate whether a payment is eligible for a refund.
- Apply cancellation and failed-confirmation policies.
- Calculate the eligible refund amount.

This policy must not independently mark a refund as successful. Refund success is established from the verified payment-provider outcome.

---

## 7. Application Services and Use Cases

Application services coordinate a business use case. They call domain behavior, repositories, and infrastructure adapters without taking ownership of every business rule.

Examples include:

| Application service | Main use case |
|---|---|
| `CreateParkingListingService` | Register a space and its participation settings |
| `SearchParkingService` | Find suitable spaces |
| `CalculateParkingPriceService` | Calculate a quote |
| `SubmitOfferService` | Submit a customer offer |
| `DecideOfferService` | Accept or reject an offer |
| `CreateBookingService` | Validate and establish a booking |
| `CancelBookingService` | Apply cancellation rules |
| `ProcessPaymentService` | Coordinate a payment attempt |
| `ReconcileBookingPaymentService` | Resolve uncertain payment and confirmation outcomes |
| `RequestRefundService` | Initiate an eligible refund |
| `ProcessRefundResultService` | Record and reconcile the refund result |

For example, `CreateBookingService` may coordinate the following:

1. Validate the customer's authorization and request.
2. Load the required listing and booking-related data.
3. Check the applicable time, vehicle, and pricing rules.
4. Create the booking through the domain model.
5. Use a transaction and concurrency strategy to persist the reservation safely.
6. Initiate the next required workflow step.
7. Return the outcome through the API layer.

The application service coordinates the operation; the domain model protects the relevant business invariants.

---

## 8. Repository Interfaces

Repositories provide a domain-oriented way to retrieve and persist aggregate roots.

Proposed interfaces include:

- `UserRepository`
- `ParkingListingRepository`
- `OfferRepository`
- `BookingRepository`
- `PaymentRepository`
- `RefundRepository`, if a separate Refund aggregate is used

Illustrative operations:

```java
public interface BookingRepository {
    Optional<Booking> findById(BookingId bookingId);

    void save(Booking booking);
}
```

This is illustrative pseudocode for the interface shape, not a final implementation contract. A real repository may require additional operations for transactions, idempotency, locking, or reservation conflicts.

Repository interfaces should not expose database-specific implementation details unnecessarily. Their implementations belong in infrastructure.

For example, PostgreSQL-specific locking and exclusion constraints belong in the persistence adapter and database schema, not inside the domain entity.

---

## 9. Domain Events

A domain event records something meaningful that has happened in the business domain.

Proposed events include:

| Event | Meaning |
|---|---|
| `ParkingListingCreated` | A parking space has been registered |
| `ParkingParticipationChanged` | Its marketplace participation mode has changed |
| `OfferSubmitted` | A customer submitted an offer |
| `OfferAccepted` | The owner accepted an offer |
| `OfferRejected` | The owner rejected an offer |
| `OfferExpired` | An offer reached its expiry condition |
| `BookingCreated` | A booking was created |
| `BookingConfirmed` | A booking was confirmed |
| `BookingCancelled` | A booking was cancelled |
| `PaymentSucceeded` | A payment was verified as successful |
| `PaymentFailed` | A payment failed |
| `RefundRequested` | A refund was requested or initiated |
| `RefundSucceeded` | A refund completed successfully |
| `RefundFailed` | A refund attempt failed |

Events should represent facts that have already occurred, not commands asking another component to do something.

For example, `BookingConfirmed` is a fact. `ConfirmBooking` is a command or operation.

In the initial modular monolith, events can be handled internally. Kafka should not be required just to introduce domain events. If reliable asynchronous delivery becomes necessary, the architecture can later add a transactional outbox and message broker.

Event publication must be coordinated with persistence so that the system does not announce a successful business operation that was rolled back.

---

## 10. Critical Workflow: Offer Acceptance

The Offer aggregate and Booking aggregate have separate responsibilities.

A proposed flow is:

1. `DecideOfferService` verifies that the owner is authorized.
2. The Offer aggregate validates the current state and records acceptance.
3. The application workflow rechecks the listing's participation settings and requested availability.
4. If the space is still eligible, the booking workflow attempts to establish the reservation.
5. The customer proceeds through the required booking and payment steps.
6. The booking is confirmed only after its confirmation conditions are met.
7. The customer and owner receive the appropriate notifications.

If the space is no longer available, the workflow must not create a conflicting booking. The accepted offer and the unsuccessful reservation attempt must be represented accurately rather than silently treating the offer as a confirmed booking.

The precise policy for an accepted offer that cannot become a booking must be finalized before implementation.

---

## 11. Critical Workflow: Payment Succeeds but Booking Confirmation Fails

The design must handle this failure without assuming that a timeout means the booking was not confirmed.

The workflow should:

1. Record or verify the payment result.
2. Check the authoritative booking state.
3. Retry or reconcile any uncertain confirmation operation.
4. If confirmation cannot be recovered within the configured recovery window, make the applicable refund option available or initiate a refund according to the agreed policy.
5. Track the refund until the provider confirms success or failure.
6. Notify the customer of the actual outcome.

This workflow requires idempotency and durable records. Retrying a request must not create a duplicate booking, charge, or refund.

The Booking and Payment aggregates remain separate because they have different lifecycles. Their coordination belongs to the application workflow and, if the system later becomes distributed, an appropriate orchestration or saga mechanism.

---

## 12. Concurrency and Transaction Responsibilities

The technical domain model identifies the rules, while the database and application architecture enforce them under concurrent execution.

| Problem | Domain responsibility | Technical enforcement |
|---|---|---|
| Double booking | A confirmed booking must not overlap another confirmed booking for the same space | Database constraint or transaction/locking strategy |
| Duplicate booking request | The same logical request must not create multiple bookings | Idempotency key and durable request handling |
| Concurrent offer decisions | Only a valid current offer can be accepted or rejected | State validation and concurrency control |
| Duplicate payment callback | A payment outcome must not be applied repeatedly | Provider reference, idempotent processing, durable state |
| Duplicate refund request | The same eligible amount must not be refunded multiple times | Idempotency and refund constraints |
| Owner changes participation | Changes must respect existing commitments | Authorization, state validation, transaction coordination |
| Delayed host response | Unknown outcomes must be reconciled before refunding | Durable workflow state and retry/reconciliation policy |

Java synchronization alone is insufficient for cross-request or multi-instance concurrency. Correctness must survive application restarts, retries, and multiple backend instances.

---

## 13. Proposed Java Package Structure

A domain-oriented structure is recommended. The following is a proposed organization, not a requirement to create every package immediately.

```text
com.peterparkhere
├── user
│   ├── domain
│   ├── application
│   └── infrastructure
├── parking
│   ├── domain
│   ├── application
│   └── infrastructure
├── availability
│   ├── domain
│   ├── application
│   └── infrastructure
├── search
│   ├── application
│   └── infrastructure
├── pricing
│   ├── domain
│   └── application
├── offer
│   ├── domain
│   ├── application
│   └── infrastructure
├── booking
│   ├── domain
│   ├── application
│   └── infrastructure
├── payment
│   ├── domain
│   ├── application
│   └── infrastructure
└── notification
    ├── application
    └── infrastructure
```

The API controllers and request/response DTOs can be organized in adapters or API packages within the relevant module.

### Package responsibilities

- **`domain`** — aggregates, entities, value objects, domain services, domain events, and repository interfaces.
- **`application`** — use cases, orchestration, transaction boundaries, and application-level coordination.
- **`infrastructure`** — repository implementations, persistence mappings, payment-provider adapters, messaging adapters, and other technical integrations.

Not every module needs all three packages on day one. Search, for example, may initially need only application and infrastructure code.

---

## 14. What This Design Deliberately Avoids

The initial design will not:

- Turn every entity into a separate microservice.
- Put all business rules in controllers.
- Make one giant aggregate contain users, listings, bookings, payments, and refunds.
- Use distributed transactions without a demonstrated need.
- Introduce Kafka, Redis, or distributed locks solely to demonstrate technology usage.
- Couple the domain model directly to JPA, HTTP, or payment-provider classes.
- Assume that an accepted offer is equivalent to a confirmed booking.
- Assume that a successful payment means a booking is confirmed.
- Assume that a timeout proves an operation failed.
- Introduce design patterns without a concrete requirement.

---

## 15. Decisions Required Before Implementation

The following details must be finalized in the corresponding technical or business documents:

1. **Booking confirmation policy:** Precisely when does a booking become confirmed relative to availability reservation and payment?
2. **Pending booking expiry:** How long can a pending booking remain open?
3. **Offer deadlines:** What are the owner response window, offer expiry, and customer waiting options?
4. **Accepted offer failure:** What happens if the owner accepts but the requested period is no longer available?
5. **Payment recovery:** What is the recovery window, and when does the customer receive a refund option?
6. **Refund policy:** Which cases are refundable, and are partial refunds allowed?
7. **Concurrency strategy:** Which PostgreSQL constraints and transaction/locking approach will prevent overlapping bookings?
8. **Persistence mapping:** Which domain objects can map directly to database entities, and where is a separate persistence representation justified?
9. **Event delivery:** Which events initially remain internal, and which require durable asynchronous delivery?

These decisions should be recorded in the relevant design documents or Architecture Decision Records (ADRs). They should not be hidden inside implementation details.

---

## 16. Next Steps

The technical domain model is a design proposal, not a completed implementation.

The next step is to review and validate the aggregate boundaries, state transitions, and business invariants. Then proceed to:

1. High-Level Design — module interactions, runtime architecture, failure handling, and scalability.
2. Database Design — PostgreSQL schema, constraints, indexes, transaction strategy, and persistence mapping.
3. API Design — endpoints, request/response contracts, validation, errors, and idempotency.
4. Implementation — build the first small module, write tests, inspect behavior, and document the result.

**Completion criterion:** The model should make it possible to explain where each business rule lives, which component owns each state transition, how concurrent requests are handled, and how the design can evolve without prematurely introducing distributed-system complexity.

**End of Document**
