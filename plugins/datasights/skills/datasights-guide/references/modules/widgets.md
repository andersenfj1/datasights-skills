# Widgets

Audience: End users
Last reviewed: 2026-06-12
Routes: `/widgets`, `/widgets/:widgetId`

## Purpose

Widgets are reusable dashboard elements. A widget can show model data as a chart, table, KPI, pivot table, map, generated summary, custom HTML, or free text. Build and save widgets from the Widgets module, then add them to boards and panels when you want to share them in a dashboard.

## Prerequisites

- You must be signed in and have an active room selected.
- You need edit access in the room to add, rename, duplicate, delete, save, or add widgets to boards.
- Data-backed widgets need a model selected in the widget editor.
- `Summarise` widgets need the AI assistant to be available for the studio.
- `Maps` widgets need map runtime settings to be available for the studio.
- To add a widget to a board, the widget must already be saved and the room must have an available board and panel, or permission to create a panel.

## Main User Workflows

### Create A Widget

1. Open **Widgets** from the app navigation.
2. Select the room you want to work in if no room is active.
3. Click **Add**.
4. Enter a **Name**.
5. Click **Create**.
6. Open the widget from the list by clicking its name.
7. Choose a **Model** if the widget type needs data.
8. Choose a **Widget type** from the row of type icons. The first icon (chart) is selected by default; hover an icon to see the type name. Then configure the query and settings.
9. Click **Run** to preview the output.
10. Click **Save**.

### Open And Edit A Widget

1. Open **Widgets**.
2. Find the widget in the list. Use **Search widgets** when needed.
3. Click the widget name.
4. Change the **Model**, **Widget type**, **Build Query**, or **Settings** sections.
5. Click **Run** to refresh the preview.
6. Click **Save** to keep the changes.

The editor shows a preview area called **Widget**. Use the preview to confirm the output before saving.

### Rename A Widget

1. Open **Widgets**.
2. Click the edit icon in the widget row.
3. Update **Name**.
4. Click **Save**.

Names are required and must be 255 characters or fewer.

### Duplicate A Widget

From the widget list:

1. Open **Widgets**.
2. Click the duplicate icon in the widget row.
3. Confirm or edit the copied **Name**.
4. Choose the destination room when shown.
5. Click the duplicate action.

From the widget editor:

1. Open the widget.
2. Click **Duplicate**.
3. Confirm the copied **Name**.
4. Choose the destination room.
5. Click the duplicate action.

Duplicating copies the saved widget configuration into the destination room.

### Save A Copy

1. Open an existing widget.
2. Click **Save As**.
3. Enter the new widget name.
4. Save the copy.

Use **Save As** when you want to branch an existing widget without changing the original.

### Delete A Widget

1. Open **Widgets**.
2. Click the delete icon in the widget row.
3. Confirm the delete action.

Deleting a widget removes it from the room and any board panels that use it.

### Add A Widget To A Board

1. Open a saved widget.
2. Click **Add to board**.
3. Choose a **Board**.
4. Choose a **Panel**.
5. Click **Add to board** to stay in the editor, or **Add and go to board** to open the board.
6. If the board has no available panels, use **Create panel and add** or **Create panel and go to board**.

### Use The Widget AI Assistant

1. Open a widget you can edit and click the floating **Widget assistant** button in the bottom-right corner.
2. Describe the change you want, for example "swap the axes", "use the muted palette", or "format the x-axis as short dates".
3. Review the proposed **Draft changes** list and untick any change you do not want.
4. Click **Apply selected** to apply the ticked draft changes to the editor.
5. Click **Undo last apply** to revert the most recently applied set of changes.
6. Click **Save** when you are happy; the assistant never saves the widget for you.

Good to know:

- If part of a request could not be turned into a valid draft change, the reply shows a warning saying a proposed change was left out. Rephrase the request or name the exact setting to try again.
- You can also ask how-to questions ("How do I add a widget to a board?"); the assistant answers from this documentation.
- Color requests can use a palette name or a mood such as "muted" or "lighter"; the assistant picks from your studio's palettes.
- Assistant chats are private to your account. Other users who edit the same widget cannot see your conversation.

## Build A Query

