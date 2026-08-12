# DataSights SQL Contract

Every model in DataSights is **one saved SELECT statement** (`models.model_query`) that runs
against the studio's *source* database (MySQL or PostgreSQL — never the app's own DB). The app
never executes your SQL as-is at widget time: it wraps it as a derived table and layers a
machine-built outer query on top. Writing SQL that survives this pipeline means satisfying two
independent contracts:

1. **The validator** — a text-level gate every model query must pass before it is ever sent to
   the source.
2. **The wrapper** — the `FROM (<your sql>) AS base` composition that turns your column aliases
   into widget fields.

This document is the authoritative reference for both, plus proven idioms, dialect notes, a
pre-flight checklist, and a troubleshooting table.

---

## 1. The validator, step by step

The exact algorithm:

1. **Strip comments.** Block comments (`/* ... */`, non-greedy, spanning lines) and line
   comments (`--` to end of line) are removed *before any other check*. **Comments in model SQL
   are therefore legal**, and banned keywords inside comments are harmless — they are gone
   before the scan. (Calculated-field expressions are the opposite; see section 5.)
2. **Strip outer parentheses.** Leading runs of `(` and trailing runs of `)` are removed
   (regex `/^\(+|\)+$/g`), then trimmed. This is validation-only — the executed SQL keeps its
   parentheses, so `(SELECT ... ) UNION (SELECT ...)` validates and runs fine.
3. **Strip trailing semicolons** (`/;+\s*$/`). One trailing `;` is tolerated (and also stripped
   by every execution path before running), but omit it anyway.
4. **Reject any remaining semicolon.** If a `;` survives anywhere in the statement — including
   *inside a string literal* (the validator is not string-aware) — the query is rejected.
   Need a literal semicolon in output? Build it: `CONCAT(a, CHAR(59), b)` (MySQL) /
   `a || CHR(59) || b` (PostgreSQL).
5. **Must start with `SELECT`** (case-insensitive). This is what bans CTEs: `WITH x AS (...)
   SELECT ...` starts with `WITH`, so it fails. Same for `EXPLAIN`, `SHOW`, `DESCRIBE`, `SET`.
   The replacement for a CTE is a derived table in `FROM` (section 6, idiom 1).
6. **Banned-keyword scan**, word-boundary, case-insensitive, over the whole statement:

   ```
   /\b(INSERT|UPDATE|DELETE|DROP|ALTER|CREATE|TRUNCATE|REPLACE|GRANT|REVOKE)\b/i
   ```

Failing any step returns HTTP 400 **`Only SELECT queries are allowed`** — from
`POST /api/models/:id/run` (model editor preview), `POST /api/models/:id/run-config` (widget
execution), and column inspection. One error string, three doors; all three must pass.

### 1.1 What the word-boundary scan really catches

`\b` treats letters, digits, and `_` as word characters. Consequences:

| Text in your SQL | Verdict | Why |
| --- | --- | --- |
| `created_at`, `updated_at`, `deleted_at` | PASS | banned word followed by a word character — no boundary |
| `insert_ts`, `delete_count`, `last_update` | PASS | `_` is a word character, so no boundary forms |
| `REPLACE(name, 'a', 'b')` | **FAIL** | `(` is a non-word char — `REPLACE` matches as a whole word |
| `INSERT(str, 2, 3, 'xy')` (MySQL string function) | **FAIL** | same |
| `TRUNCATE(price, 1)` (MySQL numeric function) | **FAIL** | same |
| `` `update` `` (backtick-quoted column) | **FAIL** | the scan ignores quoting; backtick is a non-word char |
| `'please update your app'` (string literal) | **FAIL** | the scan ignores string literals too |
| `'STATUS: CREATED'`, `'granted'` | PASS | trailing word character breaks the boundary |
| `DATE_SUB(...)`, `GROUP_CONCAT(...)`, `SUBSTRING(...)` | PASS | contain no banned word as a whole word |

### 1.2 Safe alternatives for each banned function you might reach for

