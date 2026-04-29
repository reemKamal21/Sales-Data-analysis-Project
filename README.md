# Sales Data Analysis — Excel
> *An end-to-end retail sales analysis built in Excel — using Power Query for data cleaning, Power Pivot for modelling, Pivot Tables for aggregation, and an interactive Dashboard to explore revenue across categories, brands, branches, and payment methods.*

---

## ⚙️ Project Type Flags

- [x] Exploratory Data Analysis (EDA)
- [ ] SQL Analysis / Querying
- [x] Dashboard / Data Visualization
- [x] Data Pipeline / ETL
- [ ] Predictive Modelling / Machine Learning
- [x] Data Cleaning / Wrangling
- [x] End-to-End (multiple of the above)

---

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Repository Structure](#4-repository-structure)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [Analysis & Metrics](#7-analysis--metrics)
8. [Key Insights](#8-key-insights)
9. [Recommendations](#9-recommendations)
10. [Assumptions & Limitations](#10-assumptions--limitations)
11. [Future Enhancements](#11-future-enhancements)
12. [Author](#13-author)

---

## 1. Project Overview

**Context:** A retail business runs five branches and sells products across five categories (Fashion, Electronics, Beauty, Home, Sports) from five different brands. Each sale is recorded with customer, product, branch, and payment information.

**Problem Statement:** There was no single view of sales performance. The goal was to build something that lets you quickly answer questions like: which category brings in the most revenue? Which branch is underperforming? How do sales move across the year?

**Approach:** I cleaned and loaded six related tables through Power Query, built a relational data model in Power Pivot using a star schema, wrote DAX measures for the key KPIs, then created eight Pivot Tables and a dashboard with charts and slicers to make everything interactive.

**Outcome:** A working Excel dashboard showing **$974,693 in total sales from 638 customers**, broken down by category, brand, branch, payment method, and month — with five slicers to filter everything in real time.

---

## 2. Objectives

- **Primary Objective:** Build an interactive dashboard that gives a clear picture of annual sales performance across all key dimensions.
- **Secondary Objective 1:** Identify which product category, brand, and branch generate the most revenue.
- **Secondary Objective 2:** Analyse monthly sales to spot any seasonal patterns across the year.
- **Secondary Objective 3:** Break down revenue by payment method to understand how customers are paying.



---

## 3. Project Scope & Tools

### Scope

| Dimension | Details |
|-----------|---------|
| **In Scope** | Sales transactions across 5 branches, 5 brands, 5 product categories, and 4 payment methods — full calendar year (January–December) |
| **Out of Scope** | Customer demographics beyond ID; product SKU-level detail; cost or profit margin data |
| **Time Period** | Full calendar 2 years — January 2023 through December 2024 (24 months) |
| **Granularity** | Individual transactions, rolled up to category, brand, branch, payment method, and monthly level for reporting |

### Tools & Technologies

| Category | Tool(s) Used |
|----------|-------------|
| Data Storage | Excel Workbook (.xlsx) |
| Data Cleaning | Power Query (M language) — 6 queries |
| Data Modelling | Power Pivot (in-memory data model, DAX) |
| Analysis | Pivot Tables — 8 total, all connected to the Power Pivot model |
| Visualization | Excel Charts (3 × Bar Charts, 1 × Line Chart) + 5 Slicers |
| Version Control | — |
| Documentation | This README (Markdown) |

---

## 4. Repository Structure

```
sales-data-analysis/
│
├── Sales_Data_Analysis.xlsx   # Main workbook — Power Query, Power Pivot, Pivot Tables, Dashboard
│
└── README.md                  # Project documentation (this file)
```

---

## 5. Data Workflow

```
[Raw Source Tables — Sales, Customers, Products, Branches, Brands, Payment Methods]
      ↓
[Power Query — Clean and load each table as a named query (6 total)]
      ↓
[Power Pivot — Build star schema, relate tables, create Calendar table, write DAX measures]
      ↓
[Pivot Tables (8) — Slice data by Category, Brand, Branch, Payment Method, Month]
      ↓
[Dashboard — 4 Charts + 5 Slicers connected to all Pivot Tables]
```

1. **Source:** Six tables inside the workbook — `All sales` (the fact table), plus `Customers`, `Products`, `Branches`, `Brands`, and `Payment method` as dimension tables.

2. **Ingestion:** Each table was connected through Power Query as a separate named query, loaded in connection-only mode so the data feeds directly into Power Pivot without landing on a worksheet.

3. **Cleaning:** For each query I promoted headers, set the correct data types (dates, decimals, integers), removed any blank rows, and made sure text values like category names and branch names were consistent.

4. **Transformation:** In Power Pivot, I linked the six tables using their shared ID keys to build a star schema. I also added a `Calendar` table with a date hierarchy (Year → Month → Date) and wrote two DAX measures: **Total Sales** (`SUM` of TotalAmount) and **Total Customers** (`DISTINCTCOUNT` of CustomerID).

5. **Analysis:** Eight Pivot Tables pull from the Power Pivot model, each breaking down the measures by a different dimension — Category, Brand, Branch, Payment Method, Month, and a Brand × Category cross-tab. All Pivot Tables are connected to the same five slicers so filtering one updates everything.

6. **Output:** A Dashboard sheet with four charts, five slicers, and summary KPI cards (Total Sales: $974,693.16 | Total Customers: 638). All Pivot Tables sit on a separate `pivot tables` sheet behind the dashboard.

---

## 6. Data Model & Schema

The model uses a **star schema** — one fact table (`All sales`) at the centre, surrounded by five dimension tables, plus a `Calendar` table for date-based analysis.

### Fact Table: `All sales`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `SaleID` | Integer | Unique ID for each transaction | 1001 |
| `CustomerID` | Integer | Links to the Customers table | 201 |
| `ProductID` | Integer | Links to the Products table | 305 |
| `BranchID` | Integer | Links to the Branches table | 3 |
| `PaymentMethodID` | Integer | Links to the Payment method table | 2 |
| `TotalAmount` | Decimal | Revenue amount of the transaction | 452.75 |
| `DateColumn` | Date | Date the sale occurred | 15/03/2024 |

> **Date range:** January – December (full year) | **Primary key:** `SaleID`

---

### Dimension Table: `Customers`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `CustomerID` | Integer | Unique customer identifier | 201 |
| `CustomerName` | String | Customer's full name | Ahmed Hassan |

---

### Dimension Table: `Products`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `ProductID` | Integer | Unique product identifier | 305 |
| `Category` | String | Product category | Fashion |
| `BrandID` | Integer | Links to the Brands table | 2 |

---

### Dimension Table: `Brands`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `BrandID` | Integer | Unique brand identifier | 2 |
| `BrandName` | String | Brand name | Brand B |

---

### Dimension Table: `Branches`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `BranchID` | Integer | Unique branch identifier | 3 |
| `BranchName` | String | Branch name | Branch 3 |

---

### Dimension Table: `Payment method`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `PaymentMethodID` | Integer | Unique payment method identifier | 2 |
| `PaymentMethod` | String | Payment type label | Credit Card |

---

### Calendar Table (Power Pivot)

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `DateColumn` | Date | Date key — relates to the fact table | 15/03/2024 |
| `Month` | String | Month name | March |
| `Year` | Integer | Calendar year | 2024 |

> **Hierarchy defined:** `Date Hierarchy` → Year > Month > DateColumn

---

### DAX Measures

| Measure | Logic | What It Does |
|---------|-------|--------------|
| `Total Sales` | `SUM([TotalAmount])` | Adds up all transaction values |
| `Total Customers` | `DISTINCTCOUNT([CustomerID])` | Counts unique customers |
| `Sum of TotalAmount` | `SUM([TotalAmount])` | Used in dimension-level Pivot Tables |

---

### Relationship Map

| From Table | Join Key | To Table | Type |
|------------|----------|----------|------|
| `All sales` | `CustomerID` | `Customers` | Many-to-One |
| `All sales` | `ProductID` | `Products` | Many-to-One |
| `All sales` | `BranchID` | `Branches` | Many-to-One |
| `All sales` | `PaymentMethodID` | `Payment method` | Many-to-One |
| `Products` | `BrandID` | `Brands` | Many-to-One |
| `All sales` | `DateColumn` | `Calendar` | Many-to-One |

---

## 7. Analysis & Metrics

### Analytical Approach

This is an exploratory analysis — I wasn't testing a specific hypothesis. The aim was to get a complete picture of how revenue is distributed across every dimension in the dataset, then surface that through an interactive dashboard so others can explore it themselves.

### Key Metrics

| Metric | What It Measures | Why It's Useful |
|--------|-----------------|-----------------|
| `Total Sales` | Sum of all transaction amounts in the current filter context | The main KPI — overall revenue |
| `Total Customers` | Count of distinct customers in the current filter context | Shows reach; separates volume-driven revenue from a small number of big buyers |
| `Sales per Category` | Total Sales grouped by product category | Shows which product type brings in the most money |
| `Sales per Brand` | Total Sales grouped by brand | Shows brand-level contribution to revenue |
| `Sales per Branch` | Total Sales grouped by branch | Shows which locations are doing well and which aren't |
| `Sales per Payment Method` | Total Sales grouped by payment type | Useful for understanding customer payment preferences |
| `Sales per Month` | Total Sales grouped by month | Highlights seasonal trends over the year |

### Methods Used

- Aggregation via DAX (`SUM`, `DISTINCTCOUNT`) in Power Pivot
- Dimensional slicing across six dimensions using Pivot Tables
- Monthly trend analysis using a Line Chart
- Brand × Category cross-tab to find where each brand is strongest
- Five interconnected slicers (Category, Brand, Branch, Payment Method, Date Hierarchy) for live filtering across all charts and Pivot Tables

---

## 8. Key Insights

**Insight 1: Fashion leads by category, but all five categories are fairly close**
Fashion is top at $221,302, Sports is last at $180,259 — a gap of about $41,000 on a ~$975K total. That's a 23% spread from first to last. No single category is carrying the business, which suggests the product mix is reasonably balanced.

**Insight 2: Brand D and Brand B lead overall, but each has a different home category**
Brand D is first at $219,828, with strong numbers in Electronics ($50,416) and Sports ($48,292). Brand B is second at $212,339, and its biggest number in the Brand × Category cross-tab is Fashion at $66,804 — the highest single cell in that whole table. These brands aren't uniformly strong; they each have one category where they clearly outperform the others.

**Insight 3: Branch 3 is the top location; Branch 2 is the weakest**
Branch 3 brought in $205,153 and Branch 2 $186,560. The gap is about $18,600 — not huge in absolute terms, but worth digging into to understand whether it's a stocking, footfall, or staffing issue rather than just accepting it as the norm.

**Insight 4: January and August are the two strongest months; May and November are the slowest**
January peaks at $97,353 and August at $94,159. May ($75,055) and November ($74,303) are the two weakest months. That's roughly a $23,000 swing between peak and trough months — meaningful if the business has fixed costs to cover in slow periods.

**Insight 5: All four payment methods are in active use, with PayPal slightly ahead**
PayPal leads at $268,474, then Credit Card at $248,973, Cash at $233,038, and Bank Transfer at $224,208. The spread is about $44,000. Nothing dramatic here — customers are using all four options — but PayPal's lead is worth noting.

---

## 9. Recommendations

| Priority | Recommendation | Based On | Suggested Owner |
|----------|---------------|----------|-----------------|
| High | Look into what's driving January and August peaks — whether it's promotions, specific products, or seasonality — and try to replicate those conditions in May and November to lift the slower months | Insight 4 — monthly trend | Sales / Marketing |
| High | Since Brand B consistently wins in Fashion and Brand D in Electronics, it makes sense to stock and promote them more heavily in those categories rather than spreading them evenly across all five | Insight 2 — brand × category breakdown | Merchandising |
| Medium | Investigate Branch 2 by checking whether it's under-stocked in Fashion and Electronics. If those are its weakest categories, it could be a product availability issue rather than a fundamental location problem | Insight 3 — branch comparison | Operations |
| Medium | Bank Transfer is the lowest payment method. If it involves manual processing, it may be worth exploring whether those customers can be guided toward PayPal or card to reduce back-office work | Insight 5 — payment methods | Finance |
| Low | A useful next step would be calculating average transaction value per customer ($974,693 ÷ 638 = ~$1,528). Understanding whether revenue is driven by frequency or order size would add useful context to everything else here | General — customer KPIs | Data / Analytics |

---

## 10. Assumptions & Limitations

### Assumptions
- The six source tables are assumed to be complete. I didn't validate row counts against any external source.
- `TotalAmount` is treated as the final sale value after any discounts — there's no separate discount or tax field in the data.
- The `Calendar` table covers one year only. The model isn't set up for multi-year comparisons without changes to the date dimension.
- Category names, brand names, and branch names are assumed to be consistent throughout the year.

### Limitations
- No cost data is included, so this is a revenue-only analysis. A high-revenue category isn't necessarily the most profitable one.
- Customer data is ID-only. Without demographics or location data, it's not possible to segment customers meaningfully beyond branch level.
- One year of data limits how much can be said about trends. The January and August peaks might be recurring patterns, or they might not be — there's no second year to compare.
- The deepest cross-dimensional view available is Brand × Category. Combinations like Branch × Brand or Month × Category would require additional Pivot Tables that aren't currently in the model.
- The Power Pivot data model and DAX measures are only accessible in Excel for Microsoft 365 or Excel 2019+. Users on older versions may not see the full functionality.

---

## 11. Future Enhancements

- [ ] Add a second year of data and build Year-over-Year DAX measures (e.g., `Sales YoY %`) to turn this from a snapshot into a proper trend analysis
- [ ] Add cost or margin data to the Products table and create a `Gross Profit` measure in Power Pivot
- [ ] Build a Branch × Category cross-tab Pivot Table to identify which branches are over- or under-indexed in specific product categories
- [ ] Add a customer spend tier field (high / mid / low value) derived in Power Query, usable as a slicer dimension
- [ ] Migrate to Power BI for easier sharing, scheduled data refresh, and better handling of larger datasets going forward

---

## 13. Author

**Reem Ewis**
Data Analyst

- 🔗 gmail: reem.aweys21@gmail.com

---

*Last updated: April 2026*
