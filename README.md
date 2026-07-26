# Brewer Bros Analytics

## Overview

Brewer Bros Analytics is an end-to-end data platform built to manage and analyze the operations of Brewer Bros Cards & Collectibles. While it serves as the operational system for a real sports card business, the project is also designed as a hands-on learning environment for modern data engineering, analytics, and product development.

The goal is not simply to track inventory. The goal is to create an analytics platform that improves business decisions while providing a realistic sandbox for experimenting with enterprise data technologies.

---

# Vision

Build a modern analytics platform that enables Brewer Bros Cards & Collectibles to operate with the same data-driven discipline as a larger retail business.

Every purchase, card, grading submission, listing, sale, and customer interaction should become structured data that can be analyzed to answer meaningful business questions.

---

# Project Goals

## Business Goals

* Maintain an accurate inventory of every card owned.
* Track purchases, sales, and profitability.
* Organize inventory across physical storage locations.
* Understand business performance through meaningful metrics.
* Reduce time spent searching for inventory.
* Support informed buying, grading, and selling decisions.

## Technical Goals

This project is intentionally designed to develop practical experience with modern analytics technologies, including:

* PostgreSQL (Supabase)
* Databricks
* SQL
* Delta Lake
* Data modeling
* ETL pipelines
* Power BI
* GitHub
* Python

The project is built using the same architectural patterns commonly found in enterprise data platforms.

---

# Guiding Principles

## Start Simple

Build the smallest solution that delivers value.

Avoid unnecessary complexity until the business genuinely requires it.

---

## Data First

Every important business event should become structured data.

Examples include:

* Purchasing inventory
* Receiving cards
* Grading submissions
* Listing cards
* Completing sales
* Moving inventory between storage locations

---

## Build Like a Product

Every feature should solve a real operational problem.

New functionality should only be introduced after validating a business need through actual usage.

---

## Learn by Building

Brewer Bros Analytics exists as both an operational tool and a personal laboratory for exploring modern analytics architecture.

The best way to learn is by solving real business problems with real data.

---

# Initial Scope (Version 1)

Version 1 focuses on establishing a reliable operational database.

Core entities include:

* Cards
* Inventory
* Purchases
* Sales
* Locations

Success is defined by the ability to answer five questions:

1. What inventory do we currently own?
2. Where is each card located?
3. What have we purchased?
4. What have we sold?
5. What inventory remains available for sale?

---

# Future Roadmap

Potential future capabilities include:

* Grading management
* Customer management
* Market value tracking
* Inventory valuation
* Sales analytics
* Profitability reporting
* Dashboarding
* AI-assisted grading recommendations
* Collection purchase analysis
* Inventory turnover analysis
* Mobile-friendly inventory lookup

These features are intentionally deferred until a stable operational foundation exists.

---

# High-Level Architecture

```text
Operational Database (Supabase)
            │
            ▼
      Data Ingestion
            │
            ▼
 Databricks Lakehouse
   Bronze → Silver → Gold
            │
            ▼
     Analytics & Reporting
        (Power BI)
```

---

# Repository Structure

```
brewer-bros-analytics/

├── docs/
├── database/
├── sql/
├── notebooks/
├── dashboards/
└── README.md
```

---

# Current Status

**Phase:** Foundation

Current priorities:

* Design the operational data model.
* Build the PostgreSQL database in Supabase.
* Populate the first inventory records.
* Establish source control.
* Create initial SQL queries for inventory reporting.

---

# Success Metrics

This project will be considered successful when it enables Brewer Bros Cards & Collectibles to operate more efficiently while simultaneously serving as a practical demonstration of modern data engineering and analytics capabilities.

Success is measured not by the number of features implemented, but by the quality of the decisions the platform enables.

---

# License

This repository is intended for personal learning, experimentation, and the operation of Brewer Bros Cards & Collectibles.