| You wanted | Use instead (MySQL) | Use instead (PostgreSQL) |
| --- | --- | --- |
| `REPLACE(str, from, to)` | Do it in the source (view/ETL), or compose with `SUBSTRING_INDEX` + `CONCAT`, or `TRIM(BOTH 'x' FROM str)` for edge stripping | `TRANSLATE(str, from, to)` for single-character swaps (passes the scan); otherwise fix in the source |
| `INSERT(str, pos, len, new)` | `CONCAT(LEFT(str, pos - 1), new, SUBSTRING(str, pos + len))` | `LEFT(str, pos - 1) \|\| new \|\| SUBSTRING(str FROM pos + len)` |
| `TRUNCATE(x, d)` | `FLOOR(x * POW(10, d)) / POW(10, d)` — or `ROUND(x, d)` if rounding is acceptable | `TRUNC(x, d)` — `TRUNC` is a different word and passes |
| The literal string `'CREATE'` / `'UPDATE'` / … in a comparison | Build it so the scan can't see it: `status = CONCAT('CREAT', 'E')` | same trick, or `status = 'CREAT' \|\| 'E'` |
| A source column literally named `update`, `delete`, etc. | No in-model workaround — even quoted it fails. Rename or alias it in a database view and query the view | same |
| Actual DML/DDL | Never. Models are read-only by design | — |

---

## 2. The `AS base` wrapper and what it does to your SQL

At widget time the app builds (MySQL/PostgreSQL shape; identifiers quoted per dialect):

```sql
SELECT <widget select expressions>
FROM (<your model_query, trailing ';' stripped>) AS `base`
[WHERE ...]      -- widget + board runtime filters, parameterized
[GROUP BY ...]
[ORDER BY ...]
[LIMIT n]
```

With calculated fields, an extra inner layer is added first:

```sql
FROM (
  SELECT `calc_base`.*, (<calc expression>) AS `calc_name`
  FROM (<your model_query>) AS `calc_base`
) AS `base`
```

(`calc_base.*` is only emitted when a base column is referenced somewhere in the widget config;
a widget that references only calculated fields gets just the calculated expressions.)

Consequences you must design for:

- **Column aliases become field names.** Whatever each output column is called is exactly what
  the widget binds to (`base`.`alias`). Alias every expression deliberately; aliases may contain
  spaces if quoted (`` AS `Sales today` `` → field "Sales today", which doubles as the KPI label).
- **Every output column name must be unique** (compared case-insensitively). The app probes with
  `SELECT * FROM (<sql>) AS subq LIMIT 0` — both to list columns/types and before the editor
  preview (`POST /api/models/:id/run`). MySQL itself rejects duplicates in a derived table
  (`ER_DUP_FIELDNAME`); PostgreSQL allows them, so the app detects them from the probe's field
  list. Either way you get HTTP 400:
  `Duplicate column name detected: <name>. Please alias duplicate columns in the model query.`
  The classic trigger is a join where both tables have `name` or `id` — alias both sides, and
  never `SELECT *` across a join.
- **Column types come from driver metadata of the `LIMIT 0` probe**, not from parsing your SQL
  (MySQL type ids resolve to names like `NEWDECIMAL`, `LONGLONG`, `VAR_STRING`, `DATE`,
  `DATETIME`; PostgreSQL OIDs resolve to `INTEGER`, `NUMERIC`, `TIMESTAMP`, `VARCHAR`; unmapped
  types surface as `TYPE_<id>` / `UNKNOWN`). Practical effect: `CONCAT(...)` and MySQL
  `FORMAT(...)` produce *string* fields — perfect for KPI tiles, wrong for chart axes. Keep
  chart values numeric (`ROUND`, `SUM`) and pre-format only KPI models.
- **`ORDER BY` inside the model is legal** and drives the editor preview and datagrid default
  order — but the widget layer appends its own `ORDER BY` outside the wrapper whenever a sort is
  configured, which wins. MySQL may also ignore a derived table's `ORDER BY` without `LIMIT`.
  Treat model-level ordering as a nicety, never a guarantee.
