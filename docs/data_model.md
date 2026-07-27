# Brewer Bros Analytics Platform

## Version 1 Operational Data Model

### Overview

The Version 1 operational database is designed around a single business principle:

> **Every physical card owned by Brewer Bros is represented by one record in the `Cards` table.**

All other tables represent business events or supporting reference data that occur throughout a card's lifecycle.

---

## Entity Relationship Diagram

```text
                     Purchases
                    (1 Purchase)
                         │
                         │ 1:N
                         ▼
                     Cards
              (Physical Inventory)
          ┌─────────┼─────────┐
          │         │         │
        1:N       1:N       1:N
          ▼         ▼         ▼
    Locations   Listings   Grading
                    │
                    │ 1:N
                    ▼
                  Sales
```

---

## Entity Descriptions

### Purchases

Represents every acquisition event where inventory enters the business.

Examples include:

* Hobby Box
* Blaster Box
* Single Card
* Collection Purchase
* Trade
* Gift

**Relationship**

* One Purchase can create many Cards.

---

### Cards

The core operational entity.

Each record represents one physical card owned (or previously owned) by Brewer Bros.

The `Cards` table stores descriptive information about the card along with its current operational state.

Examples include:

* Player
* Year
* Brand
* Set
* Parallel
* Condition
* Current Location
* Status

All operational activity originates from this table.

---

### Locations

Represents the physical storage hierarchy for inventory.

Examples:

```text
Home
└── Office
    └── Shelf A
        └── Monster Box 001
```

A Location stores many Cards.

Each Card is stored in one Location at any given time.

---

### Listings

Represents an attempt to sell a card.

A card may:

* Never be listed
* Be listed once
* Be listed multiple times over its lifetime

Listing information includes:

* Marketplace
* Asking Price
* Listing Date
* Listing Status

Listings preserve selling history without modifying the underlying Card record.

---

### Sales

Represents completed sales transactions.

A Sale references the Listing that generated it.

Sales capture financial information such as:

* Sale Price
* Platform Fees
* Shipping
* Net Proceeds

Sales are immutable business events.

---

### Grading

Represents professional grading submissions.

Each grading event stores information such as:

* Grading Company
* Submission Date
* Return Date
* Grade
* Certification Number
* Grading Fees

A Card may have zero or many grading events throughout its lifecycle.

---

# Relationship Summary

| Parent    | Child    | Relationship |
| --------- | -------- | ------------ |
| Purchases | Cards    | One-to-Many  |
| Locations | Cards    | One-to-Many  |
| Cards     | Listings | One-to-Many  |
| Cards     | Grading  | One-to-Many  |
| Listings  | Sales    | One-to-Many  |

---

# Card Lifecycle

```text
Acquire Inventory
        │
        ▼
 Create Purchase
        │
        ▼
  Create Card
        │
        ▼
 Assign Location
        │
        ▼
   Optional Events
   ├── Listing
   ├── Grading
   └── Relocation
        │
        ▼
      Sale
        │
        ▼
 Historical Record
```

---

# Operational Design Principles

* Every physical card is represented by exactly one record in the `Cards` table.
* The operational database models Brewer Bros' business, not the entire trading card market.
* Events (Purchases, Listings, Sales, Grading) preserve historical activity rather than overwriting data.
* Data entry should be fast, intuitive, and require minimal manual overhead.
* Analytics-specific transformations belong in the downstream Databricks environment, not the operational database.

