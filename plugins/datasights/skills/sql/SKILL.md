---
name: sql
description: Write or repair the SQL for a DataSights model — use when you need a new model query, or an existing one is rejected ("Only SELECT queries are allowed", duplicate column, wrong types) or returns the wrong rows.
argument-hint: "[what the model should return, or the failing SQL + error]"
---

# Model SQL

The user's request: $ARGUMENTS

If that is empty, ask for: (1) what the model must return — one row per what, which metrics and
dimensions; (2) the source dialect, **MySQL** or **PostgreSQL**; (3) the table and column shape
(names + types) or 10–30 sample rows; (4) for a repair, the current SQL and the exact error text
or wrong output.

Check the project folder for a `*.datasights-erd.md` file first — it answers (2) and (3) outright,
so ask only for what it does not cover.

## Read first, in this order

1. `../guide/SKILL.md` — hard rules (guide-only: you never run the SQL or touch their
   database), the *italic label* → **bold value** convention, and role checks. Invoking this
   command does not load it automatically.
2. `../guide/references/sql-contract.md` — the authority for everything below:
   validator algorithm and banned-word scan (§1), the `AS base` wrapper and aliasing (§2), what
   the widget layer can do so your SQL doesn't over-do it (§3), proven idioms (§5), MySQL vs
   PostgreSQL table (§6), the pre-flight checklist (§7), error → cause → fix (§8).
3. `../guide/references/modules/models.md` — the editor flow you hand back: *Source*,
   *Query*, *Run*, *Data*, *Save*, *Commit message*, *Version history* / *Restore*.
4. `../guide/references/getting-data-in.md` §2.1 — how to find and read a downloaded
   `*.datasights-erd.md` schema file; and §3.4, only if the table came from a CSV import: every
   column is TEXT, so `CAST` before any math, sort, or date filter.
5. `../guide/references/gotchas.md` — only if the query validates and runs but the
   widget shows the wrong thing.

## How to work

1. **Confirm the dialect and the shape before writing a line.** Never guess column names or
   types. In this order: (a) look in the project folder for a `*.datasights-erd.md` file (or grep
   for `format: datasights.source-erd`) — take the dialect from its `source.dialect` header and
   the table names, column names, types and keys from its `## Table:` sections, and do not re-ask
   for anything it already states; with several such files, each is one source, so ask which
   applies only when it is genuinely ambiguous. (b) Otherwise ask them to paste the schema.
   (c) Otherwise have them open the model and read tables and columns from the table list on the
   left, then paste it. If they have none of it, tell them about *Sources* → the ERD icon on the
   source row → **Download** — that produces the file in (a). Reading the file is exactly as
   allowed as reading any project file; you still never touch their database yourself.
2. **State the output contract first** — the exact aliases and their types. Aliases become the
   widget field names, so every output column gets a unique explicit alias, always, including
   inside `UNION ALL` branches and on both sides of a join. This is not optional formatting.
3. **Run the §7 pre-flight checklist over every query before you show it**, all twelve items,
   scanning string literals and quoted identifiers too. A query that fails the checklist never
   reaches the user.
4. **Deliver one fenced block per model**, copy-paste ready, no placeholders. Then the steps:
   paste into *Query*, click *Run*, check *Data*, click *Save*, enter a *Commit message*.
5. **Verify with computed numbers.** From their sample rows, state what *Run* should return, and
   have them paste the result back. Mismatch → fix the SQL, not the expectation.
6. **Repairs:** match the error string in §8 first, fix the cause, and tell them *Version
   history* → *Restore* is the way back if the previous version was better.