- **`LIMIT` inside the model is legal and useful**: it is applied *before* the widget layer, so
  a `... ORDER BY revenue DESC LIMIT 15` top-N stays a top-N no matter how the widget re-sorts.
- **Alias your own derived tables.** MySQL requires every subquery in `FROM` to have an alias
  (`) pr`, `) v`, etc.). The app supplies `base`/`subq`/`calc_base` for its layers only.
- **Preview economics:** `POST /api/models/:id/run` executes your raw SQL *in full* on the
  source and only then truncates to 10 rows — no LIMIT is injected. Add your own `LIMIT` while
  iterating on expensive queries. Widget runs (`POST /api/models/:id/run-config`) apply a limit
  policy instead: `executionContext: "preview"` → default 500 rows, hard cap 5000;
  `executionContext: "board"` → default 500, **no cap** (an explicit widget limit is honored
  as-is).

---

## 3. What the widget layer can do (so your SQL doesn't over-do it)

The outer query is assembled from a constrained config — nothing else exists at that layer:

- **Aggregations** (`select[].agg`): `SUM`, `COUNT`, `AVG`, `MIN`, `MAX`, `DISTINCT` only
  (anything else → `Invalid aggregation`). `DISTINCT` turns the whole select into
  `SELECT DISTINCT`; it is not per-column. When any aggregate is present, every non-aggregated
  selected field is automatically added to `GROUP BY`; explicitly grouped fields are kept in the
  output even if not selected.
- **Where operators**: `=`, `!=`, `>`, `>=`, `<`, `<=`, `LIKE`, `IN` (else `Invalid operator`),
  joined with `AND`/`OR` (else `Invalid where logic`). All values are parameterized. Board
  runtime filters use the same operators; a runtime `LIKE` value is auto-wrapped as `%value%`,
  and a runtime `IN` with no values is dropped (a config-level empty `IN` becomes `1=0`).
- **Filters are `WHERE`, never `HAVING`.** They apply to base columns *before* aggregation.
  If a user must filter on an aggregate ("categories with revenue > 10 000"), compute that
  aggregate inside the model so it is a plain column at the widget layer.
- **Sort**: `ASC`/`DESC` only (else `Invalid sort direction`); sorting an aggregated field
  targets its output alias.
- **No joins, no window functions, no expressions, no date bucketing** at the widget layer.
  All of that belongs in the model SQL — or in a calculated field (next section).

**Division of labor:** model SQL = joins, date bucketing, pivots (`CASE WHEN`), pre-aggregation,
top-N, formatting for KPIs. Widget layer = re-aggregation, filtering, sorting, limiting over the
clean columns you exposed.

---

## 4. Calculated fields (widget-scoped expressions)

A calculated field is a **plain SQL expression** stored per widget
(`widget_settings.queryConfig.calculatedFields`) and injected verbatim as
`(<expression>) AS <quoted name>` in the `calc_base` layer. Rules:

- Must be non-empty → else `Calculated field SQL is required`.
- **No `{{` or `}}`** → else `Calculated field SQL must be a plain SQL expression`.
- **No `;`, `--`, `/*`, or `*/`** — unlike model SQL, comments are hard-banned here →
  `Calculated field SQL cannot contain comments or semicolons`.
- Names must be unique per widget → else `Calculated field names must be unique`.
- Quote base-column references yourself, in the source's dialect: `` `Col` `` (MySQL),
  `"Col"` (PostgreSQL).
- The expression may be an aggregate (`AVG(...)`) or row-level (`CASE WHEN`, arithmetic,
  `NULLIF`, `COALESCE`, `date_trunc`, ...). Once defined, the name behaves like any base field
  in select/where/groupBy/sort.
- Assistant-path caps: at most 30 fields per widget, 12 000 characters per expression.

---

## 5. Proven idioms (all verbatim from a working production demo, all validator-clean)

