# Brewer Bros Cards Operating System (BBOS)

## Purpose

The Brewer Bros Cards Operating System (BBOS) is designed around a single guiding principle:

> Every table must reduce operational friction.

The system is intentionally optimized for a small, family-run operation. It prioritizes speed, simplicity, and scalability over exhaustive data modeling.

---

# Design Principles

1. Every table must reduce operational friction.
2. Avoid duplicate data.
3. Store facts once.
4. Derive values whenever possible.
5. Add reference tables only when they improve reporting or reduce maintenance.
6. Optimize for speed of use over theoretical perfection.
7. Every workflow begins when a card enters inventory and ends when it leaves inventory.

---

# System Context

```text
                          Brewer Bros Cards
                       Operating System (BBOS)

                 +-------------------------------+
                 |                               |
                 |       Card Inventory          |
                 |       Operational DB          |
                 |                               |
                 +---------------+---------------+
                                 |
        +------------------------+------------------------+
        |                        |                        |
        ▼                        ▼                        ▼
   Acquisitions             Listings                Grading
        |                        |                        |
        ▼                        ▼                        ▼
      Cards  <------------->  Sales  <-------------> Locations
```

---

# Core Operational Data Model

```text
                           Acquisitions
                                │
                                │ 1
                                │
                                │
                                │ *
                              Cards
        ┌───────────────────────┼────────────────────────┐
        │                       │                        │
        ▼                       ▼                        ▼
   Locations               Listings                 Grading
                                │
                                ▼
                              Sales
```

---

# Entity Relationship Diagram (V1)

```text
+-------------------+
| ACQUISITIONS      |
+-------------------+
| AcquisitionID PK  |
| Source            |
| PurchaseDate      |
| TotalCost         |
| Notes             |
+---------+---------+
          |
          | 1
          | *
+---------v---------+
| CARDS             |
+-------------------+
| CardID PK         |
| AcquisitionID FK  |
| Sport             |
| Year              |
| Brand             |
| Set               |
| Player            |
| CardNumber        |
| Parallel          |
| Rookie            |
| Condition         |
| CostBasis         |
| CurrentLocationFK |
| Status            |
+----+--------+-----+
     |        |
     ▼        ▼
+-----------+ +-------------+
| LOCATIONS | | GRADING     |
+-----------+ +-------------+
| LocationID| | GradingID   |
| ParentID  | | CardID FK   |
| Name      | | Service     |
| Type      | | Submission# |
+-----------+ | Grade       |
              | Status      |
              +-------------+

     |
     ▼

+--------------------+
| LISTINGS           |
+--------------------+
| ListingID PK       |
| CardID FK          |
| Marketplace        |
| ListingDate        |
| AskingPrice        |
| Status             |
+---------+----------+
          |
          | 1
          | 0..1
+---------v----------+
| SALES              |
+--------------------+
| SaleID PK          |
| ListingID FK       |
| SaleDate           |
| SalePrice          |
| MarketplaceFees    |
| ShippingPaid       |
| ShippingCost       |
| BuyerName          |
| OrderNumber        |
+--------------------+
```

---

# Card Lifecycle

```text
Purchase
   │
   ▼
Create Acquisition
   │
   ▼
Create Card
   │
   ▼
Assign Storage Location
   │
┌──┴──────────────┐
▼                 ▼
Grade Card    List For Sale
▼                 ▼
Return Grade  Active Listing
└──────┬──────────┘
       ▼
   Card Sold
       ▼
  Record Sale
       ▼
Remove Inventory
```

---

# Physical Storage Model

```text
Location
│
├── Closet
│
├── Shelf A
│   ├── Row 1
│   │   ├── Box 1
│   │   ├── Box 2
│   │   └── Box 3
│   └── Row 2
│
└── Safe
    ├── Graded Cards
    └── High Value
```

---

# Current V1 Tables

| Table | Purpose |
|--------|---------|
| Acquisitions | Record every event where inventory enters the business |
| Cards | Active inventory and master operational record |
| Listings | Marketplace listings for cards |
| Sales | Completed sales transaction history |
| Locations | Physical storage hierarchy |
| Grading | PSA/SGC/BGS submission tracking |

---

# Deferred Backlog

## V2 Reference Layer

- Card Definitions
- Manufacturers
- Sets
- Players
- Teams
- Parallel Definitions

## V3 Growth Layer

- Customers
- Shows
- Consignments
- Purchase Orders
- Price History
- Market Values
- Marketing
- Want Lists

**Promotion Rule**

A deferred table should only be added when it:

1. Eliminates repetitive manual work.
2. Enables better business decisions.
3. Unlocks a new business capability.

---

# Architecture Vision

```text
Operational Layer
────────────────────────
Acquisitions
Cards
Listings
Sales
Locations
Grading
        │
        ▼
Reporting Layer
────────────────────────
Inventory Value
Profitability
Sales Performance
Aging Inventory
Grading ROI
        │
        ▼
Automation Layer
────────────────────────
Marketplace Sync
Barcode Scanning
Label Printing
Price Updates
Dashboards
Alerts
```

