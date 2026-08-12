# Settings: Color Palettes

Audience: End users
Last reviewed: 2026-04-22
Routes: `/settings`
Settings tab: **Color Palettes**
Generation target: Settings Color Palettes training material

## Purpose

The **Color Palettes** tab lets studio admins create, edit, search, and delete custom palettes used by widget chart settings.

## Prerequisites

- You must open **Settings**.
- You must be an admin for at least one studio.
- The **Color Palettes** tab only appears when you administer at least one studio.
- A studio must be selected before custom palettes can be managed.

## Main User Workflows

### Select A Studio

1. Open **Settings**.
2. Select **Color Palettes**.
3. Use the studio dropdown to choose a studio.

If no studio is selected, the table shows **Select a studio to view custom palettes.**

### Find A Palette

1. Open **Settings**.
2. Select **Color Palettes**.
3. Choose a studio.
4. Type in **Search palettes**.
5. Review the table columns **Name** and **Colors**.

### Add A Palette

1. Open **Settings**.
2. Select **Color Palettes**.
3. Choose a studio.
4. Click **Add Palette**.
5. In **Add custom palette**, enter **Palette Name**.
6. Review the color rows.
7. Click **Add color** if you need more colors.
8. Set each color as a valid hex value, such as `#2563eb`.
9. Click **Add palette**.

A palette needs at least three colors.

### Edit A Palette

1. Open **Settings**.
2. Select **Color Palettes**.
3. Choose a studio.
4. Find the palette row.
5. Click the edit icon.
6. In **Edit color palettes**, update **Palette Name** or color values.
7. Click **Add color** or remove color rows as needed.
8. Click **Save changes**.

### Delete A Palette

1. Open **Settings**.
2. Select **Color Palettes**.
3. Choose a studio.
4. Find the palette row.
5. Click the delete icon.
6. In **Delete Custom Palette**, confirm the delete action.

If the palette is used by widgets, the modal shows **Palette In Use**. Choose **Replacement Palette** and then click **Delete and reassign**.

## Feature Reference

- **Select studio** controls which studio's custom palettes are shown.
- **Add Palette** opens the custom palette modal.
- **Search palettes** filters the palette table.
- **Colors** displays the palette swatches.
- **Palette Name** is required and can be up to 100 characters.
- Each color must be a valid hex color.
- A custom palette must contain at least three colors.
- Delete conflicts list affected widgets and link to the relevant widget records.

## Permissions And Availability

- Only studio admins can see and use the **Color Palettes** tab.
- **Add Palette** is disabled until a studio is selected.
- Custom palettes are studio-specific.
- Widgets that use a deleted custom palette must be reassigned to an active replacement palette before deletion can complete.

## Troubleshooting

### Color Palettes Is Missing

You are not an admin for any studio.

### Add Palette Is Disabled

Select a studio from the studio dropdown.

### A Color Value Is Rejected

Use a valid hex color such as `#2563eb`.

### Delete Requires A Replacement Palette

One or more widgets still use the palette. Choose an active replacement palette before deleting.

## Related Routes And Terms

- Route: `/settings`
- Tab: **Color Palettes**
- Related terms: palette, custom palette, colors, widget colors, chart colors, replacement palette
