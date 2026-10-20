# STEP 5 — Business Model & Marketplace Model

## 1. Purpose

The purpose of this step is to understand **how Peter Park Here operates as a marketplace and how value and money could flow between its participants**.

Peter Park Here is initially conceived as a **two-sided marketplace** connecting parking supply with parking demand.

---

## 2. Marketplace Participants

### 2.1 Supply — Parking Owners

Parking capacity may come from privately controlled spaces such as:

- Homes and driveways
- Apartment parking spaces
- Offices
- Shops and business premises
- Commercial properties
- Other eligible private parking spaces

The owner controls whether and when the parking space is available.

### 2.2 Demand — Parking Customers

Potential customers include people who need parking for purposes such as:

- Daily commuting
- Visiting businesses or commercial areas
- Residential visits
- Appointments and activities
- Events
- Short-term or longer-duration parking

The customer's requirement is primarily defined by **location, time, vehicle and parking suitability**.

These categories describe the initial marketplace hypothesis and should be validated through customer discovery.

---

## 3. Marketplace Interaction

The basic marketplace relationship is:

```text
Parking Owner
     │
     │ lists available capacity
     ▼
Peter Park Here
     │
     │ makes parking discoverable
     ▼
Parking Customer
     │
     │ selects and reserves
     ▼
Booking
     │
     │ payment
     ▼
Owner receives payout
```

The platform's fundamental role is to make parking capacity **discoverable, comparable, and transactable**.

This is consistent with the characteristics of a two-sided marketplace: the platform facilitates interaction between distinct participant groups whose participation creates value for the other side. Parking-sharing research specifically models private parking owners and parking demanders connected through a platform.

---

## 4. Value Flow

### Owner provides

- Parking capacity
- Availability
- Parking information
- Applicable rules and conditions

### Customer provides

- Demand
- Booking
- Payment

### Peter Park Here provides

- Marketplace discovery
- Matching between supply and demand
- Booking coordination
- Transaction/support mechanisms
- Trust and operational processes

The marketplace becomes more valuable when it can provide sufficient **supply in locations and time periods where customers have demand**.

This creates a two-sided network effect hypothesis:

> More useful parking supply can improve customer choice, while stronger customer demand can make listing parking more attractive to owners.

This is a hypothesis to validate rather than an assumption that network effects will automatically occur.

---

## 5. Money Flow

The conceptual transaction is:

```text
Customer
   │
   │ Booking payment
   ▼
Marketplace transaction
   │
   ├── Platform revenue / fees
   │
   └── Owner payout
```

The exact payment structure has **not yet been finalized**.

Possible models include:

### Model A — Commission

The platform retains a percentage of each completed booking.

```text
Customer payment
      │
      ├── Peter Park Here commission
      └── Owner payout
```

Parking-sharing research identifies commission-based models as one established way for a platform to monetize transactions.

### Model B — Customer Service Fee

The owner receives the listed parking price while the customer pays an additional platform/service fee.

### Model C — Owner Listing / Subscription Fee

Owners pay to keep their parking space listed, while the parking transaction itself may remain between customer and owner.

Current Indian examples demonstrate that this is commercially possible. ParkingNearby describes a yearly owner listing plan and no commission on parking transactions, while SpotWala advertises free listing and direct payment.

### Model D — Hybrid

A combination of:

- Booking commission
- Customer service fee
- Owner subscription
- Optional premium services

A hybrid model may be considered later if the marketplace reaches sufficient scale.

---

## 6. Initial Revenue Hypothesis

For Peter Park Here, the **initial revenue model remains a hypothesis** rather than a finalized business decision.

A transaction-based commission is a natural candidate because the platform creates value by enabling a successful booking.

However, the model must balance:

- Customer willingness to pay
- Owner willingness to participate
- Payment-processing costs
- Support and operational costs
- Competition
- Marketplace liquidity

Existing parking marketplaces demonstrate that both commission and non-commission models can be used, so the project should not assume that commission is automatically the best choice.

---

## 7. Cancellation and Refunds

A marketplace transaction requires clear handling of cancellation.

At the business-model level, the system needs policies for:

- Customer cancellation
- Owner cancellation
- No-show
- Booking failure
- Refund eligibility
- Platform fee treatment
- Owner payout treatment

The exact percentages and time windows are **not decided at this stage**.

The principle is:

> Cancellation rules should balance customer fairness, owner protection, and predictable marketplace operations.

---

