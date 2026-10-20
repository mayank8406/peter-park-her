# Peter Park Here

## Product to Production Development Workflow

This workflow defines how **Peter Park Here** will be developed from an initial product idea into a production-oriented backend system.

The goal is not to create documentation for the sake of documentation. Each step should answer a specific question, produce a useful output, and provide the foundation for the next step.

The overall principle is:

**Understand → Decide → Document → Build → Test → Learn → Evolve**

---

# PHASE 1 — PRODUCT & BUSINESS

The purpose of this phase is to understand the problem, users, product, business model, and real-world processes **before designing the software**.

We should think like a founder/product person during this phase, not like a developer choosing technologies.

---

## STEP 1 — Company / Product Introduction

### Purpose

Establish what Peter Park Here is, the problem space it belongs to, and the vision behind the product.

### What we need to figure out

- What is Peter Park Here?
- What problem space does it operate in?
- Who is it intended to help?
- What is the basic product idea?
- What is the vision?

### What we do

Create a concise company/product introduction that communicates the idea and its purpose.

### Output

- Company/product introduction
- Product vision
- High-level description of the marketplace

### Status

**Completed**

### Exit Criteria

Someone unfamiliar with the project should understand what Peter Park Here is and why it exists.

### What we should NOT do yet

- Technical architecture
- Database design
- API design
- Detailed feature specifications
- Cloud/service selection

---

# STEP 2 — Problem Discovery

### Purpose

Understand the actual problem before deciding exactly what the product should build.

A common mistake is:

> "I have an idea, therefore I should immediately build the features."

Instead, we first ask:

> "What problem are we actually solving?"

### What we need to figure out

For parking:

- What problems do drivers face when looking for parking?
- What problems do parking-space owners face?
- Why are existing parking options insufficient?
- What makes parking difficult in dense cities such as Bengaluru?
- What happens today when someone needs parking?
- What alternatives already exist?
- Which problems are important enough to solve?

### Theory / Framework

Useful concepts:

- Lean Startup
- Customer Development
- Problem Discovery
- Jobs-to-Be-Done

These are used to improve our thinking, not to create unnecessary paperwork.

### Output

**Problem Definition**

Containing:

- Core problem
- Problems faced by customers
- Problems faced by parking owners
- Existing alternatives
- Major pain points

### Exit Criteria

We can clearly state:

> "Peter Park Here exists because \_\_\_\_\_\_."

without immediately describing the solution.

### What we should NOT do yet

- Finalize all features
- Design APIs
- Choose microservices
- Choose databases
- Build the application

---

# STEP 3 — Users & Jobs-to-Be-Done

### Purpose

Understand who interacts with the product and what they are actually trying to accomplish.

### What we need to figure out

The initial marketplace has two primary sides.

### Customer

A person looking for parking.

Possible job:

> Find a suitable parking space near my destination for the required time, know the cost, and confidently reserve it.

### Parking Owner

A person or organization with an available parking space.

Possible job:

> Make unused parking capacity available and earn money from it without creating excessive operational work.

### What we do

Define:

- User types
- Their goals
- Their problems
- Their motivations
- Their important tasks
- Their expectations
- Their pain points

Then convert these into **use cases**.

### Example customer use cases

- Search for parking
- View parking details
- Check availability
- Calculate price
- Reserve parking
- Pay
- Cancel booking
- View booking
- Receive booking information

### Example owner use cases

- Register
- Add parking space
- Define facilities
- Define rules
- Define availability
- Set pricing information
- View bookings
- Manage parking
- Receive booking/payment information

### Theory / Framework

**Jobs-to-Be-Done (JTBD)** is particularly useful here.

The focus is not merely:

> "Who is the user?"

but:

> "What is the user trying to accomplish?"

### Output

**Users & Use Cases**

A lightweight document/table containing:

- User types
- Goals
- Jobs
- Pain points
- Use cases

### Exit Criteria

We understand who uses the system and what they need to accomplish.

### What we should NOT do yet

Do not turn every possible user request into an MVP feature.

That decision comes later.

---

# STEP 4 — Value Proposition

### Purpose

Define why someone would choose Peter Park Here instead of their current alternative.

### What we need to figure out

For customers:

- What value do they receive?
- What frustration are we removing?
- Why would they use this instead of searching manually?

For owners:

- What value do they receive?
- Why would they list their parking?
- What makes listing worthwhile?

### Theory / Framework

Use the principles of the **Value Proposition Canvas**:

