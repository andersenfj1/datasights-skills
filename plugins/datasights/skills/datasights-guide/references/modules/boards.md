# Boards

Audience: End users
Last reviewed: 2026-05-12
Routes: `/boards`, `/boards/:boardId`, `/boards/:boardId/export`, `/embed/boards/:boardId`, `/embed/boards/:boardId/export`

## Purpose

Boards are dashboard workspaces made of panels and widgets. Use Boards to organize saved widgets, apply board-level filters, share dashboards, and export dashboards to CSV, Excel, PDF, or PowerPoint.

## Prerequisites

- You must be signed in and have an active room selected.
- Users with view access can open boards.
- You need edit access in the active room to add, rename, duplicate, delete, share, filter, add widgets, or manage panels.
- Widgets must exist before they can be added to a board panel.

## Main User Workflows

### Find A Board

1. Open **Boards** from the app navigation.
2. Type in **Search boards** when needed.
3. Review **Name**, **Panels**, and **Created At**.
4. Click the board name to open it.

If no room is selected, the page shows **Select a room from the user menu to view boards.** If no boards exist, it shows **No boards found.**

### Add A Board

1. Open **Boards**.
2. Click **Add**.
3. In **Add board**, enter **Board name**.
4. Click **Save**.
5. Open the board and add widgets or panels as needed.

Board names are required and must be 255 characters or fewer.

### Rename A Board

From the board list:

1. Open **Boards**.
2. Click the edit icon in the board row.
3. In **Edit name**, enter the new name.
4. Click **Save**.

From the board detail page:

1. Open the board.
2. Open **Settings**.
3. Click **Edit name**.
4. Enter the new name.
5. Click **Save**.

### Duplicate A Board

1. Open **Boards** or open the board detail page.
2. Click the duplicate icon, or open **Settings** and click **Duplicate**.
3. In **Duplicate board**, enter **Name**.
4. Choose **Destination room**.
5. If shown, choose whether to **Include applied filters**.
6. Click **Duplicate**.

Only editable rooms in the same studio are available as destinations.

### Delete A Board

1. Open **Boards**.
2. Find the board row.
3. Click the delete icon.
4. In **Delete board?**, confirm **Delete**.

### Add Or Rename Panels

1. Open a board.
2. Use the panel tabs under the board title.
3. Click **Add** near the panel tabs to create a new panel.
4. Double-click an active panel tab, or press `F2` while it is active, to rename it.
5. Drag panel tabs to reorder panels.

Boards must keep at least one panel.

### Add Widgets To A Board

1. Open a board.
2. Open **Settings**.
3. Click **Add widget**.
4. Choose the widgets to add to the active panel.
5. Arrange widgets in the panel grid.

Widgets can also be added from the widget editor using **Add to board**.

### Configure Board Filters

1. Open a board.
2. Open **Settings**.
3. Click **Filters**.
4. In **Board filters**, click **Add filter**.
5. Select **Orientation**: **Horizontal** or **Vertical**.
6. Choose whether filters should **Start collapsed**.
7. For each filter, choose a **Model** and then a **Column**.
8. Click **Save**.

Users can then choose filter values, click **Apply filters**, use **Save as default**, or click **Clear filters**. Click-generated filters can be removed individually or cleared with **Clear click filters**.

### Share A Board

1. Open a board.
2. Open **Settings**.
3. Click **Share board**.
4. Click **Enable sharing** if sharing is disabled.
5. Use **Share URL** or **Iframe Embed Snippet**.
6. Click **Copy** to copy the share URL.
7. Use **Refresh link** to rotate the link, or **Disable sharing** to revoke it.

Disabling sharing makes any current iframe or URL stop working.

Embedded boards can receive host-supplied runtime filters from the parent application. If the host sends no filters, the embedded board loads like a normal shared board. When host filters are applied, manual board filter dropdowns load their values inside that external filter context, and embedded exports use the same filtered data the viewer sees. The developer handoff is documented in [External App Embed Filter Handoff](../external-app-embed-filter-handoff.md).

### Export Or Download A Board

1. Open a board.
2. Open **Settings**.
3. Choose **Download CSV**, **Download Excel**, **Export to PDF**, or **Export to PowerPoint**.
4. For PDF or PowerPoint, choose **Active panel**, **All panels**, or **Custom selection**.
5. Click **Export**.

PDF and PowerPoint exports can include selected panels only.

## Feature Reference

- **Settings** opens board-level actions.
- **Download CSV** and **Download Excel** export board data.
- **Export to PDF** and **Export to PowerPoint** render dashboard panels.
- **Share board** manages public share links and iframe embeds.
- Shared iframe embeds can receive host-supplied runtime filters from the parent application.
- Embedded board filter dropdown values are constrained by host-supplied filters.
- **Re-render thumbnail** refreshes the board thumbnail used in navigation.
- **Filters** configures manual board filters.
- **Apply filters**, **Save as default**, and **Clear filters** control runtime filter selections.
- **Show filters** and **Hide filters** control the vertical filter drawer when filters are vertical.

## Permissions And Availability

- View-only users can open and view boards but cannot edit layout, add widgets, manage panels, share, duplicate, or delete.
- The board list shows **View only** when you cannot edit.
- Export and download actions are available from the board detail page.
- Sharing and filter configuration require edit access.
- A panel cannot be deleted when it is the only panel on the board.

## Troubleshooting

### Add Is Missing Or Disabled

Select a room and confirm that you have edit access.

### Add Widget Is Disabled

Wait for panels to load, or create a panel first.

### No Models On This Board Appears In Filters

Add widgets that use models to the board before creating board filters.

### Export Fails Or Looks Incomplete

Confirm that the widgets render correctly on the board and that the selected panels contain the expected widgets.

## Related Routes And Terms

- Routes: `/boards`, `/boards/:boardId`, `/boards/:boardId/export`, `/embed/boards/:boardId`
- Related terms: board, dashboard, panel, add widget, board filter, share board, iframe, embedded filters, export PDF, export PowerPoint, download CSV, download Excel