## 8. Owner Incentives

Owners need a reason to supply parking capacity.

Potential incentives include:

- Additional income from unused capacity.
- Simple listing and availability management.
- Control over when the space is offered.
- Access to nearby parking demand.
- Reduced manual coordination.
- Reliable booking and payment processes.

Current Indian parking marketplaces explicitly position unused parking as an income opportunity and emphasize owner control over pricing and availability.

However, the importance of each incentive for Peter Park Here's target owners must be validated.

---

## 9. Customer Retention

Customers are likely to return when the marketplace consistently provides:

- Suitable parking near their destinations.
- Accurate availability information.
- Clear and competitive pricing.
- Reliable reservations.
- A convenient booking experience.

The key retention hypothesis is therefore:

> **A reliable parking experience should create repeat usage, particularly for recurring parking needs.**

This must be validated through actual customer behavior rather than assumed.

---

## 10. Pricing Model

Initial parking pricing should use **deterministic rules** rather than machine-learning-based dynamic pricing.

Potential pricing inputs include:

- Location
- Vehicle type
- Duration
- Time of day
- Day of week
- Parking facilities
- Other clearly defined business rules

A simple conceptual model could be:

```text
Base Price
    × Duration
    + Applicable Adjustments
    = Parking Price
```

The exact formula and who controls the base price remain product/business decisions to be defined later.

Existing Bengaluru services demonstrate examples where owners set hourly rates and may use separate rates for vehicle types.

---

## 11. Basic Marketplace Economics

We do not need a detailed financial model yet, but the following concepts should be understood.

### Booking Value

The amount paid for a parking booking.

### Gross Booking Value (GBV)

The total value of bookings processed through the marketplace over a period.

### Marketplace Commission

The portion of booking value retained by the platform when using a commission model.

```text
Platform Revenue = Booking Value × Commission Rate
```

### Owner Payout

The amount ultimately paid to the parking owner.

```text
Owner Payout = Booking Value − Applicable Platform Fees
```

### Transaction Costs

Costs associated with processing a transaction, such as payment-processing fees and other operational costs.

### Contribution Margin

The amount remaining after variable costs associated with generating and servicing a transaction.

Conceptually:

```text
Contribution Margin
= Platform Revenue
− Variable Transaction Costs
```

### Utilization

How much of the available parking capacity is actually booked.

For example:

```text
Utilization
= Booked Capacity / Available Capacity
```

Utilization is important because an owner may have a parking space available for many hours but receive few bookings.

---

## 12. Marketplace Liquidity

A major business challenge is **marketplace liquidity**.

Having many owners does not guarantee success if customers cannot find suitable spaces where and when they need them.

Similarly, attracting customers does not guarantee success if there are insufficient parking spaces available.

Therefore, Peter Park Here needs to eventually answer:

> **Can we provide sufficient relevant parking supply for customer demand in a given location and time?**

This makes geographic density and availability potentially more important than simply maximizing the number of registered users.

---

## 13. Current Business Model Hypotheses

At this stage, the following are **hypotheses rather than finalized decisions**:

| Area | Initial hypothesis |
|---|---|
| Marketplace | Two-sided parking marketplace |
| Supply | Underutilized private parking capacity |
| Demand | People needing parking at specific locations/times |
| Customer payment | Customer pays for a booking |
| Owner earnings | Owner receives payment for successful usage |
| Platform revenue | Transaction commission is a candidate model |
| Pricing | Deterministic pricing initially |
| Dynamic/ML pricing | Deferred |
| Cancellation | Policy-based refunds/cancellation |
| Owner incentive | Monetize unused capacity with manageable effort |
| Customer retention | Reliable, convenient repeat parking experience |
| Economics | Track booking value, revenue, payout, costs and utilization |

These hypotheses should be validated before being treated as final business decisions.

---

## 14. What This Step Does Not Decide

This document does **not** yet finalize:

- Commission percentage
- Service-fee percentage
- Exact parking prices
- Cancellation windows
- Refund percentages
- Owner payout timing
- Payment provider
- Tax treatment
- Detailed financial projections
- Dynamic pricing algorithms

Those decisions can be made when sufficient product and business information is available.

---

## 15. Exit Criteria

Step 5 is complete when we understand the marketplace at a conceptual commercial level:

> **Who provides parking, who demands it, how they interact, how value flows, how money could flow, how Peter Park Here could generate revenue, and which economic assumptions still require validation.**