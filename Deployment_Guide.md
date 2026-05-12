# Deployment Guide

End-to-end steps for publishing the Superstore Report and configuring it in Power BI Service.

---

## Prerequisites

- **Power BI Desktop** (April 2026 or later)
- **Power BI Pro** or **Premium Per User** license
- A **workspace** in Power BI Service where you have Admin or Member access

---

## 1. Open & Review Locally

1. Open `Superstore_report.pbix` in Power BI Desktop
2. Verify data loads correctly (Home → Refresh)
3. Check all visuals render without errors
4. Test RLS locally via Modeling → View as Roles (see [`RLS_Setup.md`](RLS_Setup.md))

---

## 2. Publish to Power BI Service

1. In Power BI Desktop, click **Home → Publish**
2. Select your target workspace
3. Wait for the confirmation dialog — click **Open in Power BI** to verify

This uploads both the **report** and the **dataset** to the workspace.

---

## 3. Configure the Dataset

### Data Source Credentials

If your data source requires authentication (SQL Server, SharePoint, etc.):

1. Go to **Workspace → Settings (⚙)** on the dataset
2. Under **Data source credentials**, click **Edit credentials**
3. Enter the appropriate credentials and sign in

### Scheduled Refresh (Optional)

If data needs periodic updates:

1. In dataset **Settings → Scheduled refresh**
2. Toggle **Keep your data up to date** to On
3. Set the refresh frequency and time zone
4. Add failure notification email addresses
5. Click **Apply**

---

## 4. Configure Row-Level Security

1. In the workspace, find the **dataset** (not the report)
2. Click **⋯ → Security**
3. Select the RLS role (`RegionRole`)
4. Add members by email
5. Save

Full details in [`RLS_Setup.md`](RLS_Setup.md).

---

## 5. Share the Report

### Option A — Direct Sharing

1. Open the report in Power BI Service
2. Click **Share** in the top toolbar
3. Enter recipient emails
4. Choose permissions (view only, or allow resharing)
5. Click **Send**

### Option B — Create an App

1. In the workspace, click **Create app**
2. Configure the app name, description, and branding
3. Under **Content**, select which reports/dashboards to include
4. Under **Access**, define who can access the app
5. Click **Publish app**

### Option C — Embed (for developers)

Use the Power BI Embedded REST APIs or the JavaScript embed SDK to embed the report in a custom application. Ensure RLS is enforced by passing the effective identity in the embed token.

---

## 6. Post-Deployment Checklist

- [ ] Report opens without errors in Power BI Service
- [ ] All visuals load and filter correctly
- [ ] RLS roles are assigned to the correct members
- [ ] RLS is tested — each member sees only their region
- [ ] Scheduled refresh is configured (if needed)
- [ ] Report is shared or published as an app
- [ ] User Info Card displays the correct identity for each viewer

---

## Workspace & Artifact IDs

| Artifact | ID |
|----------|----|
| Dataset | `63bc5b01-bc7e-4a99-b48f-c6556d81532e` |
| Report | `a54f557c-a466-43cd-a5fd-5c0043061e0a` |

These IDs are stable after the first publish and are useful for API calls, embedding, and automation scripts.