- Customer jobs
- Customer pains
- Customer gains
- Products/services
- Pain relievers
- Gain creators

We use this as a thinking tool, not as a mandatory elaborate document.

### Output

A concise **Peter Park Here Value Proposition**.

For example, conceptually:

**Customers:** discover, compare, and reserve nearby parking with predictable availability and pricing.

**Owners:** monetize unused parking capacity through a managed marketplace.

### Exit Criteria

We can explain the value of the product separately for both sides of the marketplace.

---

# STEP 5 — Business Model & Marketplace Model

### Purpose

Understand how the business operates and how value and money flow between participants.

Peter Park Here is a **two-sided marketplace**.

### What we need to figure out

#### Supply

Where do parking spaces come from?

- Homes
- Apartments
- Offices
- Shops
- Commercial properties
- Other private spaces

#### Demand

Who needs them?

- Daily commuters
- Visitors
- Residents
- Customers visiting businesses
- Event attendees

#### Marketplace interaction

Understand:

**Owner → Parking Space → Marketplace → Customer → Booking → Payment**

### Business questions

- Who pays?
- Who receives money?
- How could Peter Park Here make revenue?
- Is revenue commission-based?
- Is there a service fee?
- Are there refunds?
- What happens when a booking is cancelled?
- What incentives encourage owners to list?
- What makes customers return?

### Pricing

Initial pricing should be deterministic.

Potential factors:

- Location
- Vehicle type
- Duration
- Facilities
- Time
- Day

We deliberately avoid ML-based dynamic pricing at this stage.

### Basic economics

We should understand concepts such as:

- Booking value
- Marketplace commission
- Owner payout
- Transaction costs
- Contribution margin
- Utilization

We do not need a complicated financial model for this project.

### Output

**Business & Marketplace Model**

Containing:

- Participants
- Value flow
- Money flow
- Basic revenue model
- Pricing hypothesis
- Marketplace mechanics

### Exit Criteria

We understand how the marketplace works commercially at a conceptual level.

---

# STEP 6 — Assumptions & Validation

### Purpose

Identify the assumptions that could make the product fail.

A product idea contains many assumptions.

For example:

> "People will be willing to rent unused parking spaces."

That is an assumption, not a fact.

### What we need to figure out

Identify assumptions around:

- Customer demand
- Owner willingness
- Pricing
- Availability
- Trust
- Safety
- Convenience
- Marketplace liquidity
- Payment
- Cancellation
- Location density

### Classify assumptions

Identify which assumptions are:

- High impact
- High uncertainty

These deserve attention first.

### Theory / Framework

Useful concepts:

- Lean Startup
- Hypothesis-driven development
- Assumption mapping
- Build–Measure–Learn

### Output

**Assumption & Validation List**

Example:

| Assumption                      | Impact | Uncertainty | How to Validate             |
| ------------------------------- | ------ | ----------- | --------------------------- |
| Drivers want pre-booked parking | High   | High        | User interviews / prototype |
| Owners will list unused spaces  | High   | High        | Owner interviews            |
| Customers accept commission     | High   | Medium      | Pricing experiment          |

For an actual startup, these could be validated through interviews, prototypes, landing pages, pilots, etc.

For this learning project, we can document the assumptions even when we cannot perform real-world validation.

### Exit Criteria

We know which assumptions are important and which should remain hypotheses rather than being treated as facts.

### What we should NOT do yet

Do not spend weeks doing formal market research just because frameworks exist.

The goal is learning, not paperwork.

---

# STEP 7 — MVP & Product Scope

### Purpose

Decide exactly what Peter Park Here will build first.

The MVP is not:

> "Everything the final product might eventually contain."

It is:

> "The smallest useful product that demonstrates the core value proposition."

### What we need to decide

#### Must Have

The minimum journey should support something close to:

**Search parking → Select parking → Check availability → Calculate price → Reserve**

For owners:

**Register → List parking → Define availability → Receive bookings**

### Later features

Examples:

- Advanced recommendations
- Dynamic pricing
- Loyalty
- Reviews
- Advanced analytics
- Complex notifications
- AI assistant
- Large-scale distributed architecture

### Prioritization

Use a lightweight prioritization method such as:

**MoSCoW**

- Must Have
- Should Have
- Could Have
- Won't Have Now

Other prioritization frameworks such as RICE can be introduced if the product becomes large enough to justify them.

### Output

**MVP Scope**

Containing:

- MVP features
- Future features
- Out-of-scope features
- Core user journeys
- Prioritization

### Exit Criteria

