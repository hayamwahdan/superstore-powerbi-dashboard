# Row-Level Security (RLS) Setup

This document explains how Row-Level Security is configured in the Superstore Report, both in Power BI Desktop and Power BI Service.

---

## How RLS Works in This Report

Each person in the `People` table is assigned a **Region** (Central, East, South, West). The RLS role filters the `People` table so that each user only sees data for their assigned region. Because `Orders` is related to `People` through the Region column, the filter propagates automatically — users only see orders, returns, and KPIs for their region.

The **User Info Card** at the top of the report displays the logged-in user's email and domain\username, confirming which identity is active.

---

## Step 1 — Create the Role in Power BI Desktop

1. Open `Superstore_report.pbix` in Power BI Desktop
2. Go to **Modeling** → **Manage Roles**
3. Create a new role (e.g., `RegionRole`)
4. Select the **People** table
5. Add the following DAX filter expression:

```dax
[Person] = USERPRINCIPALNAME()
```

Or, if matching by email:

```dax
[E-Mail] = USERPRINCIPALNAME()
```

This ensures each user sees only rows in the `People` table where their identity matches, and the relationship to `Orders` filters everything downstream.

6. Click **Save**

---

## Step 2 — Test RLS in Power BI Desktop

1. Go to **Modeling** → **View as Roles**
2. Check the role you created (`RegionRole`)
3. Optionally enter a specific user's email in the **Other User** field
4. Click **OK**
5. Verify that the report shows only data for the corresponding region
6. Confirm the User Info Card reflects the test identity
7. Click **Stop viewing** when done

---

## Step 3 — Publish to Power BI Service

1. Click **Publish** in Power BI Desktop
2. Select your target workspace
3. Wait for the publish to complete

---

## Step 4 — Assign Members to the RLS Role in Power BI Service

1. Open **Power BI Service** (app.powerbi.com)
2. Navigate to your **Workspace**
3. Find the **Superstore dataset** (not the report)
4. Click the **three-dot menu (⋯)** → **Security**
5. Select the role (`RegionRole`)
6. In the **Members** section, add workspace members by their email address
7. Click **Add** → **Save**

Each member you add will only see data for the region where their identity matches in the `People` table.

---

## Step 5 — Verify RLS in Power BI Service

1. On the Security page, click **Test as role** next to the role name
2. Optionally enter a specific user's email in **Test as a specific person**
3. Verify the report filters correctly
4. Share the report with the assigned members and ask them to confirm they see only their region's data

---

## RLS Role Summary

| Role | Table | DAX Filter | Effect |
|------|-------|-----------|--------|
| `RegionRole` | People | `[Person] = USERPRINCIPALNAME()` | Filters People to current user → cascades to Orders & Returns via relationships |

---

## Important Notes

- **Admins and Members with Build permission** can see all data unless they are also mapped to an RLS role. If you want admins to be restricted, add them to a role as well.
- **Power BI Pro or Premium** license is required for sharing RLS-protected reports.
- If you add new people to the `People` table, remember to assign them to the RLS role in Power BI Service.
- The `Current E-Mail` and `Current DOMAIN\User` measures use `USERPRINCIPALNAME()` and `USERNAME()` respectively. These only return meaningful values when RLS is active (in View as Role mode or in Power BI Service with a role assigned).
