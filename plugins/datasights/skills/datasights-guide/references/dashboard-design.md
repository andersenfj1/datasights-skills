# Dashboard design: grid, layout templates, model strategy, filters

Curated guidance (Layer C). Step-by-step UI workflows live in the mirrored module docs —
see [modules/boards.md](./modules/boards.md) for board/panel/filter/share/export workflows and
[modules/widgets.md](./modules/widgets.md) for widget creation and per-type settings. This file
covers what those docs do not: the physical grid, proven layouts, and the model-sharing decision
that controls cross-filtering.

## 1. The grid system

Boards use a drag-and-resize tile grid. Constants:

| Property | Value | Meaning |
|---|---|---|
| Columns | `12` | 12 columns; a tile's `x` is 0–11, `w` is 1–12 |
| Row height | `32` | one grid row = 32 px |
| Tile margin | `[2, 2]` | 2 px gap between tiles, horizontally and vertically |
| Rows per export page | `22` | one PDF/PPTX export page = 22 grid rows |
| Resize handles | `["w","sw","s","se","e"]` | resize from left, bottom-left, bottom, bottom-right, right — **no top handles** |

Physical math:

- Tile height in px = `h × 32 + (h − 1) × 2` = `34h − 2`. So `h=3` → 100 px, `h=6` → 202 px,
  `h=7` → 236 px, `h=8` → 270 px.
- One export page = `22 × 32 + 21 × 2` = **746 px** tall.
- While editing (not read-only, not export mode), the board draws a **dashed black horizontal
  guide** at every multiple of 22 rows. **Align section boundaries to these guides**: any tile
  that straddles a guide will be cut across two pages in PDF/PowerPoint export. If a band of
  widgets would start at `y=20` with `h=8` (rows 20–28), move it down to `y=22` instead.

Positions below are written `(x, y, w, h)` — column, row, width in columns, height in rows.

## 2. Proven layout templates

These coordinates are lifted from two production demo boards for a two-store retail POS dashboard. Reuse them as-is.

### Building blocks

- **KPI card**: `w=3, h=3` (quarter width, 100 px). Four across fill the top row exactly.
- **Hero chart + companion**: `w=8, h=7` main chart with a `w=4, h=7` secondary beside it.
- **Half-width pair**: two `w=6, h=7` tiles, or an asymmetric `4 + 8` split.
- **Full-width band**: one `w=12` tile, `h=6`–`8`.
- **Wide grid + narrow chart**: `w=7, h=8` data grid beside a `w=5, h=8` pivot/chart.

### Template A — 1-panel operational board (9 widgets, fits one export page: 17 rows ≤ 22)

| Slot | Type | Position `(x,y,w,h)` |
|---|---|---|
| KPI 1 (headline metric) | KPI | `0,0,3,3` |
| KPI 2 | KPI | `3,0,3,3` |
| KPI 3 | KPI | `6,0,3,3` |
| KPI 4 (alert/exception count) | KPI | `9,0,3,3` |
| Hero: metric over time (e.g. by hour) | Bar/Line | `0,3,8,7` |
| Breakdown by entity (e.g. by store) | Bar | `8,3,4,7` |
| Top-N ranking (horizontal bars) | Bar, *Swap axes* On | `0,10,6,7` |
| Composition (mix) | Pie/doughnut | `6,10,3,7` |
| Action list (e.g. low stock) | Data grid + conditional formatting | `9,10,3,7` |

### Template B — 1-panel reporting/trends board (10 widgets, 35 rows ≈ 2 export pages)

| Slot | Type | Position `(x,y,w,h)` |
|---|---|---|
| Trend lines (multi-series) | Line | `0,0,8,7` |
| Growth KPIs (2-row `(label,value)` model) | KPI | `8,0,4,7` |
| Index / benchmark bars | Bar | `0,7,4,6` |
| Grouped comparison (weekday × entity) | Bar, multi-series | `4,7,8,6` |
| Share of total | Pie/doughnut | `0,13,4,7` |
| Top-N by value (horizontal) | Bar, *Swap axes* On | `4,13,8,7` |
| Detail table (e.g. affinity) | Data grid | `0,20,7,8` |
| Cross-tab | Pivot | `7,20,5,8` |
| Grouped category bars | Bar with *Series field* | `0,28,6,7` |
| Movers table | Data grid | `6,28,6,7` |

Export note for Template B: the `y=20, h=8` band spans rows 20–28 and crosses the page guide at
row 22. If PDF export matters, shift that band and everything below it down by 2 rows (start at
`y=22`) so the page break falls in the gap.

Layout rules of thumb:

- KPI rows first, `h=3`; charts `h=6`–`7`; data grids and pivots `h=7`–`8` (they need row space).
- Horizontal bars (*Swap axes* On) whenever category labels are long product/customer names.
- Keep a board to 8–10 widgets per panel; split further content into additional panels
  (panel tabs — see [modules/boards.md](./modules/boards.md)).

## 3. Model strategy: one shared model vs one model per widget

This is the most consequential design decision on a board, because of how click cross-filtering
works.

### The propagation rule

When you click an element of an emitting widget on a board, the widget emits a runtime filter
`{ modelId, field, value, op }` where `modelId` is **that widget's `model_id`** and `field` is the
clicked category/group column alias. The board then applies that filter to **every other widget on
the displayed panel whose `model_id` is the same** (the source widget is excluded). Widgets with a
different `model_id` are untouched. Clicking a different element in the same widget **replaces**
that widget's previous click filter; each active click filter shows as a removable chip, and
**Clear click filters** removes them all. Click filters persist while you switch panels, so
same-model widgets on other panels of the board are also affected when displayed.

**Emitters**:

| Widget type | Emits on | Opt-in required? |
|---|---|---|
| Bar chart | bar click | no — always, when the widget has a model |
| Combo chart | series click | no |
| Pie chart | slice click | no |
| Funnel chart | stage click | no |
| Pivot table | row/column header or value cell click | yes — Settings → click filtering: **Row names** (default on), **Column names** (default on), **Value cells** (default off) |
| Maps | marker/shape interaction | yes — per layer, **Cross filter** settings: `enabled` (default off), filter field, value field, operator `=` or `IN` |

**Never emit** (receive only): Line chart, Radar chart, Sankey diagram, KPI, Data grid,
Summarise, Custom HTML, Free text, Form. A KPI or data grid on the same model *reacts* to clicks
elsewhere but clicking it does nothing.

### Decision rule

> Want click-to-filter interactivity between widgets? Give those widgets the **same model** and
> put them on the **same panel**, and do the aggregation in each widget's Build Query.
> Want precise, pre-aggregated SQL per tile (formatted KPIs, window functions, LIMITs)? Use
> **one model per widget** and accept that clicks will not propagate.

### Worked example 1 — one model per widget (reporting style, no cross-filter)

Those demo boards use 19 pre-aggregated models, one per widget: e.g. a KPI model
`SELECT CONCAT('R ', FORMAT(SUM(amount), 0)) AS \`Sales today\` ...`, a bar model
`SELECT hour_of_day, store_1_sales, store_2_sales ...`. Every widget's Build Query is
**pass-through** (auto-filled Selected fields, empty Where/Group By, Run). Clicking a bar in
"Sales by hour" does nothing to "Payment mix" — different `model_id`s. This is the right shape
when the SQL does the work (formatting, window functions, `LIMIT 10`) and the board is for
reading, not exploring.

### Worked example 2 — one shared model (operational drill style, cross-filter on)

One row-level model `sales_rows`:

```sql
SELECT o.created_at AS order_date, s.name AS store, p.category, p.name AS product,
       oi.quantity, oi.line_total AS amount
FROM order_items oi
JOIN orders o   ON o.id = oi.order_id
JOIN products p ON p.id = oi.product_id
JOIN stores s   ON s.id = o.store_id
```

Three widgets on one panel, **all bound to this model**:

1. Bar "Revenue by store" — Build Query: `store` + `SUM(amount)`, Group By `store`.
2. Pie "Revenue by category" — Build Query: `category` + `SUM(amount)`, Group By `category`
   (first column = slice label, second = value).
3. Data grid "Order detail" — Build Query: all fields, Limit 200.

Clicking the "Store 1" bar emits `{field: 'store', value: 'Store 1', op: '='}`; the pie and grid
re-run filtered to Store 1 because they share the `model_id`. Clicking a pie slice then filters
the bar and grid by `category`. The clicked field is the model's **column alias**, so every
sharing widget must come from a model that exposes that alias — automatic when the model is
shared. Aggregation cost note: each widget aggregates the row-level model per run, so keep the
model's row count reasonable (pre-filter the date window in the model SQL).

Hybrid is fine: share one model among the 3–4 widgets you want interactive, and use dedicated
models for formatted KPIs on the same panel.

## 4. Board filters — the only global filter mechanism

There are **no variables, no parameters, and no date-range picker** in DataSights. The only
board-level filter is the manual board filter (Board → Settings → Filters; workflow in
[modules/boards.md](./modules/boards.md)):

- Each filter is a **model + column** pair. Viewers pick one or more values from that column's
  distinct values; on **Apply filters** the selection is applied with **`IN` semantics**
  (`op: "IN"`, multi-value) — there are no range/date operators here.