We can clearly answer:

> "What exactly are we building first?"

---

# STEP 8 — Product Requirements Document (PRD)

### Purpose

Turn the chosen MVP into precise product requirements.

The PRD is where the product becomes sufficiently precise for engineering.

### What we need to define

For each important feature:

- Purpose
- User
- Trigger
- Expected behavior
- Inputs
- Outputs
- Business rules
- Validation
- Success/failure conditions
- Acceptance criteria

### Example

For booking:

**Customer selects a parking space and time period.**

The system must:

1. Verify that the parking space is available.
2. Calculate the price.
3. Create the reservation.
4. Prevent conflicting reservations.
5. Return booking information.

### Output

**PRD**

The PRD becomes one of the primary inputs to the engineering phase.

### Exit Criteria

Engineering should be able to understand **what the product must do** without having to invent business behavior.

### What we should NOT do yet

The PRD should not prescribe implementation unnecessarily.

For example:

Bad:

> "Use PostgreSQL transaction X with Redis lock Y."

Better:

> "The system must prevent two customers from successfully reserving the same parking space for overlapping time periods."

The implementation belongs to engineering design.

---

# STEP 9 — Business Workflows

### Purpose

Understand how the business process operates from beginning to end.

The PRD describes requirements.

The workflow describes **how those requirements interact as a real-world process**.

### Workflows to define

#### Customer journey

**Search → Select → Availability → Price → Reserve → Pay → Confirmation**

#### Owner journey

**Register → List parking → Configure → Make available → Receive booking → Earn**

#### Cancellation

**Cancellation request → Eligibility check → Cancellation → Refund calculation → Payment/refund processing**

#### Failure scenarios

Examples:

- Parking becomes unavailable
- Payment fails
- Customer cancels
- Owner cancels
- Payment succeeds but booking confirmation fails
- Booking request is duplicated

### Output

**Business Workflow Diagrams**

Potentially using:

- Flowcharts
- Sequence-like business flows
- State transitions

### Exit Criteria

The major real-world processes are understandable from start to finish, including important exception paths.

---

# STEP 10 — Domain Model

### Purpose

Bridge the business world and the software world.

Before designing classes or databases, identify the important concepts that exist in the business.

### Possible domain concepts

- User
- Customer
- Parking Owner
- Vehicle
- Parking Space
- Parking Listing
- Availability
- Booking
- Payment
- Refund
- Pricing Rule
- Facility
- Parking Rule

### What we need to understand

For each domain concept:

- What does it represent?
- Who owns it?
- What information does it contain?
- What relationships does it have?
- What states can it have?
- What business rules apply?

### Example

A Booking may have states such as:

**PENDING → CONFIRMED → ACTIVE → COMPLETED**

or:

**PENDING → CANCELLED**

depending on the business rules.

### Important principle

The domain model should represent the **business**, not the database tables.

We should not begin by asking:

> "What tables should I create?"

We first ask:

> "What concepts exist in the parking business?"

### Output

**Domain Model**

Containing:

- Domain entities
- Relationships
- Important attributes
- Lifecycle/state information
- Business rules
- Domain glossary

### Exit Criteria

We understand the important business objects and their relationships well enough to begin software design.

---

# PHASE 2 — ENGINEERING

The purpose of this phase is to convert the product/domain understanding into a reliable software system.

The engineering principle is:

**Simple first → prove correctness → identify real constraints → evolve architecture**

---

# STEP 11 — Functional & Non-Functional Requirements

### Purpose

Translate product requirements into engineering requirements.

### Functional requirements

Describe what the system must do.

Examples:

- Search parking
- Create listing
- Check availability
- Create booking
- Cancel booking
- Process payment

### Non-functional requirements

Describe how well the system must operate.

Examples:

- Availability
- Performance
- Reliability
- Security
- Scalability
- Observability
- Maintainability

### Output

**Functional & Non-Functional Requirements**

### Exit Criteria

We know both:

**What the system does**

and

**What qualities the system must provide.**

---

# STEP 12 — High-Level Design (HLD)

### Purpose

Design the architecture of the system.

### Questions

- What are the major components?
- What is the request flow?
- Where is data stored?
- How do components communicate?
- Where are transactions required?
- Where could concurrency occur?
- What failures can happen?
- What should be synchronous?
- What should eventually become asynchronous?

### Initial architecture

We start with a **modular monolith** unless there is a genuine reason to split services.

Potential modules:

- User
- Parking
- Search
- Pricing
- Booking
- Payment
- Notification

