# Brewer Bros Cards Operating System (BBOS)
# Data Dictionary (Version 1.0)

**Status:** Draft (Living Document)

## Purpose

This document defines the Version 1 operational data model for Brewer Bros Cards (BBOS). The objective is to document the business meaning of every table and field rather than implementation details.

**Guiding Principle**

> Every table must reduce operational friction.

---

# Table of Contents

1. Acquisitions
2. Cards
3. Listings
4. Sales
5. Locations
6. Grading

---

# ACQUISITIONS

## Purpose

Represents an event where cards enter inventory.

An acquisition may represent:

- Hobby box purchase
- Retail blaster
- Card show purchase
- eBay purchase
- Trade
- Gift
- Collection purchase

One acquisition may contain many cards.

### Primary Key

| Field | Type | Description |
|--------|------|-------------|
| AcquisitionID | UUID | Unique acquisition identifier |

### Fields

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| AcquisitionDate | Date | Yes | Date inventory entered business |
| Source | Text | Yes | Where cards came from |
| AcquisitionType | Enum | Yes | Wax, Single Purchase, Trade, Collection, Gift |
| TotalCost | Currency | Yes | Total acquisition cost |
| Tax | Currency | No | Sales tax paid |
| Shipping | Currency | No | Shipping cost |
| Notes | Text | No | Freeform notes |

### Relationships

```
Acquisition (1)
        │
        │
        ▼
Cards (*)
```

---

# CARDS

## Purpose

Master operational record representing a single physical card currently or previously owned.

Every card exists exactly once in this table.

### Primary Key

| Field | Type | Description |
|--------|------|-------------|
| CardID | UUID | Internal unique identifier |

### Foreign Keys

| Field | References |
|--------|------------|
| AcquisitionID | Acquisitions |
| CurrentLocationID | Locations |

### Identity

| Field | Type | Description |
|--------|------|-------------|
| Sport | Text | Baseball, Football, Basketball, etc. |
| Year | Integer | Card year |
| Manufacturer | Text | Topps, Panini, Upper Deck |
| Brand | Text | Chrome, Prizm, Select |
| Set | Text | Specific product/set |
| CardNumber | Text | Printed card number |
| Player | Text | Player name |
| Team | Text | Team shown on card |

### Attributes

| Field | Type | Description |
|--------|------|-------------|
| Parallel | Text | Refractor, Gold, Mojo, etc. |
| InsertName | Text | Optional insert set |
| Rookie | Boolean | Rookie card indicator |
| SerialNumber | Text | Example: 12/99 |
| Auto | Boolean | Autograph |
| Memorabilia | Boolean | Relic/Patch |
| Condition | Enum | Raw, NM, LP, etc. |

### Inventory

| Field | Type | Description |
|--------|------|-------------|
| CostBasis | Currency | Allocated acquisition cost |
| CurrentLocationID | UUID | Current storage location |
| Status | Enum | In Inventory, Listed, Sold, At Grading |
| Notes | Text | Internal notes |

### Relationships

```
Cards
 │
 ├── Listings
 ├── Grading
 └── Locations
```

---

# LISTINGS

## Purpose

Represents a listing created to sell a card.

A card may have multiple listings over time.

Only one listing should be active at a time.

### Primary Key

| Field | Type |
|--------|------|
| ListingID | UUID |

### Foreign Keys

| Field | References |
|--------|------------|
| CardID | Cards |

### Fields

| Field | Type | Description |
|--------|------|-------------|
| Marketplace | Enum | eBay, COMC, Facebook, Card Show |
| ListingDate | Date | Date listed |
| AskingPrice | Currency | Initial asking price |
| CurrentPrice | Currency | Current listing price |
| MarketplaceListingID | Text | External marketplace identifier |
| Status | Enum | Active, Sold, Ended, Cancelled |
| Notes | Text | Listing notes |

### Relationships

```
Card (1)
      │
      ▼
Listing (*)
      │
      ▼
Sale (0..1)
```

---

# SALES

## Purpose

Represents a completed sale resulting from a listing.

Sales are immutable historical records.

### Primary Key

| Field | Type |
|--------|------|
| SaleID | UUID |

### Foreign Keys

| Field | References |
|--------|------------|
| ListingID | Listings |

### Fields

| Field | Type | Description |
|--------|------|-------------|
| SaleDate | Date | Date sold |
| SalePrice | Currency | Gross selling price |
| MarketplaceFees | Currency | Platform fees |
| ShippingCharged | Currency | Shipping collected |
| ShippingCost | Currency | Actual shipping expense |
| TaxesCollected | Currency | Marketplace collected tax |
| BuyerName | Text | Marketplace username |
| OrderNumber | Text | Marketplace order ID |
| NetProceeds | Currency | Sale proceeds after fees |
| Notes | Text | Sale notes |

### Relationships

```
Listing (1)
      │
      ▼
Sale (1)
```

---

# LOCATIONS

## Purpose

Defines the physical storage hierarchy.

Locations are reusable and hierarchical.

### Primary Key

| Field | Type |
|--------|------|
| LocationID | UUID |

### Fields

| Field | Type | Description |
|--------|------|-------------|
| ParentLocationID | UUID | Parent location |
| Name | Text | Display name |
| Type | Enum | Room, Shelf, Row, Box, Binder, Safe |
| Active | Boolean | Active location |

### Example

```
Closet
 ├── Shelf A
 │      ├── Row 1
 │      │      ├── Box 1
 │      │      └── Box 2
 │      └── Row 2
 └── Safe
```

---

# GRADING

## Purpose

Tracks grading submissions and outcomes.

One card may be submitted multiple times over its lifecycle.

### Primary Key

| Field | Type |
|--------|------|
| GradingID | UUID |

### Foreign Keys

| Field | References |
|--------|------------|
| CardID | Cards |

### Fields

| Field | Type | Description |
|--------|------|-------------|
| GradingCompany | Enum | PSA, SGC, BGS, CGC |
| SubmissionNumber | Text | Submission reference |
| SubmissionDate | Date | Date shipped |
| ReceivedDate | Date | Date received |
| Grade | Text | Final grade |
| CertificationNumber | Text | Certification ID |
| Status | Enum | Preparing, Submitted, Grading, Returned |
| Cost | Currency | Grading cost |
| Notes | Text | Internal notes |

### Relationships

```
Card (1)
      │
      ▼
Grading (*)
```

---

# Enumerations

## Acquisition Type

- Wax
- Single Purchase
- Collection Purchase
- Trade
- Gift
- Other

---

## Card Status

- In Inventory
- Listed
- Sold
- At Grading

---

## Listing Status

- Draft
- Active
- Sold
- Ended
- Cancelled

---

## Grading Status

- Preparing
- Submitted
- Received
- Grading
- Returned

---

# Deferred Tables

These tables are intentionally excluded from Version 1.

- Customers
- Card Definitions
- Manufacturers
- Players
- Teams
- Sets
- Parallel Definitions
- Price History
- Shows
- Purchase Orders
- Consignments

A deferred table should only be added when it:

1. Reduces operational effort
2. Improves business decisions
3. Enables new capabilities

---

# Version History

| Version | Date | Notes |
|----------|------|-------|
| 1.0 | Initial Design | Initial operational data dictionary |