- A board filter is keyed by `model_id` exactly like click filters: it only affects widgets bound
  to **that model**. On a one-model-per-widget board you must add one filter row per model (same
  column name in each model) to filter the whole board; on a shared-model board one filter row
  covers every sharing widget.
- **Orientation**: Horizontal (filter bar above the board) or Vertical (right-hand drawer with
  Show/Hide filters), plus a **Start collapsed** option.
- **Save as default** (editors only) persists the current selections as the board's default
  filter values for everyone; **Clear filters** empties selections.
- Exports (CSV/Excel/PDF/PPTX) honor the currently applied board + click filters.
- Filters require the board to contain widgets that use models ("No models on this board").

**Date windows must be encoded in the model SQL**, not in a picker. Use the data-driven "today"
idiom (anchor "today" to `MAX(date)` in the data so demo/stale data still shows a full day) —
see [sql-contract.md](./sql-contract.md) for the exact pattern, and put rolling windows
(`>= today − INTERVAL 30 DAY` etc.) in the model. If users need to switch windows, build separate
models/widgets ("Last 7 days" vs "Last 30 days") or add a precomputed window-label column
(e.g. `period` = `'7d' | '30d'`) and expose it as a board filter.

## 5. No board-to-board drill-through

There is no drill-through: clicking a widget never navigates to another board, and filters cannot
be passed from one board to another. Closest workarounds:

1. **Same-board drill**: put summary and detail widgets on the same panel with a shared model;
   click filters give summary→detail drill in place. This is the idiomatic answer.
2. **Detail panel on the same board**: put the detail grid on a second panel of the same board
   with the same model — click filters persist across panel switches, so click on panel 1, then
   open panel 2 (viewers must know to switch tabs).
3. **Pivot expansion**: a pivot with row fields `category → product` gives hierarchical
   drill-down inside one tile.
4. **Free text navigation aids**: a Free text widget can hold instructions ("open the *Monthly &
   Trends* board for history") — plain text only; there is no in-app inter-board link widget.
5. **Embedded boards**: an external host page embedding two boards can pass host-supplied runtime
   filters to each iframe — this is the only programmatic filter-injection path, and it lives
   outside DataSights. Ask your DataSights contact for the embed filter integration guide; it is
   not part of this skill.

## 6. Field-mapping cheat-sheet

Which query column lands where, per widget type. Full setting keys are in
[widget-settings-reference.md](./widget-settings-reference.md).

| Widget type | Dimension / category | Measure / value | Series (multi) | Notes |
|---|---|---|---|---|
| **KPI** | — | the cell value | — | 1 col = big value + column name as caption; 2 cols `(label, value)` = one captioned card per row. Format numbers in SQL (`R 41,250`) |
| **Bar** | *Category field* | *Value field* | *Series mode* = `multi_series` → *Value fields* (wide data), or *Series field* (long data) | *Swap axes* On = horizontal; *Stack bars* On = stacked; set *Series field* = category field to color each bar individually |
| **Line** | *X data field* | *Y data field* | *Chart style* = `multi_series` → *Y data fields* | *Series axis* Left/Right for dual-axis |
| **Combo** | *Category field* | per-series values | mixes bar + line series | Same icon as Bar/Line; *Chart mode* = `Combo` |
| **Pie** | **1st column (implicit)** | **2nd column (implicit)** | — | No field dropdowns — column order decides. *Pie chart type* pie/doughnut, *Slice order*, *Top N grouping* |
| **Funnel** | *Stage field* | *Value field* | — | *Label mode*: conversion = vs first stage, drop-off = vs previous stage |
| **Sankey** | *Source field* + *Target field* | *Value field* | — | One row per source→target flow |
| **Radar** | group field(s) = axes | numeric value column(s) | multi-chart per extra group field | Shape, area opacity, split number |
| **Data grid** | all returned columns | — | — | *Default sort*, *Columns* show/hide/label/width, *Conditional formatting* (**Add rule** / **Add color scale** — the color scale is the "heatmap"; there is no heatmap widget) |
| **Pivot** | *Add row field* / *Add column field* | *Add measure* (field + aggregate, e.g. SUM) | column fields form the cross-tab | Query-level **Group By stays empty** — pivot aggregates itself (see gotchas.md) |
| **Maps** | lat/lng (or geometry) fields per layer | marker size/color/heat weight fields | layers | Per-layer *Cross filter* (field + value field + `=`/`IN`) for click-filter emission |
| **Form** | — | — | — | No auto-mapping: add each field manually and map it to a column; per-field value source = user input, constant, UUID, current user, or now |