Later, some may become independent services.

### Output

**HLD**

Containing:

- Architecture diagram
- Components/modules
- Request flows
- Data flows
- Major technology decisions
- Scalability considerations
- Failure scenarios

### Exit Criteria

We have a defensible high-level architecture.

---

# STEP 13 — Database Design

### Purpose

Design persistent storage based on actual domain and access requirements.

### Initial database

**PostgreSQL**

Suitable for:

- Users
- Vehicles
- Ownership
- Bookings
- Payments
- Refunds
- Transactional data

### Concepts to learn

- Tables
- Relationships
- Primary keys
- Foreign keys
- Constraints
- Normalization
- Indexes
- Transactions
- Isolation
- Locking
- Query optimization

### Later

Introduce MongoDB where flexible document-oriented data provides a real benefit.

Potential examples:

- Parking metadata
- Facilities
- Rules
- Photos/metadata
- Geospatial/search-oriented information

### Output

**Database Design**

Containing:

- ER diagram
- Schema
- Relationships
- Constraints
- Indexes
- Transaction boundaries

### Exit Criteria

We know where authoritative data lives and why.

---

# STEP 14 — API Design

### Purpose

Define how clients interact with the backend.

### Examples

- User APIs
- Parking APIs
- Search APIs
- Pricing APIs
- Booking APIs
- Payment APIs

### Concepts

- REST
- HTTP methods
- Status codes
- Request/response models
- Validation
- Pagination
- Filtering
- Error responses
- Idempotency

### Output

**API Specification**

Potentially using OpenAPI/Swagger.

### Exit Criteria

The API contract is clear enough for implementation and testing.

---

# STEP 15 — Low-Level Design (LLD)

### Purpose

Design the internal implementation of important modules.

### Example: Pricing Engine

Possible design:

- PricingStrategy
- LocationPricingStrategy
- VehiclePricingStrategy
- DurationPricingStrategy
- PricingRule
- PricingEngine

This is where patterns such as **Strategy** become useful when they solve a real extensibility problem.

### Other LLD exercises

- Booking lifecycle
- Payment providers
- Notification system
- Search filtering
- Availability calculation

### Concepts

- SOLID
- Interfaces
- Composition
- Design patterns
- State machines
- Exception handling
- Concurrency
- Testability

### Output

**LLD**

Containing:

- Classes
- Interfaces
- Responsibilities
- Relationships
- Important sequence flows
- Design decisions

---

# STEP 16 — Architecture Decision Records (ADRs)

### Purpose

Record important technical decisions and their reasoning.

### Example

**Decision:** Start with a modular monolith.

**Alternatives:**

- Microservices immediately
- Modular monolith

**Reason:**

The project is initially small and the primary objective is correctness and learning domain boundaries.

### Output

Short ADRs for important decisions.

### Exit Criteria

Someone reading the project can understand not only **what** we chose but **why**.

---

# STEP 17 — Development Plan

### Purpose

Convert the architecture into incremental implementation milestones.

Each milestone should produce something working.

### Example

**Milestone 1**

User registration

**Milestone 2**

Parking listing

**Milestone 3**

Parking search

**Milestone 4**

Availability

**Milestone 5**

Pricing

**Milestone 6**

Booking

**Milestone 7**

Concurrency protection

**Milestone 8**

Payment abstraction

### Principle

Do not build the whole system and test it at the end.

Instead:

**Build → Test → Understand → Document → Continue**

### Exit Criteria

We have a sequence of small, demonstrable increments.

---

# STEP 18 — First Vertical Slice

### Purpose

Build the first complete working path through the system.

For example:

**Customer → Search Parking → Select Parking → Check Availability → Create Booking**

This is more valuable than implementing ten disconnected modules.

### Output

A working end-to-end feature.

---

# STEP 19 — Testing

### Purpose

Establish confidence in correctness.

### Testing layers

- Unit tests
- Integration tests
- Repository/database tests
- API tests
- End-to-end tests where useful

### Preferred tools

- JUnit
- Mockito
- Spring Boot testing facilities

### Output

Automated test suite covering important business behavior.

---

# STEP 20 — Concurrency & Data Integrity

### Purpose

Solve one of the most important distributed/backend problems in the project:

**Preventing double booking.**

### Problems to study

- Race conditions
- Concurrent requests
- Duplicate requests
- Retries
- Optimistic locking
- Pessimistic locking
- Database constraints
- Idempotency
- Distributed locking

### Important principle