Every query below starts with `SELECT`, contains no semicolon, and no banned word — each one has
been run against a live MySQL source. Notice the alias discipline: every column has an explicit,
unique, widget-friendly name.

### Idiom 1 — Derived tables instead of CTEs

You cannot write `WITH`. Name each sub-result as an aliased subquery in `FROM` and join them.
Market-basket affinity with three derived tables plus a scalar total:

```sql
SELECT CONCAT(p1.name,' + ',p2.name) AS pairing,
       pr.pair_cnt AS baskets_together,
       ROUND(100*pr.pair_cnt/tot.n,2) AS support_pct,
       ROUND(100*pr.pair_cnt/f1.cnt,1) AS confidence_pct,
       ROUND((pr.pair_cnt*tot.n)/(f1.cnt*f2.cnt),2) AS lift
FROM (
  SELECT a.product_id p1, b.product_id p2, COUNT(*) pair_cnt
  FROM sale_items a JOIN sale_items b ON a.sale_id=b.sale_id AND a.product_id<b.product_id
  GROUP BY a.product_id,b.product_id
) pr
JOIN (SELECT product_id, COUNT(DISTINCT sale_id) cnt FROM sale_items GROUP BY product_id) f1 ON f1.product_id=pr.p1
JOIN (SELECT product_id, COUNT(DISTINCT sale_id) cnt FROM sale_items GROUP BY product_id) f2 ON f2.product_id=pr.p2
JOIN products p1 ON p1.product_id=pr.p1
JOIN products p2 ON p2.product_id=pr.p2
CROSS JOIN (SELECT COUNT(DISTINCT sale_id) n FROM sale_items) tot
WHERE pr.pair_cnt >= 30
ORDER BY lift DESC
LIMIT 25
```

The `LIMIT 25` keeps this a top-25 even after widget re-sorting (limit applies inside `base`).

### Idiom 2 — Data-driven "today"

For demos or feeds that lag, anchor "today" to the newest data date instead of the wall clock —
`(SELECT MAX(DATE(sale_ts)) FROM sales)` — so boards never show an empty day. For live
operational data, use `CURDATE()` (MySQL) / `CURRENT_DATE` (PostgreSQL) instead.

```sql
SELECT st.name AS store, ROUND(SUM(s.subtotal),0) AS sales, COUNT(*) AS transactions
FROM sales s JOIN stores st ON st.store_id=s.store_id
WHERE s.is_refunded=0 AND DATE(s.sale_ts)=(SELECT MAX(DATE(sale_ts)) FROM sales)
GROUP BY st.name
ORDER BY sales DESC
```

Rolling windows use the same anchor: `sale_ts >= DATE_SUB((SELECT MAX(sale_ts) FROM sales),
INTERVAL 90 DAY)`.

### Idiom 3 — Pre-formatted KPI strings via CONCAT/FORMAT

A KPI tile shows one column of one row. Format in SQL so the tile is exactly right (the field
becomes a string — do this only for KPI models, never for chart values):

```sql
SELECT CONCAT('R ', FORMAT(COALESCE(SUM(subtotal),0),0)) AS `Sales today`
FROM sales WHERE is_refunded=0 AND DATE(sale_ts)=(SELECT MAX(DATE(sale_ts)) FROM sales)
```

The boolean-sum variant packs two counts into one readable value (MySQL booleans are 0/1):

```sql
SELECT CONCAT(SUM(stock_on_hand=0),' OOS  ·  ',
       SUM(stock_on_hand>0 AND stock_on_hand<=reorder_level),' reorder') AS `Stock alerts`
FROM store_inventory
```

### Idiom 4 — Label/value UNION ALL for multi-metric KPIs

One model, several KPI rows, each a `label` + pre-formatted `value`. Derived tables replace what
would otherwise be a CTE, `NULLIF` guards the division, `CROSS JOIN` pairs the periods:

