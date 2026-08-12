# Gotchas: symptom → cause → fix

Curated (Layer C). Each entry is a real behavior verified against the codebase. Workflow context
lives in [modules/widgets.md](./modules/widgets.md) and [modules/boards.md](./modules/boards.md).

### KPI shows the wrong thing / caption missing
- **Symptom**: KPI renders a column name as the value, or one big number where you expected
  labeled cards.
- **Cause**: KPI semantics depend on column count. **1 column** → the column name becomes the
  caption and the single value is displayed big. **2 columns** `(label, value)` → one captioned
  card is rendered **per row** (multi-KPI).
- **Fix**: shape the model accordingly: `SELECT 'R 41,250' AS \`Sales today\`` for a single KPI;
  `SELECT label, value` returning N rows for N cards. Format numbers/currency in SQL — KPI
  settings style fonts and colors only.

### Pie chart ignores my field choices
- **Symptom**: no category/value dropdowns for Pie; slices come out wrong.
- **Cause**: Pie has **no field pickers** — it implicitly binds the **first returned column as the
  slice label and the second as the value**.
- **Fix**: order the columns in the model or in Build Query's Selected fields:
  label first, numeric value second. Extra columns are ignored.

### Pivot is double-aggregated or shows one row
- **Symptom**: pivot totals are wrong, or the query builder's Group By seems to disappear.
- **Cause**: pivot mode requires the query-level **Group By to be EMPTY**. The editor enforces
  this: it rewrites the query so every needed field is in Selected fields with **no aggregation**
  and Group By is always cleared — which is why the Group By section is hidden for Pivot. The
  pivot aggregates internally from its own **Rows / Columns / Measures** settings.
- **Fix**: feed the pivot raw (or pre-grouped) rows; configure aggregation only via
  **Add measure** (field + SUM/COUNT/AVG/...). Do not try to Group By in the query.

### Summarise refresh produced a stale/old summary
- **Symptom**: you changed the summary mode/source/extra context, hit Refresh, and got a summary
  of the old configuration.
- **Cause**: summary refresh always runs against the **last saved** widget definition, not the
  editor's unsaved state.
- **Fix**: click **Save** first, then **Refresh preview**.

### A chart setting I need is missing from the Settings panel
- **Symptom**: field dropdowns (Category field, Value fields, Stage field, ...) are empty or the
  section says to run the query first.
- **Cause**: many settings are populated from the **columns of the last Run** — before a
  successful Run there is nothing to bind.
- **Fix**: click **Run**, wait for the preview, then return to Settings. Re-run after changing
  Selected fields or aliases.

### I can't find separate "Bar chart" and "Line chart" widget types
- **Symptom**: only one chart icon exists where you expected bar and line.
- **Cause**: the editor collapses the legacy `Bar chart` and `Line chart` widget types into a
  single **"Bar / line chart"** option.
- **Fix**: pick **Bar / line chart**, then set **Chart mode** in Settings to `Line`, `Bar`, or
  `Combo`. Existing widgets saved as the legacy types keep working and open in the same editor.

### Numbers from a CSV-imported table won't sort/aggregate correctly
- **Symptom**: SUM returns 0/garbage, sorting is lexicographic ("10" before "9"), date filters
  fail on a table created via Import CSV.
- **Cause**: CSV import creates **every column as TEXT** (plus an auto-increment `id` primary
  key) — no type inference is applied at table-creation time.
- **Fix**: CAST in the model SQL (`CAST(amount AS DECIMAL(12,2))`, `STR_TO_DATE(...)`) and alias
  the result. Full patterns in [getting-data-in.md](./getting-data-in.md).

### Deleting a widget silently changed my boards
- **Symptom**: tiles vanished from one or more boards after a widget was deleted.
- **Cause**: widgets are standalone and shared — deleting one removes it from **every board
  panel** that uses it, on every board.
- **Fix**: before deleting, check which boards use it; use **Duplicate**/**Save As** to branch
  instead of editing-then-deleting shared widgets.

### My trigger can't select the widget I want to monitor
- **Symptom**: a chart or grid widget doesn't appear in the trigger's widget dropdown.
- **Cause**: **Triggers only accept KPI widgets**. Conditions are evaluated against KPI values.
- **Fix**: build a KPI widget exposing the number to watch (1-col, or 2-col `(label,value)` —
  *Match mode* controls how multiple KPI values must satisfy the condition), then select it in
  the trigger.

### I need live numbers inside text / custom markup
- **Symptom**: assuming Free text and Custom HTML are static-only.
- **Cause**: both can be **data-backed** by selecting a Model. Free text: use the toolbar's
  **Insert field** menu to drop data-field chips, replaced on the board by the first row's
  values. Custom HTML: a Handlebars template with `{{row}}`, `{{rows}}`, `{{columns}}`,
  `{{rowCount}}`, helpers like `{{formatNumber}}`, rendered in a sandboxed iframe.
- **Fix**: pick a model and build/Run a query in those widgets; they refresh on boards, in
  exports, and in thumbnails like any data-backed widget.

### Feature is missing from the type list / settings (availability gates)

| Feature | What to check | Where | Who can enable |
|---|---|---|---|
| **Summarise** widget type | tenant tier must be `boost` (feature flag `summaryWidget`), AND the studio assistant must be configured/enabled (model + API key + enabled) | Settings → **AI Assistant** tab | studio admin (tier is a tenant/provisioning attribute) |
| **Maps** widget rendering | studio map runtime: Google Maps API key, optional Map ID, default location | Settings → **Maps** tab (admins only see the tab) | studio admin |
| **Data grid writeback** | source-level **Allow data-grid writeback** toggle (MySQL/PostgreSQL only), then per-widget **Allow editing data in this grid**, then per-column **Editable** | Sources → edit source; Widget → Settings → Writeback (editing) | source toggle: studio admin; widget toggles: editor |
| **Form** widget submits | same source writeback toggle, AND the model must resolve to a single writable base table, AND every required column mapped; on shared/embedded boards, **Form submission** must be set to Allow (off by default) | Sources; Form settings banner; Share board dialog | admin (source), editor (widget), board sharer (share setting) |

- **Fix**: work through the row for the missing feature top-to-bottom; each layer gates the next.

### Clicking a line/radar/sankey (or grid/KPI) doesn't filter other widgets
- **Symptom**: cross-filtering works from bars and pie slices but not from lines, radar, sankey,
  KPIs, or data-grid rows.
- **Cause**: only **Bar, Combo, Pie, Funnel** emit click filters unconditionally; **Pivot**
  (Settings → click filtering: row names/column names default on, value cells default off) and
  **Maps** (per-layer Cross filter, default off) are opt-in. **Line, Radar, Sankey, KPI,
  Data grid, Summarise, Custom HTML, Free text, Form never emit** — they only receive.
- **Fix**: if the interaction matters, present the data as a bar/pie/funnel/pivot, or accept
  receive-only. Receivers must share the emitter's `model_id` (see
  [dashboard-design.md](./dashboard-design.md) §3).

### I clicked Save but my grid edits weren't written to the database
- **Symptom**: cell edits/new rows in an editable data grid disappeared, or the widget editor's
  **Save** seemed to have no effect on data.
- **Cause**: two different buttons. The grid's **Save changes** button (on the tile, next to the
  pending-changes counter) commits data edits to the source database in one transaction. The
  widget editor's **Save** saves the widget's *configuration* only — never data.
- **Fix**: after editing cells, click **Save changes** on the grid itself (or **Discard** to drop
  pending edits). If the save is rejected as a conflict, someone else changed the row — reload
  the board and re-apply.
