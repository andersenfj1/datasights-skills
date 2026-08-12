---
name: data
description: Get data into DataSights — connect a MySQL/PostgreSQL source, import a CSV, or convert a PDF/XLSX/pasted table to clean CSV first. Use this before any model or dashboard work when the data is not in the app yet.
argument-hint: "[where the data lives now — e.g. 'postgres on RDS', 'sales.xlsx', 'pasted invoice rows']"
disable-model-invocation: true
---

# Get data into DataSights

The user's description of their data: $ARGUMENTS

If that is empty, ask for: what the data is (live DB / CSV / XLSX / PDF / pasted rows), roughly
how many rows, whether a Source already exists in the target studio, and their role — CSV import
and source edits need edit access, writeback needs studio admin.

## Read before answering

1. `../datasights-guide/SKILL.md` — hard rules (guide-only, *italic label* → **bold value**,
   role checks). Invoking this command does not load it; load it yourself.
2. `../datasights-guide/references/getting-data-in.md` — the primary reference here: decision
   tree (§1), source fields and *Test Connection* (§2), CSV limits/identifier rules/all-TEXT
   caveat (§3), the in-chat conversion spec (§4), error table (§6).
3. `../datasights-guide/references/modules/sources.md` — literal UI click-paths for *Add*,
   *Import CSV*, *Create New Table*, *Append Existing*, *Allow data-grid writeback*.
4. `../datasights-guide/references/sql-contract.md` — only when you hand over the model SQL that
   casts an imported table (§6 for MySQL vs PostgreSQL syntax).
5. `../datasights-guide/references/gotchas.md` — when the symptom is already happening (numbers
   from a CSV-imported table sorting or aggregating wrong).

## How to run this command

1. **Place the data on the decision tree** in getting-data-in.md §1 before proposing anything:
   already in MySQL/PostgreSQL → add a Source, no import; CSV within the row cap → import;
   anything else → convert first; over the row cap or a recurring feed → load into the database
   directly and point a Source at it.
2. **Convert in chat yourself.** Producing clean CSV from XLSX/PDF/pasted rows is explicitly
   allowed and expected — do it to the §4 spec and hand the file back. What stays off-limits is
   touching the app or the database: the user clicks *Import CSV*, not you.
3. **Name columns deliberately** in the CSV you produce — header normalization silently rewrites
   anything else, and those names flow through model aliases into widget field bindings.
4. **Never ship a CSV without its model SQL.** Imported tables are all-TEXT plus an auto `id`, so
   deliver the casting SELECT in the same message, in the source's dialect.
5. **Give the UI steps from sources.md**, in order, with real values for every field — including
   *Test Connection* → **Connection successful.** before *Save*.
6. **On a failure, get the exact error string** and route it through the §6 table rather than
   guessing; note that a failed MySQL create-mode import can leave the table behind.
7. **Verify before moving on:** have the user *Run* the model and paste rows, and check them
   against values you computed from their sample data.
