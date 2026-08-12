---
name: fix
description: Diagnose something broken in DataSights — an empty or wrong widget, a chart binding the wrong columns, a read-only data grid, a model that fails to Run, a board filter or cross-filter that does not propagate.
argument-hint: "[symptom + widget type + any error text]"
disable-model-invocation: true
---

# Fix a broken DataSights thing

The user reported: **$ARGUMENTS**

If that is empty or vague, ask for these before diagnosing — batch them in one turn:

1. **The exact symptom.** What you expected vs what you see (empty tile, wrong numbers,
   greyed-out control, nothing happens on click).
2. **Where.** Widget type and *Chart mode* if it is a Bar / line chart; model name; board and
   panel; source type (MySQL or PostgreSQL) and whether the table came from *Import CSV*.
3. **Error text verbatim.** Copy the message, do not paraphrase it — the fix tables key off
   the exact string.
4. **The evidence.** Have them click *Run* in the model or widget editor and **paste the
   output** (columns + first rows, or the failure). Never guess at what the query returns.

## Read before answering

1. `../datasights-guide/SKILL.md` — **always first**. Its hard rules bind you: guide-only (never
   call the app API, never connect to their database, never run their SQL), the *italic label* →
   **bold value** convention, the gate table, and the roles digest for who may apply the fix.
2. `../datasights-guide/references/gotchas.md` — **the primary lookup.** Scan its symptom → cause
   → fix entries and its availability-gates table before anything else; most reports land here.
3. The module doc for the surface, then its `## Troubleshooting` section:
   `../datasights-guide/references/modules/widgets.md` (widget/grid/summary/settings), `../datasights-guide/references/modules/boards.md`
   (panels, board filters, sharing, export), `../datasights-guide/references/modules/models.md` (Run, version
   history), `../datasights-guide/references/modules/sources.md` (connection, CSV import, writeback toggle).
4. `../datasights-guide/references/sql-contract.md` — only for SQL/model failures. §8 is the
   error-message → cause → fix table; §1 explains `Only SELECT queries are allowed`; §2 covers
   duplicate columns and wrapper-driven column types.
5. `../datasights-guide/references/dashboard-design.md` §3–§4 — filter problems: the propagation
   rule, the emitter/non-emitter lists, and what board filters can and cannot do.
6. `../datasights-guide/references/widget-settings-reference.md` — when a setting is missing or a
   value is being rejected; confirm the key exists for that widget kind and the enum is legal.
7. `../datasights-guide/references/getting-data-in.md` — when numbers or dates from a
   CSV-imported table sort or aggregate wrong.

## How to answer

1. Name the **cause** first, in one sentence, and say which reference it came from.
2. Give the fix as numbered UI steps against real labels, or one corrected fenced SQL block —
   never a placeholder for the user to fill in.
3. State what the user should see when it works, then have them re-*Run* and paste the result.
   The bug is closed only when pasted evidence confirms it.
4. If nothing in the references matches, say so plainly and ask for the next piece of evidence
   rather than inventing a mechanism.