Most data-backed widgets use the **Build Query** area.

1. Select a **Model**.
2. In **Selected fields**, click **Add field**.
3. Choose a field and, when needed, an aggregation: `(none)`, `DISTINCT`, `SUM`, `COUNT`, `AVG`, `MIN`, or `MAX`.
4. In **Where**, click **Add condition** to filter rows. Choose a field, an operator, and a value.
5. Use `AND` or `OR` when adding more than one condition.
6. In **Group By**, click **Add group** when you want grouped results.
7. In **Sort**, click **Add sort** and choose `Ascending` or `Descending`.
8. Set **Limit** to control the number of returned rows.
9. Click **Run**.

Available **Where** operators are `=`, `!=`, `>`, `>=`, `<`, `<=`, and `LIKE`.

Pivot table mode is different: the query keeps **Group By** empty and aggregates inside the widget settings.

## Widget Type Reference

### Bar / Line Chart

Use **Bar / line chart** when you want a line, bar, or combo visualization.

- Select a model and build a query with the fields you want to chart.
- In **Settings**, use **Chart mode** to choose `Line`, `Bar`, or `Combo`.
- Use chart settings for style, multi-chart behavior, title, legend, axes, labels, colors, zoom, and layout.
- In **Legend**, use **Legend aliases** to change how each legend target is named without changing the underlying query column or series.
- Bar and combo charts use category, value, and series-style settings.
- To give each bar its own colour, set **Category (series)** to the category field itself: bars stay centred in their category band and each one is coloured from the palette (assign specific colours per category under colour settings).
- Combo charts can mix bar and line series when the data shape supports it.

### Pie Chart

Use **Pie chart** to show parts of a whole.

- Build a query with a category/group field and a numeric value.
- Use layout settings to choose pie style, slice sort, label mode, top-N grouping, and the label used for grouped "other" slices.
- Use title, legend, label, and color settings to tune the display.
- Legend aliases can rename slice/category legend entries for display only.

### Radar Chart

Use **Radar chart** to compare multiple measures across categories.

- Build a query with group fields and numeric values.
- Configure shape, area opacity, split number, and axis labels.
- Enable multi-chart when another group field is available and you want separate radar charts.

### KPI

Use **KPI** for a single headline metric.

- The first returned column becomes the header and the second becomes the value.
- If the query returns one column, the column name becomes the header and the returned value becomes the KPI.
- Configure text alignment plus header and value color, size, weight, and style.

### Custom HTML

Use **Custom HTML** when you need a formatted custom display from query results.

- Build and run a query first so the template has data.
- Edit the HTML template in **Settings**.
- Open **Template help** for variables and examples.
- Available variables include `{{row}}`, `{{rows}}`, `{{columns}}`, `{{rowCount}}`, `{{columnCount}}`, and `{{theme.mode}}`.
- Available template helpers: `{{json value}}` (pretty-prints any value as JSON — useful for inspecting `row`/`rows`), `{{#if (eq a b)}}…{{/if}}` (equality check), and `{{formatNumber value maximumFractionDigits=2}}` (alias `{{number …}}`), in addition to the Handlebars built-ins `if`, `each`, `unless`, `with`, and `lookup`. Calling an unregistered helper raises a `Missing helper: "name"` render error.
- For column names with spaces, use bracket notation such as `{{row.[My Column]}}`.
- The widget assistant receives the current template **and** any live render error (such as `Missing helper`), so you can ask it to explain or fix a Custom HTML error without pasting the template.
- HTML is rendered in a sandboxed iframe. Inline CSS and JavaScript `<script>` tags are supported.
- The iframe provides theme-aware CSS variables: `--widget-text`, `--widget-muted-text`, `--widget-surface`, `--widget-border`, and `--widget-accent`.
- Assistant-generated Custom HTML uses inline `style` attributes for styling instead of custom CSS classes.
- Assistant-generated full Custom HTML templates, including JavaScript in `<script>` blocks, are normalized into readable multi-line code before review.
- Assistant-generated targeted edits to an existing Custom HTML template are reviewed as patch hunks, so unrelated markup, styling, scripts, and prior behavior can be preserved unless the user asks to change them.

