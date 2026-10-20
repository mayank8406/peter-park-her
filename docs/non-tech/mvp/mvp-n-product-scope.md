# STEP 7 — MVP & Product Scope

## 1. Purpose

The purpose of this step is to define **exactly what Peter Park Here will build first**.

The MVP is not a collection of everything the final product may eventually contain.

It is:

> **The smallest useful product that demonstrates the core value proposition and allows us to learn from real usage.**

This follows the Lean Startup concept of an MVP as the smallest product capable of generating validated learning with reasonable effort.

For Peter Park Here, the MVP must demonstrate value for **both sides of the marketplace**.

---

## 2. MVP Objective

The MVP should prove that the fundamental marketplace loop can work:

```text id="x7m1pz"
Parking Owner
    ↓
List available parking
    ↓
Parking becomes discoverable
    ↓
Customer searches
    ↓
Customer selects parking
    ↓
Availability is checked
    ↓
Price is calculated
    ↓
Customer reserves
    ↓
Owner receives booking
```

The primary MVP question is therefore:

> **Can Peter Park Here successfully connect a customer who needs parking with an owner who has available parking and complete a reservation?**

This is more important than adding advanced functionality.

---

# 3. Core MVP User Journeys

## 3.1 Customer Journey

The minimum customer journey is:

```text id="h8d2wk"
Search Parking
      ↓
Select Parking
      ↓
Check Availability
      ↓
Calculate Price
      ↓
Reserve Parking
      ↓
View Booking
```

The customer should be able to understand:

- Where the parking is.
- Whether it is suitable.
- Whether it is available for the requested time.
- What it will cost.
- Whether the reservation was successful.

---

## 3.2 Parking Owner Journey

The minimum owner journey is:

```text id="g1x5qk"
Register
   ↓
List Parking
   ↓
Define Parking Details
   ↓
Define Availability
   ↓
Receive Booking
```

The owner should be able to:

- Create a parking listing.
- Provide essential parking information.
- Define when the parking is available.
- Set the applicable pricing information.
- See reservations for the parking space.

---

# 4. MVP Scope — MoSCoW

MoSCoW categorizes requirements as **Must Have, Should Have, Could Have, and Won't Have this time**. The important discipline is that the categories represent the needs of the specific release; they are not permanent judgments about the product.

## 4.1 Must Have

These are required for the core marketplace transaction to work.

### Customer

- Register / sign in
- Manage basic vehicle information
- Search parking
- View parking details
- Specify required date/time
- Check parking availability
- Calculate parking price
- Reserve parking
- View booking
- Cancel booking according to defined policy

### Parking Owner

- Register / sign in
- Create parking listing
- Provide essential parking details
- Define supported vehicle type
- Define availability
- Provide/set pricing information
- View bookings
- Manage listing availability

### Marketplace

- Match customer requirements with available parking.
- Prevent an unavailable space from being reserved.
- Calculate the booking price using deterministic rules.
- Create a booking with a clear status.
- Make the booking visible to the customer and owner.

### Basic Trust & Safety

- Basic user validation.
- Basic listing validation.
- Basic booking information.
- Clear cancellation rules.
- Basic handling of invalid/unavailable bookings.

These are intentionally basic. The MVP does not need a sophisticated trust-and-safety platform.

---

# 5. Payment Scope

Payment needs special treatment.

The long-term customer journey includes:

```text
Search → Reserve → Pay
```

However, the core product hypothesis we need to validate first is the **parking marketplace and reservation flow**.

Therefore:

### MVP

The booking model should support a clear price and a reservation.

Payment can initially be implemented using a **mock/test payment flow** rather than making production payment infrastructure a prerequisite.

This keeps the learning project aligned with its free-first constraint while still allowing us to model:

```text
Booking
   ↓
Payment Attempt
   ↓
Success / Failure
```

### Later

Real payment processing can be introduced after the booking flow is stable.

The intended business model remains the **hybrid model** established in Step 5:

- Transaction-based platform revenue.
- Potential customer service/platform fee.
- Potential future owner premium/subscription services.

The exact percentages are intentionally not part of the MVP scope.

---

# 6. Pricing Scope

Pricing is **Must Have** because the customer needs to know the cost before reserving.

The MVP will use deterministic pricing.

Initial inputs may include:

- Location
- Vehicle type
- Duration
- Time
- Day
- Applicable parking characteristics

The pricing system should remain simple enough to understand and test.

### Not in MVP

- Machine-learning pricing
- Demand prediction
- Real-time dynamic pricing
- Complex promotional pricing
- Personalized pricing

---

# 7. Should Have

These improve the MVP but are not required to prove the core marketplace loop.

### Customer

- Search filters
- Map-based search
- Booking history
- Saved/favourite parking
- Basic notifications
- Better cancellation/refund experience

### Owner

- Listing photos
- More detailed facilities
- Parking rules
- Owner booking notifications
- Basic earnings summary

### Marketplace

- Basic admin management
- Basic support/dispute workflow
- Basic listing approval
- Basic operational reporting

A simple workaround should remain possible if these are not implemented.

---

# 8. Could Have

These are useful enhancements but should not affect the initial marketplace validation.