We should first understand how the database can guarantee correctness before introducing Redis or distributed locks.

### Output

A tested and documented booking consistency strategy.

---

# STEP 21 — Search Evolution

### Purpose

Improve parking discovery as requirements become more realistic.

Initially:

- PostgreSQL queries
- Filtering
- Sorting
- Pagination

Later:

- Geospatial search
- MongoDB where justified
- Search-result caching

### Concepts

- Spatial queries
- Indexes
- Query optimization
- Pagination
- Search relevance

---

# STEP 22 — MongoDB

### Purpose

Introduce polyglot persistence because there is a real data-modeling reason.

### Learn

- Document modeling
- Embedding vs referencing
- Indexes
- Geospatial queries
- Consistency
- Trade-offs between SQL and NoSQL

### Important rule

Do not move data to MongoDB merely to say:

> "The project uses MongoDB."

Every datastore must have a clear purpose and source-of-truth definition.

---

# STEP 23 — Kafka

### Purpose

Introduce asynchronous communication where it provides real value.

### Example events

- ParkingListed
- ParkingUpdated
- BookingCreated
- BookingConfirmed
- BookingCancelled
- PaymentSucceeded
- PaymentFailed

### Learn

- Producers
- Consumers
- Topics
- Partitions
- Consumer groups
- Offsets
- Ordering
- Retries
- Dead-letter queues
- Idempotent consumers
- Eventual consistency

### Key question

For every Kafka use case:

> "Why is this better as an asynchronous event than a synchronous REST call?"

---

# STEP 24 — Redis

### Purpose

Introduce Redis only where low-latency or short-lived state provides a genuine benefit.

### Possible uses

- Cache
- Search-result cache
- Rate limiting
- Idempotency
- Short-lived data

### Learn

- TTL
- Eviction
- Cache-aside
- Invalidation
- Cache stampede
- Failure behavior

### Important rule

Redis should not become the authoritative source for transactional booking correctness.

---

# STEP 25 — Microservice Evolution

### Purpose

Learn when and why a modular monolith should evolve into microservices.

Potential candidates:

- Booking
- Payment
- Notification
- Search

But only split a module when there is an architectural reason.

### Reasons may include

- Independent scaling
- Independent deployment
- Different reliability requirements
- Team ownership
- Technology differences
- Clear domain boundaries

### Learn

- Service boundaries
- Network failures
- Timeouts
- Retries
- Circuit breakers
- Distributed transactions
- Eventual consistency

---

# STEP 26 — Docker

### Purpose

Package services consistently and understand containerization.

### Learn

- Images
- Containers
- Dockerfiles
- Networking
- Environment configuration
- Volumes
- Container lifecycle

Because the laptop has only approximately 20 GB free space, local container infrastructure should remain minimal.

---

# STEP 27 — Kubernetes

### Purpose

Understand orchestration and deployment concepts after the application actually needs them.

### Learn

- Pods
- Deployments
- Services
- ConfigMaps
- Secrets
- Readiness probes
- Liveness probes
- Scaling
- Rolling deployments

Kubernetes should be learned conceptually and with a lightweight/free path rather than creating a large local cluster unnecessarily.

---

# STEP 28 — Cloud / AWS

### Purpose

Understand how the system maps from local architecture to cloud infrastructure.

### Learn

- Compute
- Networking
- Managed databases
- Storage
- IAM
- Monitoring
- Deployment

### Free-only rule

No paid AWS infrastructure should be required.

Any AWS service suggested must first be checked for current free-tier/free-usage conditions and limits.

If a service cannot be used safely for free, use a genuinely free alternative.

---

# STEP 29 — Security

### Purpose

Make the system secure enough to demonstrate production-oriented backend thinking.

### Learn

- Authentication
- Authorization
- Password handling
- Input validation
- API security
- Secrets
- Role-based access
- Rate limiting
- Secure payment boundaries
- AI tool authorization

### Important principle

The AI agent must never bypass backend authorization or business rules.

---

# STEP 30 — Observability

### Purpose

Understand how engineers operate and debug distributed systems.

### Learn

**Logging**

- Structured logs
- Log levels
- Correlation IDs

**Metrics**

- Request rate
- Latency
- Error rate
- Booking success rate
- Database metrics

**Tracing**

- Request propagation
- Service-to-service tracing

**Health**

- Liveness
- Readiness
- Dependency health

---

# STEP 31 — CI/CD

### Purpose

Automate building, testing, and delivery.

### GitHub workflow