### Free Text

Use **Free text** for notes, headings, explanations, or manually authored content — optionally with live values pulled from data.

- Type directly in the preview widget.
- Highlight text to use the toolbar for font family, size, color, alignment, and inline styles.
- To include a value from data, pick a **Model** and build a query, then use the toolbar's **Insert field** menu to drop a data-field chip into the text. On the board (and in exports/thumbnails) each chip is replaced by that column's value from the first row of the widget's query.
- A model is optional: without one, free text is plain static content; with one, its query runs and refreshes like any other data-backed widget.
- Unsafe HTML or JavaScript is stripped automatically.

### Form

Use **Form** to let people submit a record that is **inserted as a new row** into the underlying source table, with the look and feel of a form.

- A form is bound to a **Model** whose query resolves to a single writable base table (the same writeback rules as an editable Data grid). If the model can't accept new rows, the settings panel explains why.
- The source must have **writeback enabled** (an admin setting), and only editors can submit.
- Build the form field-by-field: **add a field and map it to a specific column** — nothing is included automatically. For each field set a label, an input type, and where its value comes from.
- **Value source** per field: the user enters it, a constant, a generated UUID, the current user, or the current date/time — so a non-auto primary key can be typed or auto-filled, while auto-increment keys are handled by the database.
- Map a field for every **required** column, or the insert is rejected.
- **Display**: show the form inline on the tile, or a button that opens the form in a modal.
- Every submission is inserted in a transaction and recorded in the write audit log.

### Summarise

Use **Summarise** to generate an AI-written summary.

Summary mode options:

- **Own query**: the summary widget runs its own saved query.
- **Reference widget**: the summary analyzes another saved widget from the same room.
- **Board panel**: the summary combines eligible data-backed widgets from a selected board panel.

How to configure:

1. Choose **Summarise** as the widget type.
2. Select **Summary mode**.
3. For **Reference widget**, choose **Source widget**.
4. For **Board panel**, choose **Source board** and **Source panel**.
5. Add optional **Extra context** for the assistant.
6. Save the widget before refreshing a summary. Refresh uses the last saved widget definition.
7. Click **Refresh preview** or refresh the summary from a board when available.

### Data Grid

Use **Data grid** to show tabular query results.

- Build and run a query to load the available columns.
- Configure **Table** settings such as page size and density.
- Configure **Behavior** settings such as allow sorting, sticky header, zebra rows, truncate cells, show row range, and default sort.
- Configure **Columns** to show or hide columns, change labels, and set widths.
- Configure **Writeback (editing)** to let people change the underlying data from the grid. See [Edit data in a data grid](#edit-data-in-a-data-grid).

## Edit Data In A Data Grid

A data grid can optionally write changes back to the source database. This is off by default and must be turned on per widget.

**Turn on editing**

1. The source must be enabled for writeback by a studio administrator (see the Sources documentation). Until then the grid stays read-only.
2. In the widget editor, open **Settings → Writeback (editing)** and switch on **Allow editing data in this grid**.
3. Optionally switch on **Allow inserting new rows** and **Allow deleting rows**.
4. In **Settings → Columns**, tick **Editable** on each column you want people to be able to change. Only eligible columns can be ticked; ineligible columns show the reason (for example a calculated column, a grouped/aggregated grid, or a column without a usable row key).

The widget automatically works out which table each editable column belongs to and how to identify a row — you do not choose a table. A banner in the Writeback section confirms whether the grid can be written back, or explains why not.

**Make and save changes (on a board or in the preview)**

- Click a cell in an editable column to change its value. Add a new row with **+ Add row**; remove a row with the delete control at the start of the row (click again to undo).
- Pending changes are highlighted and counted, but **nothing is written to the database until you click _Save changes_**. This _Save changes_ button is separate from the widget editor's save button (which only saves the widget's settings, never data).
- Click **Discard** to drop all pending changes.
- On save, all edits, new rows, and deletions are applied together. If someone else changed a row in the meantime, the save is rejected and you are asked to reload.

Writeback edits row data only; it does not change the table's structure (columns are added or removed in the model/source editors). Writeback supports MySQL and PostgreSQL sources.

### Pivot Table