```sql
SELECT 'Sales vs prior 30 days' AS label,
       CONCAT(ROUND(100*(cur.rev-prev.rev)/NULLIF(prev.rev,0),1),'%') AS value
FROM (SELECT SUM(subtotal) rev FROM sales WHERE is_refunded=0 AND sale_ts > DATE_SUB((SELECT MAX(sale_ts) FROM sales), INTERVAL 30 DAY)) cur
CROSS JOIN (SELECT SUM(subtotal) rev FROM sales WHERE is_refunded=0 AND sale_ts > DATE_SUB((SELECT MAX(sale_ts) FROM sales), INTERVAL 60 DAY) AND sale_ts <= DATE_SUB((SELECT MAX(sale_ts) FROM sales), INTERVAL 30 DAY)) prev
UNION ALL
SELECT 'Sales vs same 30 days last year',
       CONCAT(ROUND(100*(cur.rev-ya.rev)/NULLIF(ya.rev,0),1),'%')
FROM (SELECT SUM(subtotal) rev FROM sales WHERE is_refunded=0 AND sale_ts > DATE_SUB((SELECT MAX(sale_ts) FROM sales), INTERVAL 30 DAY)) cur
CROSS JOIN (SELECT SUM(subtotal) rev FROM sales WHERE is_refunded=0 AND sale_ts > DATE_SUB((SELECT MAX(sale_ts) FROM sales), INTERVAL 395 DAY) AND sale_ts <= DATE_SUB((SELECT MAX(sale_ts) FROM sales), INTERVAL 365 DAY)) ya
```

Each `UNION ALL` branch must alias its columns identically to the first branch's `label`/`value`.

### Idiom 5 — CASE WHEN pivoting for multi-series charts

Charts want one row per x-value with one column per series. Pivot in SQL — the widget layer
cannot:

```sql
SELECT DATE_FORMAT(sale_ts,'%Y-%m-01') AS month,
       ROUND(SUM(CASE WHEN store_id=1 THEN subtotal ELSE 0 END),0) AS store_1_sales,
       ROUND(SUM(CASE WHEN store_id=2 THEN subtotal ELSE 0 END),0) AS store_2_sales,
       ROUND(SUM(subtotal),0) AS total_sales
FROM sales WHERE is_refunded=0
GROUP BY DATE_FORMAT(sale_ts,'%Y-%m-01')
ORDER BY month
```

### Idiom 6 — Guarded division and status bucketing for grids

`NULLIF`/`COALESCE` around every division; `CASE` for human-readable status columns; a `LEFT
JOIN` onto an aggregated derived table keeps zero-velocity rows:

```sql
SELECT st.name AS store, p.name AS product, c.name AS category,
       inv.stock_on_hand, inv.reorder_level,
       ROUND(COALESCE(v.avg_daily,0),1) AS avg_daily_units,
       CASE WHEN COALESCE(v.avg_daily,0)=0 THEN NULL ELSE ROUND(inv.stock_on_hand/v.avg_daily,1) END AS days_of_cover,
       CASE WHEN inv.stock_on_hand=0 THEN 'OUT OF STOCK'
            WHEN inv.stock_on_hand<=inv.reorder_level THEN 'REORDER' ELSE 'OK' END AS stock_status
FROM store_inventory inv
JOIN products p ON p.product_id=inv.product_id
JOIN product_categories c ON c.category_id=p.category_id
JOIN stores st ON st.store_id=inv.store_id
LEFT JOIN (
  SELECT si.product_id, s.store_id, SUM(si.qty)/30.0 AS avg_daily
  FROM sale_items si JOIN sales s ON s.sale_id=si.sale_id
  WHERE s.sale_ts > DATE_SUB((SELECT MAX(sale_ts) FROM sales), INTERVAL 30 DAY)
  GROUP BY si.product_id, s.store_id
) v ON v.product_id=inv.product_id AND v.store_id=inv.store_id
WHERE inv.stock_on_hand<=inv.reorder_level OR inv.stock_on_hand=0
ORDER BY (inv.stock_on_hand=0) DESC, days_of_cover ASC
```

