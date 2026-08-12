# Build-guide recipe template

This file is a **format specification**. Follow it whenever you produce a personalized, end-to-end
dashboard build guide for a user. The format is proven: it is the exact shape of a two-store retail
POS demo build guide (a 2-board, 19-widget guide that a user followed start-to-finish without app
knowledge). That original lives outside this repository; this spec is standalone and contains
everything you need, including a complete worked example at the end.

Related skill files: `sql-contract.md` (model SQL rules), `getting-data-in.md` (sources + CSV
import), `widget-settings-reference.md` (the authoritative list of widget setting names),
`modules/widgets.md` / `modules/sources.md` / `modules/models.md` (literal UI flows).

---

## 1. When to produce this deliverable

Produce a build guide when the user asks to be walked from **their data** to **finished
dashboards** — e.g. "help me build a dashboard for this CSV", "set up boards for my POS database",
"give me step-by-step instructions my analyst can follow". The guide is a single markdown file the
user (or a colleague with zero DataSights knowledge) executes by hand in the UI.

Do **not** produce one for single-widget questions, settings lookups, or debugging — answer those
inline. A guide is warranted when the work spans **source → models → 2+ widgets → board layout**.

**Naming:** call the file `BUILD_GUIDE.md` when it sits in a dedicated project folder; otherwise
`<project-slug>-build-guide.md` (e.g. `acme-sales-build-guide.md`). The H1 title is always
`# <Project name> — build guide`. If the guide needs more than ~10 models, split the SQL into a
companion `models.md` (one `###` heading per model, a `**suggested widget:**` bullet, then a
` ```sql ` block) and reference it; otherwise inline the SQL in the guide's Models section.

---

## 2. Required section order

Every guide has these sections, in this order. Omit a section only when it truly does not apply
(e.g. no CSV step for an existing database), and never reorder.

| # | Section | Purpose |
|---|---|---|
| 0 | **Header + scope statement** | One paragraph: what the guide builds, and an explicit boundary sentence in bold — e.g. "**No DataSights app code is changed** — only data is created (a source, models, widgets, and board rows)." State what already exists vs. what the reader will create. |
| 1 | **Data inventory** | A `| table | rows | notes |` markdown table listing every source table the guide touches, with real row counts and a short note on the load-bearing columns. This is the reader's mental model of the data and the basis for every expected number in the verification checklist. |
| 2 | **Source setup** | The literal Sources-module flow with the user's real values filled in: **Sources** → **Add** → pick **MySQL** or **PostgreSQL** from the source type rail → **Name**, **Host**, **Port**, **Database**, **Username**, **Password** → **Test Connection** (expect **Connection successful.**) → **Save**. Use angle-bracket placeholders like `<your password>` only for secrets you genuinely do not know. If data arrives by CSV, include the **Import CSV** flow here (see `getting-data-in.md`): **Sources** → **Import CSV** → **Source** → **Mode: Create New Table** → **Choose File** → review **CSV Preview** → **New Table Name** → **Import CSV**. |
| 3 | **Models** | One SQL block per model, in the companion-file format described in section 1. Every query obeys `sql-contract.md`: a single `SELECT` (no CTEs — the app wraps it as `SELECT * FROM (<sql>) AS base`), every output column aliased, and the alias is the field name widgets bind to. Columns the CSV importer left as `TEXT` must be `CAST` (or `STR_TO_DATE`) **inside the model**, never in the widget. |
| 4 | **The editor flow** | The common 6-step flow, written **once**, verbatim from section 3 below. Every recipe then references it instead of repeating clicks. Follow it with a short "behaviors to remember" bullet list covering only the widget types the guide actually uses. |
| 5 | **Per-board recipes** | One `##` section per board, titled `Board <letter> — "<Board name>"`. Open with "Create the board, add a panel." then the recipes (format in section 4 below). |
| 6 | **Field-mapping cheat-sheet** | A table with columns `| Widget type | Dimension / category | Measure / value | Series (multi) | Notes |`, one row per widget type used, naming the *italicized setting labels* each field maps to. Lets the reader recover from any mis-click without re-reading recipes. |
| 7 | **Verification checklist** | Falsifiable checks with concrete expected values (rules in section 5 below). |
| 8 | **Re-running / reset** | How to refresh the data and re-run the guide idempotently: which steps are safe to repeat, which need cleanup first (e.g. re-import CSV with **Append Existing** vs. dropping and recreating the table). |
| 9 | *(optional)* **Why this beats <current tool>** | 3–4 bullets of business value, only when the user framed the project competitively. |

---

## 3. The common editor flow (write once, reference everywhere)

Copy this block into section 4 of every guide, adjusting only the parenthetical notes to the widget
types actually used. These are the literal UI labels from the widget editor (`/widgets`):

> Open **Widgets**, click **Add**, enter a **Name**, click **Create**, then open the widget.
> For each widget in the recipes below:
>
> 1. **Model** — choose the model named in the recipe from the **Model** selector.
> 2. **Widget type** — choose the type from the row of type icons (hover an icon to see its name).
> 3. **Build Query** — our models are already aggregated, so this is **pass-through**: leave
>    **Selected fields** as auto-filled, leave **Where** and **Group By** empty, then click
>    **Run**. This loads the columns and shows a preview. (Set **Limit** / **Sort** only where a
>    recipe says so. **Group By** is hidden for Pivot table — that is expected.)
> 4. **Settings** (right-hand panel) — set the fields and options exactly as the recipe lists.
> 5. **Save**.
> 6. **Add to board** — choose the **Board**, then the **Panel**, then **Add to board** (or
>    **Add and go to board**; if the board has no panel yet, **Create panel and add**). Drag and
>    resize the tile to the recipe's grid position.

When a recipe *does* need query-builder work beyond pass-through, use the literal Build Query
labels: **Add field** (aggregations `(none)`, `DISTINCT`, `SUM`, `COUNT`, `AVG`, `MIN`, `MAX`),
**Add condition** under **Where** (operators `=`, `!=`, `>`, `>=`, `<`, `<=`, `LIKE`), **Add
group** under **Group By**, **Add sort** under **Sort** (`Ascending` / `Descending`), and
**Limit**. Prefer doing the work in the model SQL instead; keep Build Query pass-through wherever
possible so the recipe stays short.

---

## 4. Recipe format

### Heading grammar

```
### <ID> · <Widget type> · "<Title>" — pos x,y,w,h
```

- `<ID>` — board letter + ordinal: `A1`…`A9`, `B1`…`B10`. IDs are stable references; the
  cheat-sheet and checklist may cite them.
- `<Widget type>` — the literal type name as the editor shows it (e.g. `KPI`, `Bar / line chart`,
  `Pie chart`, `Data grid`, `Pivot table`). Never a paraphrase.
- `"<Title>"` — the exact **Name** the reader types when creating the widget.
- `pos x,y,w,h` — position on the board's **12-column grid**: `x` = left column (0-based),
  `y` = top row, `w` = width in columns, `h` = height in rows. **Every recipe gives coordinates**;
  tiles laid out by prose ("put it on the right") do not survive contact with a reader.
- Showpiece widgets — the 1–2 per board that demonstrate a differentiating feature — get a `⭐`
  after the closing quote of the title.

### Body bullets, in order

1. `- **Model:** \`<model name>\` (cols \`a, b, c\`)` — name the model exactly as saved; list the
   output columns whenever there is more than one, so the reader can sanity-check the preview.
   May be merged with the type on one line: `` **Model:** `X` · **Type:** KPI ``.
2. `- **Type:** <Widget type>` — repeat the type (skip if merged above).
3. `- Build Query: pass-through.` **or** the explicit deltas only, e.g.
   `- Build Query: *Limit* \`200\` · *Sort* \`revenue\` Descending.` Never restate the whole
   6-step flow.
4. `- Settings:` — every setting as `*italic UI label*` → `**bold value**`. Group by settings
   panel when it disambiguates: `Settings (Behavior):`, `Settings (Conditional formatting):`.
   Field values that are column names are backticked and bold: `**\`revenue\`**`. Every settings
   label must exist in `widget-settings-reference.md` — check it; do not invent labels.
5. Optional expectation line: `Shows e.g. **R 41,250**.` — what the reader should see if
   everything worked. Use real values computed from the data whenever you can.

### ⭐ showpiece convention

A ⭐ recipe is allowed (encouraged) to be longer: use **numbered sub-steps** for multi-click
settings sequences such as conditional formatting, e.g.:

```
- Settings (**Conditional formatting**):
  1. Click **Add color scale** → *Source field* **`days_of_cover`** · *Apply to* **cell** ·
     *Min color* red `#b91c1c` · *Max color* green `#15803d`.
  2. Click **Add rule** → *Source field* **`stock_status`** · *Apply to* **row** ·
     *Condition* **equals** · *Value* `OUT OF STOCK` · *Background color* red.
```

End a ⭐ recipe with the payoff — what the reader should notice ("tops out on *House Blend 1kg +
Pods* (3.35)").

---

## 5. Verification checklist rules

The checklist is what makes the guide trustworthy. Every item must be **falsifiable**: a reader
can mark it pass or fail without judgment calls.

- **Concrete numbers, computed from the user's own data.** Before writing the checklist, run (or
  hand-compute for small data) the model queries and state the actual expected values: "KPI shows
  **R 7,400.00**", "the bar chart has **3** bars, ordered South > North > East", "the grid shows
  **12** rows". Never write "shows a total" or "looks correct".
- **Always include the network-tab check**, verbatim:
  > Every widget previews after **Run** (no "model/settings missing" / SQL error); the browser
  > network tab shows `POST /api/models/:id/run-config` returning `{columns, rows}`.
- Include at least one **shape/ordering** check per chart (bar order, line direction, slice
  count, default-sort of a grid) and one **formatting** check when the SQL formats values
  (currency prefixes, percent signs).
- If the data is live and "today" moves, phrase checks that survive drift ("peaks between 07:00
  and 09:00") and pin exact numbers only to frozen sample data.

---

## 6. Style rules

- **Literal UI labels only.** Every bold or italic term must appear in the app exactly as
  written. Source of truth: `modules/*.md` for flows, `widget-settings-reference.md` for widget
  settings. If you cannot verify a label, describe the location ("the sort dropdown in the
  Behavior panel") rather than inventing a name.
- **No invented terminology.** The app has Studios, Rooms, Sources, Models, Widgets, Panels,
  Boards. Do not introduce "datasets", "reports", "tiles" (except "tile" for the physical
  drag-target on a board), or other synonyms.
- **One model per widget**, unless the user explicitly needs cross-filtering between widgets
  sharing a model. Each model's SQL does all aggregation, formatting, and casting; widgets stay
  pass-through.
- **Grid coordinates always given**, on the 12-column grid, for every widget.
- Model SQL: single `SELECT`, **no CTEs** (the validator requires the statement to start with
  `SELECT`, so `WITH` is rejected outright — see `sql-contract.md`), every column aliased,
  `ORDER BY` / `LIMIT` in the SQL when the widget depends on order.
- CSV-imported columns: **every column lands as `TEXT`** (plus an auto `id` primary key — there
  is no type inference; see `getting-data-in.md`). Say so in the data inventory, and `CAST`
  every numeric and date column the model uses.

---

## 7. Complete worked mini-example

Everything inside the fence below is a complete, correct guide in this format, for a fictional
12-row CSV. Use it as the shape to imitate; scale up, never sideways.

The fictional input `acme_sales.csv` (12 rows + header):
`region,product,sold_on,qty,amount` — regions North/South/East, products Widget/Gadget/Sprocket,
ISO dates 2026-07-01…07, integer qty, and amounts that arrived formatted like `R 1,250.00`.
Per the cleaning rules in `getting-data-in.md`, the amounts were stripped to plain `1250.00`
**before** import — the model-SQL validator bans the `REPLACE()` function, so character stripping
cannot happen later in SQL. After import **every column lands as TEXT** (CSV-created tables get no
type inference), so the models below CAST each numeric and date column they use.

````markdown
# Acme sales — build guide

One board from `acme_sales.csv` on **DataSights**. **No DataSights app code is changed** — only
data is created (one CSV-backed table, 3 models, 3 widgets, 1 board).

## 0. What already exists

You have `acme_sales.csv` (12 sales rows, July 2026). Everything else is created below.

| table | rows | notes |
|---|---|---|
| `acme_sales` | 12 | created by CSV import; **all columns TEXT** (`amount` cleaned to plain `1250.00` before import — models must CAST) |

## 1. Add the source and import the CSV

Before importing, clean the CSV per `getting-data-in.md`: strip the `R ` prefix and thousands
separators from `amount` (`R 1,250.00` → `1250.00`). This must happen now — model SQL cannot
strip characters later because the validator bans the `REPLACE()` function.

1. **Sources** → **Add** → choose **MySQL** from the source type rail.
2. **Name** `Acme Sales` · **Host** `db.internal.acme.example` · **Port** `3306` ·
   **Database** `acme_bi` · **Username** `acme_ro` · **Password** `<your password>`.
3. **Test Connection** → expect **Connection successful.** → **Save**.
4. **Sources** → **Import CSV** → **Source** `Acme Sales` → **Mode** **Create New Table** →
   **Choose File** `acme_sales.csv` → review **CSV Preview** (5 columns, 12 rows) →
   **New Table Name** `acme_sales` → **Import CSV** → wait for the progress message.

## 2. Create the 3 models

For each: **Models** → **Add** → in **Add Model** enter **Name** → **Save** → open it → choose
**Source** `Acme Sales` → paste the SQL into **Query** → **Run** to preview → **Save**.

### Total revenue
- **suggested widget:** KPI

```sql
SELECT CONCAT('R ', FORMAT(SUM(CAST(amount AS DECIMAL(12,2))), 2)) AS `Total revenue`
FROM acme_sales
```

### Revenue by region
- **suggested widget:** Bar / line chart

```sql
SELECT region,
       ROUND(SUM(CAST(amount AS DECIMAL(12,2))), 0) AS revenue
FROM acme_sales
GROUP BY region
ORDER BY revenue DESC
```

### Order log
- **suggested widget:** Data grid

```sql
SELECT CAST(sold_on AS DATE) AS sold_on, region, product,
       CAST(qty AS UNSIGNED) AS qty,
       CAST(amount AS DECIMAL(12,2)) AS amount_zar
FROM acme_sales
ORDER BY sold_on
```

## 3. The editor flow (same 6 steps for every widget)

Open **Widgets**, click **Add**, enter a **Name**, click **Create**, then open the widget.
For each widget:

1. **Model** — choose the model named in the recipe from the **Model** selector.
2. **Widget type** — choose the type from the row of type icons (hover an icon to see its name).
3. **Build Query** — our models are already aggregated, so this is **pass-through**: leave
   **Selected fields** as auto-filled, leave **Where** and **Group By** empty, then click
   **Run**. This loads the columns and shows a preview. (Set **Limit** / **Sort** only where a
   recipe says so.)
4. **Settings** (right-hand panel) — set the fields and options exactly as the recipe lists.
5. **Save**.
6. **Add to board** — choose the **Board**, then the **Panel**, then **Add to board** (or
   **Create panel and add** if the board has no panel yet); drag and resize the tile to the
   recipe's grid position.

**Behavior to remember** — **KPI**: a 1-column result shows that value big, with the column name
as the caption; the SQL already formats the string (`R 7,400.00`), so you only style fonts.

## 4. Board A — "Acme — Sales overview"

Create the board, add a panel. Grid is 12 cols; (x,y,w,h) suggested.

### A1 · KPI · "Total revenue" — pos 0,0,4,4
- **Model:** `Total revenue` · **Type:** KPI
- Build Query: pass-through (1 row, 1 col `Total revenue`).
- Settings: nothing required. Optional: *Text alignment* → **Left** · *Value → Weight* → **Bold**.
- Shows e.g. **R 7,400.00**.

### A2 · Bar / line chart · "Revenue by region" — pos 4,0,8,8
- **Model:** `Revenue by region` (cols `region, revenue`)
- **Type:** Bar / line chart
- Build Query: pass-through.
- Settings: *Chart mode* → **Bar** · *Series mode* → **single_series** · *Category field* →
  **`region`** · *Value field* → **`revenue`** · *Swap axes* → **Off**.
- Shows 3 bars, tallest first: South, North, East.

### A3 · Data grid · "Order log" ⭐ — pos 0,8,12,8
- **Model:** `Order log` (cols `sold_on, region, product, qty, amount_zar`)
- **Type:** Data grid
- Build Query: *Limit* `100` (12 rows expected).
- Settings (Behavior): *Default sort column* → **`amount_zar`** · *Default sort direction* →
  **Descending**.
- Settings (**Conditional formatting**):
  1. Click **Add color scale** → *Source field* **`amount_zar`** · *Apply to* **cell** ·
     *Min color* light grey `#e5e7eb` · *Max color* green `#15803d` (big orders glow green).
- The 2026-07-03 Sprocket order (**1250.00**) tops the grid in the deepest green.

## 5. Field-mapping cheat-sheet

| Widget type | Dimension / category | Measure / value | Series (multi) | Notes |
|---|---|---|---|---|
| **KPI** | — | the cell value | — | 1 col = value + caption |
| **Bar / line chart** | *Category field* | *Value field* | *Series mode* → **multi_series** | *Chart mode* Bar/Line/Combo; *Swap axes* On = horizontal |
| **Data grid** | (all columns shown) | — | — | *Default sort* + **Add color scale** / **Add rule** |

## 6. Verification checklist

- Every widget previews after **Run** (no "model/settings missing" / SQL error); the browser
  network tab shows `POST /api/models/:id/run-config` returning `{columns, rows}`.
- A1 shows exactly **R 7,400.00**.
- A2 has **3** bars ordered **South (3200) > North (2200) > East (2000)**.
- A3 shows **12** rows; the top row is **2026-07-03 · South · Sprocket · 5 · 1250.00** and its
  `amount_zar` cell is the darkest green.

## 7. Re-running / reset

- **New data:** **Sources** → **Import CSV** → **Mode** **Append Existing** → **Existing
  Table** `acme_sales`. Widgets refresh on next run; recheck the checklist numbers, which will
  have changed.
- **Full reset:** drop `acme_sales` in the source database and repeat step 1's import. Models
  and widgets survive — they reference the table by name.
````

---
*Format proven by a real two-store retail POS build guide (two boards, 19 widgets); that document is not part of this package, so this spec restates every convention it established.*
