# Superstore Sales Report — Power BI Project

A Power BI analytics dashboard built on the Superstore dataset, featuring interactive visuals, DAX measures, a date hierarchy, and Row-Level Security (RLS). Published to Power BI Service.

---

## Report Overview

| Item | Detail |
|------|--------|
| **Power BI Version** | 2026.04 (Desktop → Cloud) |
| **Dataset ID** | `63bc5b01-bc7e-4a99-b48f-c6556d81532e` |
| **Report ID** | `a54f557c-a466-43cd-a5fd-5c0043061e0a` |
| **Data Sources** | Orders, Returns, People, Date Table (calculated) |
| **RLS** | Enabled — region-based, mapped to workspace member |

## Data Model

```
Orders ──────┐
             ├──► Date Table  (relationship on Order Date → Date)
Returns ─────┘
People ──────────► Orders     (Region mapping)
```

### Tables & Key Columns

**Orders** — Order ID, Customer ID, Customer Name, Segment, Ship Mode, Category, Sub-Category, Amount, Profit, Rank

**Returns** — Returned (flag)

**People** — Person, Region (used for RLS role mapping)

**Date Table** — Calculated table with hierarchy: Year → Quarter → Month Name → Day Name

## DAX Measures

All custom measures are documented in [`docs/DAX_Measures.md`](docs/DAX_Measures.md).

| Measure | Table | Purpose |
|---------|-------|---------|
| `Current E-Mail` | People | Returns the email of the currently logged-in RLS user |
| `Current DOMAIN\User` | People | Returns the domain\username of the current RLS user |
| `Profit Margin` | People | Calculates profit margin as `SUM(Profit) / SUM(Amount)` |

## Report Page — Page 1

The single-page dashboard contains the following visuals:

1. **User Info Card** — displays the current user's email and domain\username (driven by RLS)
2. **KPI Card** — Amount, Profit, Orders count, Customers count, Returned items count, Profit Margin %
3. **Year Slicer** — filters by year (2016–2019)
4. **Month Slicer** — filters by month with amount labels
5. **Category Slicer** — filters by product category with amount labels
6. **Bar Chart — Segment** — Amount by Customer Segment
7. **Bar Chart — Region** — Amount by Region
8. **Bar Chart — Ship Mode** — Amount by Ship Mode
9. **Customer Ranking Table** — Rank, Customer Name, Sum of Amount
10. **Area Chart — Time Series** — Amount over the Date Table hierarchy (Year → Quarter → Month → Day)
11. **Column Chart — Sub-Category** — Amount by Sub-Category

## Row-Level Security (RLS)

Full RLS setup instructions are in [`docs/RLS_Setup.md`](docs/RLS_Setup.md).

**How it works:** A DAX role filters the `People` table so each user sees only the data for their assigned region. The `Current E-Mail` and `Current DOMAIN\User` measures let the report display which user is currently viewing.

**Workspace member mapping:** After publishing, workspace members are assigned to their RLS role in Power BI Service → Dataset → Security.

## Deployment

See [`docs/Deployment_Guide.md`](docs/Deployment_Guide.md) for the full publish and configuration workflow.

**Quick summary:**

1. Open `Superstore_report.pbix` in Power BI Desktop
2. Load/refresh data
3. Publish to Power BI Service workspace
4. Configure scheduled refresh (if using a live data source)
5. Assign members to RLS roles in Dataset → Security
6. Share dashboard / create an app

## Project Structure

```
Superstore-PowerBI-Project/
├── README.md                  # This file
├── Superstore_report.pbix     # Power BI report file
├── .gitignore                 # Git ignore rules
├── .gitattributes             # Git LFS tracking for .pbix
├── docs/
│   ├── DAX_Measures.md        # All DAX measure definitions
│   ├── Data_Model.md          # Data model & relationships
│   ├── RLS_Setup.md           # Row-Level Security setup guide
│   └── Deployment_Guide.md    # Publishing & configuration steps
└── screenshots/               # Report screenshots (add your own)
    └── .gitkeep
```

## Prerequisites

- Power BI Desktop (April 2026 or later recommended)
- Power BI Pro or Premium Per User license (for publishing and sharing)
- Workspace access in Power BI Service

## License

This project is for educational and portfolio purposes. The Superstore dataset is a publicly available sample dataset.