Note the three `name` columns are aliased apart (`store`, `product`, `category`) — mandatory
under the duplicate-column probe.

---

## 6. MySQL vs PostgreSQL dialect notes

The validator is dialect-agnostic; the *source* decides which dialect your model must speak.
Check the source type before writing.

| Concern | MySQL | PostgreSQL |
| --- | --- | --- |
| Number formatting | `FORMAT(n, 2)` → `'1,234.57'` (string) | `TO_CHAR(n, 'FM999,999,990.00')` |
| Month bucketing | `DATE_FORMAT(ts,'%Y-%m-01')` | `date_trunc('month', ts)::date` or `TO_CHAR(ts,'YYYY-MM-01')` |
| Date arithmetic | `DATE_SUB(ts, INTERVAL 30 DAY)` | `ts - INTERVAL '30 days'` |
| Date from timestamp | `DATE(ts)` | `ts::date` or `CAST(ts AS date)` |
| Weekday name / number | `DAYNAME(ts)` / `DAYOFWEEK(ts)` | `TO_CHAR(ts,'FMDay')` / `EXTRACT(ISODOW FROM ts)` |
| Hour of day | `HOUR(ts)` | `EXTRACT(HOUR FROM ts)` |
| String concat | `CONCAT(a, b)` (`\|\|` is logical OR unless `PIPES_AS_CONCAT`) | `a \|\| b` or `CONCAT(a, b)` |
| Cast | `CAST(x AS CHAR)`, `CAST(x AS SIGNED)`, `CAST(x AS DECIMAL(10,2))` | `CAST(x AS text)`, `x::int`, `x::numeric(10,2)` |
| Boolean sum | `SUM(cond)` (booleans are 0/1) | `COUNT(*) FILTER (WHERE cond)` or `SUM(CASE WHEN cond THEN 1 ELSE 0 END)` |
| Integer division | `7/2` → `3.5000` | `7/2` → `3` — multiply by `1.0` or cast first |
| Identifier quoting | `` `Col Name` `` | `"Col Name"` (unquoted identifiers fold to lowercase) |
| Truncate decimals | `FLOOR(x * POW(10,d)) / POW(10,d)` (the `TRUNCATE()` function is validator-banned) | `TRUNC(x, d)` (passes — different word) |
| Row limit | `LIMIT n` | `LIMIT n` |

The widget layer handles its own dialect differences (backticks vs double quotes, `?` vs `$1`
placeholders) — you only own the model text.

---

## 7. Pre-flight checklist — run this over every query before showing it

1. **Starts with `SELECT`** — no `WITH`, `EXPLAIN`, `SHOW`, `SET`. CTE logic rewritten as
   aliased derived tables.
2. **No semicolon anywhere** — not even inside string literals; drop the trailing one.
3. **Banned-word scan** — mentally run
   `\b(INSERT|UPDATE|DELETE|DROP|ALTER|CREATE|TRUNCATE|REPLACE|GRANT|REVOKE)\b` over the whole
   text, *including string literals, quoted identifiers, and aliases*. `created_at` is fine;
   `REPLACE(`, `TRUNCATE(`, `INSERT(`, a column named `update`, or the literal `'CREATE'` are
   not. Comments are exempt (stripped first) — but avoid `--` inside string literals.
4. **Every output column explicitly aliased, all names unique** (case-insensitive), no
   `SELECT *` across joins. Aliases are the widget field names — make them readable
   (`store_1_sales`, `` `Sales today` ``).
5. **Every derived table has an alias** (MySQL hard requirement).
6. **Right dialect for the source** — check MySQL vs PostgreSQL before using `DATE_FORMAT`,
   `FORMAT`, `\|\|`, etc.
7. **Types match the widget** — numeric columns for chart values (`ROUND`, not `FORMAT`);
   pre-formatted strings only for KPI models.
8. **Divisions guarded** with `NULLIF(denominator, 0)`; `COALESCE` defaults where NULLs would
   blank a tile.
9. **Aggregate-level filters computed in the model** — the widget layer has `WHERE` only, no
   `HAVING`.
