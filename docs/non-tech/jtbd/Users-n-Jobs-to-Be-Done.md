# STEP 3 — Users & Jobs-to-Be-Done

## 1. Purpose

The purpose of this step is to understand **who interacts with Peter Park Here and what they are trying to accomplish**.

The initial marketplace has two primary sides:

1. **Parking Customer** — a person looking for parking.
2. **Parking Owner** — a person or organization with available parking capacity.

The focus is on understanding user needs and jobs before deciding which features belong in the MVP.

---

## 2. Primary Users

### 2.1 Parking Customer

A person looking for a parking space for a particular location and time.

**Primary goal:**

> Find a suitable parking space near my destination for the required time, understand the cost, and confidently reserve it.

**Key motivations:**

- Avoid wasting time searching for parking.
- Reduce uncertainty about availability.
- Know the expected cost before committing.
- Secure parking before reaching the destination.

**Key pain points:**

- Difficulty finding suitable parking.
- Uncertainty about availability.
- Uncertainty about price and parking conditions.
- Time spent searching for parking.
- Lack of confidence that a space will actually be available.

### 2.2 Parking Owner

A person or organization with a parking space that is available for use during certain periods.

**Primary goal:**

> Make unused parking capacity available and earn income from it without creating excessive operational work.

**Key motivations:**

- Generate value from unused parking capacity.
- Control when the space is available.
- Receive and manage bookings conveniently.
- Reduce manual coordination.

**Key pain points:**

- Finding suitable customers.
- Managing availability and bookings.
- Coordinating access.
- Concerns about misuse or disputes.
- Managing payments and cancellations.

---

## 3. Jobs-to-Be-Done

Jobs-to-Be-Done (JTBD) focuses on the outcome a user is trying to achieve rather than starting with the features of a product. This helps keep product discovery focused on customer needs rather than prematurely designing features.

### Customer Jobs

| Job | When | I want to | So I can |
|---|---|---|---|
| Find parking | I am travelling to a location | Find suitable parking nearby | Avoid wasting time searching |
| Evaluate parking | I find potential spaces | Understand location, availability, price and relevant facilities | Choose the most suitable option |
| Reserve parking | I find a suitable space | Reserve it for the required time | Have confidence that parking will be available |
| Manage parking | My plans change | View or cancel my reservation | Manage my parking needs conveniently |

### Owner Jobs

| Job | When | I want to | So I can |
|---|---|---|---|
| List parking | I have unused parking capacity | Make it available to customers | Generate value from unused capacity |
| Control availability | My parking is not always available | Define when it can be used | Retain control of my space |
| Manage bookings | Customers reserve my space | View and manage reservations | Coordinate parking usage |
| Earn from parking | Customers use my space | Receive the agreed payment | Generate income with reasonable effort |

---

## 4. User Use Cases

The identified jobs translate into candidate use cases.

### Customer Use Cases

- Register / sign in
- Manage vehicle information
- Search for parking
- View parking details
- Check availability
- Calculate price
- Reserve parking
- Pay for a reservation
- View booking
- Cancel booking
- Receive booking information

### Parking Owner Use Cases

- Register / sign in
- Add a parking space
- Define parking facilities
- Define parking rules
- Define availability
- Provide pricing information
- View bookings
- Manage parking
- Receive booking information
- Receive payment information

These are **candidate use cases**, not an MVP feature commitment.

---

## 5. Important Expectations

### Customer

The customer is expected to value:

- Accurate availability information
- Clear pricing
- Relevant parking details
- A predictable reservation process
- Confidence that the reserved space will be usable

### Parking Owner

The owner is expected to value:

- Control over availability
- Simple listing management
- Reliable booking information
- Reasonable operational effort
- Secure and predictable transactions

These expectations are assumptions that should be validated through user research.

---

## 6. Secondary Stakeholders

The primary users directly interact with the marketplace. Other stakeholders may influence or be affected by the product:

- Property or apartment associations
- Local authorities
- Platform administrators
- Payment providers

These stakeholders should be considered when defining business and product requirements, but they are not primary marketplace users at this stage.

---

## 7. Discovery Boundaries

This step establishes:

- Who the primary users are.
- What they are trying to accomplish.
- Their major pain points and motivations.
- The candidate use cases that may support those jobs.

It does **not** determine which use cases must be included in the MVP.

For example, identifying **"Pay for a reservation"** as a customer use case does not by itself determine the payment implementation, payment provider, or whether real payments are required in the first release.

Those decisions belong to subsequent product and business-requirement stages.

---

## 8. Exit Criteria

Step 3 is complete when we have a clear understanding of:

> **Who uses the system, what they are trying to accomplish, what problems they experience, and which use cases may help them accomplish those jobs.**

The next step is to translate these user needs into **Business Goals and Business Requirements**.