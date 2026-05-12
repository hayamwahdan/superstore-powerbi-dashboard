# DAX Measures

All custom DAX measures used in the Superstore Report.

---

## People Table Measures

### Current E-Mail

Returns the email address of the currently logged-in user (resolved through RLS).

```dax
Current E-Mail =
VAR CurrentUser = USERPRINCIPALNAME()
RETURN
    SELECTEDVALUE(People[E-Mail], CurrentUser)
```

**Used in:** User Info Card (Page 1)

---

### Current DOMAIN\User

Returns the domain\username of the currently logged-in user.

```dax
Current DOMAIN\User =
VAR CurrentUser = USERNAME()
RETURN
    SELECTEDVALUE(People[DOMAIN\User], CurrentUser)
```

**Used in:** User Info Card (Page 1)

---

### Profit Margin

Calculates profit margin as a percentage of total amount.

```dax
Profit Margin =
DIVIDE(
    SUM(Orders[Profit]),
    SUM(Orders[Amount]),
    0
)
```

**Format:** `0.00%;-0.00%;0.00%`

**Used in:** KPI Card (Page 1)

---

## Date Table (Calculated Table)

The `Date Table` is a calculated table that generates a complete date dimension with the following hierarchy:

**Hierarchy:** Year → Quarter → Month Name → Day Name

```dax
Date Table =
ADDCOLUMNS(
    CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Quarter", "Q" & QUARTER([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Month Number", MONTH([Date]),
    "Day Name", FORMAT([Date], "dddd"),
    "Day of Week", WEEKDAY([Date])
)
```

> **Note:** The exact DAX for the Date Table may vary. The above is the standard pattern used for Superstore datasets. Verify in Power BI Desktop by inspecting the table definition.

---

## Implicit Measures (Aggregations Used in Visuals)

These are not standalone DAX measures but are column aggregations configured directly on visuals:

| Visual | Aggregation | Expression |
|--------|-------------|------------|
| KPI Card | Sum of Amount | `SUM(Orders[Amount])` |
| KPI Card | Sum of Profit | `SUM(Orders[Profit])` |
| KPI Card | Distinct count of Order ID | `DISTINCTCOUNT(Orders[Order ID])` |
| KPI Card | Distinct count of Customer ID | `DISTINCTCOUNT(Orders[Customer ID])` |
| KPI Card | Min of Returned | `MIN(Returns[Returned])` |
| Bar Charts | Sum of Amount | `SUM(Orders[Amount])` |
| Area Chart | Sum of Amount | `SUM(Orders[Amount])` |
| Column Chart | Sum of Amount | `SUM(Orders[Amount])` |

---

## Amount & Profit Format

Both Amount and Profit columns use the currency format:

```
\$#,0.00;(\$#,0.00);\$#,0.00
```