Use **Pivot table** to aggregate and cross-tabulate data inside the widget.

- Select fields in the query, but leave query-level **Group By** to the widget.
- Configure **Rows** and **Columns** as pivot dimensions.
- Configure **Measures** with aggregate functions.
- Use expansion, click filtering, totals, value sort, and value filters to control the pivot behavior.
- On boards, pivot row names, column names, and value cells can be used for click filtering when enabled.

### Maps

Use **Maps** to display location-based data.

- Build a query that includes latitude/longitude fields or geometry fields required by the layer type.
- Configure **Map** settings: center mode, zoom, map type, and fixed center coordinates when needed.
- Add **Layers** for markers, heatmap, polygon, or route.
- Configure each layer with a name, layer type, visibility, tooltip fields, and the fields required by that layer.
- Marker layers can use label, color, size, icon, marker color, marker size, and cluster settings.
- Heatmap layers use point coordinates and optional weight data.

### Funnel Chart

Use **Funnel chart** for staged conversion or drop-off flows.

- Build and run a query with a stage field and a numeric value field.
- In **Layout**, choose **Stage field** and **Value field**.
- Choose **Label mode**. Conversion uses the first stage; drop-off uses the previous stage.
- Adjust label color, title, legend, chart labels, and colors as needed.

### Sankey Diagram

Use **Sankey diagram** for flows between source and target categories.

- Build and run a query with a source field, target field, and numeric value field.
- In **Layout**, choose **Source field**, **Target field**, and **Value field**.
- Configure node width, node gap, node alignment, link color mode, link opacity, link curveness, draggable behavior, node labels, and label color.

## Permissions And Availability

- View-only users can open widgets but cannot save, rename, duplicate, delete, or add them to boards.
- The widget list shows **View only** instead of edit actions when you do not have edit access.
- The editor hides or disables save and management actions when you cannot edit the widget.
- **Summarise** may be hidden or unavailable when the AI assistant is not enabled for the studio.
- Summary refresh uses the last saved widget definition, so save before generating or refreshing a summary.
- **Maps** needs map runtime configuration. If map services are unavailable, the map preview may show an error instead of a map.
- Some chart settings only become available after you run a query because they depend on returned columns or grouped data.
- **Data grid editing** requires edit access to the widget and a source that an administrator has enabled for writeback. Saving edits, inserting, and deleting follow the per-grid toggles.

## Troubleshooting

### I Cannot See Add Or Edit Actions

You likely have view-only access in the current room. Ask a studio or room administrator for an edit role.

### No Models Are Available

Confirm that the correct room is selected and that models exist for the room or studio. Data-backed widgets cannot be configured until a model is available.

### Fields Do Not Appear

Select a model and wait for fields to load. If fields still do not appear, the model may need to be reviewed or saved again.

### Run Does Not Return The Expected Data

Check **Selected fields**, **Where**, **Group By**, **Sort**, and **Limit**. Conditions with `LIKE` need a value pattern that matches the underlying data.

### A Chart Setting Says To Run The Query First

Some chart settings depend on the output columns. Click **Run**, then return to **Settings**.

### Summary Preview Is Out Of Date

Click **Save** first, then refresh the preview. Summary refresh uses the last saved widget definition.

### Add To Board Has No Panels

Choose a different board, or use **Create panel and add** when the dialog offers it.

### A Data Grid Will Not Let Me Edit

Check, in order: the source is enabled for writeback by an administrator; **Allow editing data in this grid** is on; the column has **Editable** ticked; and the Writeback banner does not report a blocking reason (for example an aggregated/grouped grid, a calculated column, or a column whose table has no primary key shown in the grid). Editing is also unavailable on shared/public board links.

### My Save Was Rejected As A Conflict

The row changed or was removed by someone else after it loaded. Reload the board so the grid refetches, then re-apply your change.

## Related Routes And Terms

- `/widgets`: widget list, search, add, rename, duplicate, delete.
- `/widgets/:widgetId`: widget editor, query builder, settings, preview, save, save as, duplicate, add to board.
- Related terms: widget, chart, query, model, selected fields, where, group by, sort, limit, board, panel, summary, pivot, map.