10. **Top-N ranked and `LIMIT`ed inside the model** so widget re-sorting can't break the cut.
11. **`ORDER BY` treated as preview-only**; correctness never depends on model-level order.
12. **Cost sanity** — `/run` preview executes the full query before truncating to 10 rows; add
    a `LIMIT` while exploring big tables.

---

## 8. Troubleshooting: error message → cause → fix

| Error message | Cause | Fix |
| --- | --- | --- |
| `Only SELECT queries are allowed` | Fails validation: starts with `WITH`/other keyword, contains an internal `;`, or trips the banned-word regex (often `REPLACE(`/`INSERT(`/`TRUNCATE(` or a banned word in a string literal) | Rewrite CTE as derived table; remove semicolons; substitute per the table in section 1.2 |
| `Query is required` | `POST /api/models/:id/run` with empty/whitespace query | Provide SQL |
| `Model query is required` | Widget ran a model whose `model_query` is empty | Save SQL on the model first |
| `Duplicate column name detected: <name>. Please alias duplicate columns in the model query.` (plural: `Duplicate column names detected: ...`; nameless: `Duplicate column name detected. Please alias duplicate columns in the model query.`) | Two output columns share a name (case-insensitive) — usually joined tables both exposing `name`/`id`, or `SELECT *` over a join | Alias every colliding column distinctly |
| `Invalid aggregation` | Widget `select[].agg` not in `SUM/COUNT/AVG/MIN/MAX/DISTINCT` | Use an allowed aggregate, or compute the metric in model SQL |
| `Invalid operator` | Widget/runtime where `op` not in `= != > >= < <= LIKE IN` | Use an allowed operator; encode complex predicates in the model |
| `Invalid where logic` / `Invalid sort direction` | Logic not `AND`/`OR`; direction not `ASC`/`DESC` | Correct the config value |
| `Invalid select field` / `Invalid where field` / `Invalid group by field` / `Invalid sort field` / `Invalid alias` / `Invalid calculated field name` | Empty or NUL-containing identifier reached the builder (field name doesn't exist as a non-empty string) | Reference a real model column alias or calculated-field name |
| `Calculated field SQL is required` | Empty calculated-field expression | Provide an expression |
| `Calculated field SQL must be a plain SQL expression` | Expression contains `{{` or `}}` | Remove template markers; plain SQL only |
| `Calculated field SQL cannot contain comments or semicolons` | Expression contains `;`, `--`, `/*`, or `*/` | Strip them — comments are banned in calc fields even though model SQL allows them |
| `Calculated field names must be unique` | Two calc fields normalize to the same name | Rename one |
| `Base query is required` | Empty base query reached the widget query layer | Save a non-empty model query |
| `A valid source is required to run queries` / `Model has no valid source` | Model has no `source_id` | Attach a source to the model |
| `Source not found or inactive` | Source deleted, deactivated, or belongs to another studio | Pick an active source in the model's studio |
| `Missing required connection details` | Source lacks host, database, or username | Complete the source connection settings |
| `Unsupported source type for queries` | Source type isn't `mysql`/`postgres` (`mssql` accepted only on the run-config path) | Use a MySQL or PostgreSQL source |
| `Not authorized to run this model` (403) | User lacks edit (editor preview) or view (widget run) access to the studio | Grant studio access |
| `Model not found` / `Model has no query to inspect` (404/400) | Bad model id, soft-deleted model, or empty query on column inspect | Verify the model exists and has SQL saved |
| Native SQL error surfaced verbatim (500), e.g. `Unknown column 'x' in 'field list'` | The statement passed validation but the source DB rejected it | Fix the SQL against the real source schema; remember it executes inside `FROM (...) AS subq/base` |

---

**Rule of thumb:** write the model as if its output were a clean, well-named spreadsheet — one
`SELECT`, every column aliased uniquely, numerics numeric, dates bucketed, ratios guarded —
and let the widget layer do only what it can: aggregate, filter, sort, limit.