**Code → Pull Request → Build → Test → Quality checks → Package → Deploy**

### Learn

- Git branching
- Pull requests
- GitHub Actions
- Automated tests
- Build artifacts
- Deployment pipelines
- Secrets management

Everything should use the free GitHub path where possible.

---

# STEP 32 — Load & Failure Testing

### Purpose

Move from:

> "It works."

to:

> "I understand how it behaves under stress and failure."

### Test scenarios

- Many simultaneous searches
- Concurrent bookings
- Duplicate booking requests
- Database failure
- Kafka consumer failure
- Redis failure
- Payment failure
- Slow downstream service
- Network timeout
- Retry storms

### Learn

- Capacity estimation
- Bottlenecks
- Backpressure
- Timeouts
- Retries
- Rate limiting
- Graceful degradation

---

# STEP 33 — AI Parking Assistant

### Purpose

Add an AI agent only after the underlying backend is reliable.

The AI should act as an interface/orchestration layer over existing backend capabilities.

### Possible tools

- searchParking
- getParkingDetails
- calculatePrice
- checkAvailability
- createBooking
- cancelBooking
- createSupportRequest

### Important architecture rule

The AI does **not** own business logic.

The backend remains responsible for:

- Authorization
- Availability
- Pricing rules
- Booking correctness
- Payment
- Cancellation rules

### Learn

- LLM APIs
- Structured output
- Tool calling
- Agents vs workflows
- Sessions
- Memory
- RAG where useful
- MCP where useful
- Guardrails
- Tool security
- Evaluation
- Tracing
- Cost control

### Free-only requirement

The AI portion must have a genuinely free development path.

Paid LLM API usage must not be required to complete the project.

---

# STEP 34 — Final Architecture Review

### Purpose

Step back and evaluate the entire system as an experienced backend engineer.

### Review

#### Product

- Does the system solve the intended problem?
- Are the MVP boundaries clear?

#### Architecture

- Are service boundaries justified?
- Is the architecture unnecessarily complicated?

#### Database

- Are sources of truth clear?
- Are indexes and constraints appropriate?

#### Concurrency

- Can double booking occur?
- Are retries safe?
- Is idempotency handled?

#### Distributed systems

- What happens when services fail?
- What happens when events are duplicated?
- Where is eventual consistency acceptable?

#### Security

- Can users access resources they should not?
- Are secrets protected?

#### Observability

- Can we diagnose production failures?

#### Operations

- Can the system be deployed?
- Can it be monitored?
- Can it be rolled back?

#### AI

- Can the agent misuse tools?
- Are business rules still enforced by backend services?

### Final output

A complete architecture review documenting:

- Current architecture
- Evolution history
- Important decisions
- Known limitations
- Future improvements
- Lessons learned

---

# OVERALL DEVELOPMENT MAP

The complete journey is:

**Company/Product Introduction**

↓

**Problem Discovery**

↓

**Users & Jobs-to-Be-Done**

↓

**Value Proposition**

↓

**Business & Marketplace Model**

↓

**Assumptions & Validation**

↓

**MVP & Product Scope**

↓

**PRD**

↓

**Business Workflows**

↓

**Domain Model**

↓

**Functional & Non-Functional Requirements**

↓

**HLD**

↓

**Database Design**

↓

**API Design**

↓

**LLD**

↓

**Architecture Decisions**

↓

**Development Plan**

↓

**First Vertical Slice**

↓

**Testing**

↓

**Concurrency & Data Integrity**

↓

**Search Evolution**

↓

**MongoDB**

↓

**Kafka**

↓

**Redis**

↓

**Microservices**

↓

**Docker**

↓

**Kubernetes**

↓

**Cloud / AWS**

↓

**Security**

↓

**Observability**

↓

**CI/CD**

↓

**Load & Failure Testing**

↓

**AI Parking Assistant**

↓

**Final Architecture Review**

---

# THE CORE RULE FOR THE PROJECT

At every stage, follow:

**Build → Test → Understand → Document → Continue**

Do not introduce a technology simply because it appears on the technology checklist.

For every major technology or architectural decision, answer:

**Problem → Why do we need it? → Alternatives → Decision → Trade-offs → Implementation → Testing → Interview Questions**

The objective of Peter Park Here is therefore not:

> "Build an application containing Java, Kafka, Redis, MongoDB, Kubernetes and AWS."

The objective is:

> **Build a real system while progressively learning why each engineering concept exists, when it should be used, how it works, what can go wrong, and how to defend the design in an engineering interview.**
