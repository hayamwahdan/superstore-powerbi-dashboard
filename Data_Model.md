# Data Model

Overview of tables, columns, relationships, and the date hierarchy used in the Superstore Report.

---

## Entity Relationship Diagram

```
┌──────────────┐       ┌──────────────┐
│   People     │       │  Date Table  │
│──────────────│       │──────────────│
│ Person       │       │ Date         │
│ Region     ◄─┼───┐   │ Year         │
│ E-Mail       │   │   │ Quarter      │
│ DOMAIN\User  │   │   │ Month Name   │
└──────────────┘   │   │ Month Number │
                   │   │ Day Name     │
┌──────────────┐   │   │ Day of Week  │
│   Orders     │   │   └──────┬───────┘
│──────────────│   │          │
│ Order ID     │   │          │ Date relationship
│ Order Date ──┼───┼──────────┘
│ Customer ID  │   │
│ Customer Name│   │
│ Segment      ├───┘ Region relationship
│ Ship Mode    │
│ Category     │
│ Sub-Category │
│ Amount       │
│ Profit       │
│ Rank         │
└──────┬───────┘
       │
       │ Order ID relationship
       │
┌──────┴───────┐
│   Returns    │
│──────────────│
│ Order ID     │
│ Returned     │
└──────────────┘
```

---

## Tables

### Orders (Fact Table)

The primary fact table containing all sales transactions.

| Column | Type | Description |
|--------|------|-------------|
| Order ID | Text | Unique order identifier |
| Order Date | Date | Date of the order |
| Customer ID | Text | Unique customer identifier |
| Customer Name | Text | Customer full name |
| Segment | Text | Consumer / Corporate / Home Office |
| Ship Mode | Text | Standard Class / Second Class / First Class / Same Day |
| Category | Text | Furniture / Office Supplies / Technology |
| Sub-Category | Text | Product sub-category (17 distinct values) |
| Amount | Decimal | Sales amount in USD |
| Profit | Decimal | Profit in USD |
| Rank | Integer | Customer ranking by amount (calculated column) |

### Returns (Fact Table)

Tracks which orders were returned.

| Column | Type | Description |
|--------|------|-------------|
| Order ID | Text | References Orders[Order ID] |
| Returned | Text | "Yes" if the order was returned |

### People (Dimension Table)

Maps regional managers to regions. Also used for RLS.

| Column | Type | Description |
|--------|------|-------------|
| Person | Text | Regional manager name |
| Region | Text | Central / East / South / West |
| E-Mail | Text | User email (for RLS display) |
| DOMAIN\User | Text | Domain username (for RLS display) |

### Date Table (Calculated Dimension)

Auto-generated calendar table with a drill-down hierarchy.

| Column | Type | Description |
|--------|------|-------------|
| Date | Date | Calendar date |
| Year | Integer | Year (2016–2019) |
| Quarter | Text | Q1 / Q2 / Q3 / Q4 |
| Month Name | Text | January–December |
| Month Number | Integer | 1–12 (for sorting) |
| Day Name | Text | Monday–Sunday |
| Day of Week | Integer | 1–7 (for sorting) |

---

## Relationships

| From | To | Cardinality | Cross-filter | Purpose |
|------|----|-------------|--------------|---------|
| Orders[Order Date] | Date Table[Date] | Many-to-One | Single | Time intelligence & hierarchy drill |
| Orders[Region] | People[Region] | Many-to-One | Single | Region manager lookup & RLS |
| Orders[Order ID] | Returns[Order ID] | One-to-Many | Single | Return status lookup |

---

## Date Hierarchy

The `Date Table` exposes a four-level hierarchy used in the Area Chart and Month Slicer:

```
Year (2016, 2017, 2018, 2019)
  └── Quarter (Q1, Q2, Q3, Q4)
        └── Month Name (January ... December)
              └── Day Name (Monday ... Sunday)
```

The Year Slicer on the report page filters the Date Table by Year with a default selection of all four years (2016–2019).
