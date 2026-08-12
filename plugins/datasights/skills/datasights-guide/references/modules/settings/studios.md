# Settings: Studios

Audience: End users
Last reviewed: 2026-04-22
Routes: `/settings`
Settings tab: **Studios**
Generation target: Settings Studios training material

## Purpose

The **Studios** tab lets admins create, rename, search, and delete studios. A studio is a top-level workspace that contains rooms and studio-level settings.

## Prerequisites

- You must open **Settings**.
- You must be an admin for the studio you want to manage.
- At least one studio must exist for most settings workflows.

## Main User Workflows

### Find A Studio

1. Open **Settings**.
2. Select **Studios**.
3. Type in **Search studios**.
4. Review the table columns **Name**, **Rooms**, and **Created At**.

If no studios exist, the table shows **No studios yet.**

### Add A Studio

1. Open **Settings**.
2. Select **Studios**.
3. Click **Add Studio**.
4. In **Add studio**, enter **Name**.
5. Click **Save**.

### Rename A Studio

1. Open **Settings**.
2. Select **Studios**.
3. Find the studio row.
4. Click the edit icon.
5. In **Edit studio**, update **Name**.
6. Click **Save**.

### Delete A Studio

1. Open **Settings**.
2. Select **Studios**.
3. Find the studio row.
4. Click the delete icon.
5. In **Delete studio?**, review the warning.
6. Confirm the delete action.

The delete confirmation warns that deleting a studio also deletes rooms, boards, panels, widgets, sources, and models linked to that studio.

## Feature Reference

- **Search studios** filters the studio table.
- **Add Studio** creates a new studio.
- The **Rooms** column shows how many rooms belong to each studio.
- **View only** appears when you can see the studio but cannot manage it.
- Pagination controls appear when the list has more rows than the current page size.

## Permissions And Availability

- Creating a studio requires admin-level access in the settings area.
- Editing or deleting a studio requires admin access for that studio.
- The application prevents deleting the only remaining studio.
- After deleting the active studio, the app selects another available studio when possible.

## Troubleshooting

### Add Studio Is Disabled

Your current access does not allow studio creation.

### A Studio Row Shows View Only

You can see the studio but are not an admin for that studio.

### Delete Studio Fails

The studio may be the only studio, or your role may not allow deleting it.

## Related Routes And Terms

- Route: `/settings`
- Tab: **Studios**
- Related terms: studio, add studio, edit studio, rename studio, delete studio, workspace