- Ratings and reviews
- Advanced search ranking
- Recommendations
- Recurring bookings
- Coupons
- Loyalty features
- Advanced owner analytics
- Advanced customer notifications
- Multiple pricing strategies
- Rich parking photographs
- Enhanced check-in/check-out experience

---

# 9. Won't Have in the MVP

These are deliberately outside the first product scope.

### Advanced Product Features

- AI Parking Assistant
- ML-based dynamic pricing
- Advanced recommendation engine
- Loyalty program
- Sophisticated analytics
- Complex notification orchestration
- Corporate parking management
- Multi-city optimization

### Advanced Marketplace Operations

- Automated dispute resolution
- Sophisticated fraud detection
- Advanced owner verification
- Complex subscription management
- Automated marketplace optimization

### Large-Scale Technical Capabilities

- Multiple independently deployed microservices solely for scale
- Kafka-based event architecture
- Redis-based distributed caching
- Kubernetes deployment
- Multi-region deployment
- Advanced distributed-system infrastructure

These technologies may become appropriate later, but they are **not product requirements for proving the MVP**.

---

# 10. MVP Boundary

The MVP should deliberately remain within this boundary:

```text id="w5j2na"
Customer
  │
  ├── Search
  ├── View
  ├── Check Availability
  ├── Calculate Price
  └── Reserve
          │
          ▼
      Booking
          │
          ▼
Parking Owner
  │
  ├── List Space
  ├── Set Availability
  ├── Set Pricing
  └── Receive Booking
```

If a feature does not materially contribute to this loop, it should be challenged before being included in the MVP.

---

# 11. The Smallest Useful Vertical Slice

The first implementation should aim for one complete vertical slice rather than building every feature partially.

A useful first slice could be:

```text id="7j3nqf"
Owner creates one parking listing
        ↓
Owner defines availability and price
        ↓
Customer searches that parking
        ↓
Customer selects date/time
        ↓
System checks availability
        ↓
System calculates price
        ↓
Customer creates reservation
        ↓
Owner sees reservation
```

This proves much more than implementing ten disconnected screens.

It tests the fundamental marketplace transaction end-to-end.

---

# 12. Marketplace Liquidity Consideration

A parking marketplace has an additional MVP challenge that a normal single-sided application may not have:

> **The product needs both supply and demand.**

A customer searching for parking receives little value if no suitable spaces exist.

An owner receives little value if there are no customers.

Marketplace liquidity is therefore a fundamental concern, defined as the ability of buyers and sellers to find appropriate counterparts.

For the learning project, we do not need to solve Bengaluru-wide liquidity.

We can begin with a **small controlled set of parking listings and a focused location**.

This allows the software to demonstrate the marketplace mechanism without pretending that the entire city has already been solved.

---

# 13. MVP vs Future Product

| Area | MVP | Later |
|---|---|---|
| Customer search | Basic | Advanced ranking/recommendations |
| Availability | Basic time-based availability | Advanced real-time availability |
| Pricing | Deterministic | Dynamic/ML pricing |
| Booking | Basic reservation | Recurring/complex bookings |
| Payment | Mock/test flow initially | Production payment integration |
| Owner listing | Basic listing | Rich listing management |
| Facilities | Basic | Extensive metadata |
| Notifications | Basic | Multi-channel event-driven |
| Reviews | No | Yes |
| Loyalty | No | Yes |
| Analytics | Basic operational view | Advanced analytics |
| AI | No | AI Parking Assistant |
| Architecture | Simple | Evolve as scale requires |

Existing Indian parking products demonstrate that search, booking, availability, payment, owner listing, and availability control are realistic core capabilities for parking marketplaces.

That validates these as reasonable **candidate product capabilities**, but does not mean every comparable feature belongs in our MVP.

---

# 14. Why We Are Not Using RICE Yet

RICE can be useful when the product has a sufficiently large backlog and we need to compare many competing initiatives quantitatively.

At this stage, Peter Park Here has a relatively small and clearly defined MVP.

MoSCoW is sufficient because the immediate question is:

> **What must exist for the first usable marketplace release, and what can wait?**

We can introduce RICE later if the product backlog becomes large enough to justify a more quantitative prioritization method.

---

# 15. MVP Success Criteria

The MVP should demonstrate that the core marketplace loop works:

### Customer

> Can a customer find suitable parking, understand its availability and price, and successfully reserve it?

### Owner

> Can an owner list parking, control its availability, and receive a reservation?

### Marketplace

> Can the system connect available supply with customer demand without allowing conflicting reservations?

### Business hypothesis

> Can the marketplace demonstrate enough value that both sides have a reason to participate?

The last question is a **validation goal**, not something the software alone can prove.

---

# 16. Scope Discipline

The following rule applies throughout MVP development:

> **If a feature does not help demonstrate the core value proposition or generate important learning, it should not automatically enter the MVP.**

The MVP should optimize for **learning and a complete usable journey**, not the number of technologies or features included.

---

# 17. Exit Criteria

Step 7 is complete when we can clearly answer:

> **What exactly are we building first?**

The answer is:

> **A focused two-sided parking marketplace in which owners can list parking and define availability, while customers can discover suitable parking, check availability, calculate the deterministic price, and reserve a space. The MVP demonstrates the complete owner-to-customer booking loop without introducing advanced features or large-scale infrastructure prematurely.**