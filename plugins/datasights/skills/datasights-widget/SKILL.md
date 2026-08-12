---
name: datasights-widget
description: Build or configure ONE DataSights widget — choose the right type for the question, wire its query fields, and set its settings. Pick this for single-widget work ("make a combo chart of revenue vs target", "why is my pie wrong"), not for designing a whole board.
argument-hint: "[what the widget should show, e.g. revenue by month vs target as a combo chart]"
disable-model-invocation: true
---

# Build one widget

The widget to build or fix: **$ARGUMENTS**

If that is empty, ask for: the question this one widget must answer, the model behind it (name plus
its output aliases, or the columns you expect), the room, and where it ends up — on a board panel,
in a PDF export, or watched by a trigger.

## Read before answering, in this order

1. `../datasights-guide/SKILL.md` — hard rules (guide-only, the *italic label* → **bold value**
   convention, the gated-widget table, roles). Invoking this command does not load it.
2. `../datasights-guide/references/modules/widgets.md` — *Create A Widget*, *Build A Query*, the
   *Widget Type Reference* entry for your type, and *Permissions And Availability*.
3. `../datasights-guide/references/widget-settings-reference.md` — the *Widget kinds* table, then
   that kind's *Chart field roles*, *Settings groups* and *Decision rules*. Keys and enum values not
   listed there do not exist; never invent one.
4. `../datasights-guide/references/gotchas.md` — the traps for this type (KPI column count, Pie's
   implicit first/second columns, pivot's empty Group By, which types emit click cross-filters).
5. Only if needed: `../datasights-guide/references/dashboard-design.md` §6 field-mapping
   cheat-sheet (which column lands in which role), `../datasights-guide/references/sql-contract.md`
   (if the model's SQL or aliases must change first).

## Steps

1. Restate the question in one line and name the grain of the rows feeding the widget.
2. Choose the type from that question, not from the user's phrasing — say why, and name the runner-up
   you rejected. If it will sit on a board, check the emitter list before promising click filtering.
3. **Gate check before proposing a gated type.** Summarise, Maps, editable Data grid and Form each
   depend on setup outside the editor — confirm the gate or write the enablement step naming the
   studio admin, per the gate tables in `SKILL.md` and `gotchas.md`.
4. Confirm the model's aliases cover every field role the type needs. If they do not, fix the model
   first — settings bind to aliases.
5. Spec *Build Query*: *Selected fields* (+ aggregation), *Where*, *Group By*, *Sort*, *Limit*, then
   *Run*. Many settings only populate from the last successful Run, so order it Run → Settings.
6. Spec the settings as the editor groups them, in *italic label* → **bold value** form.
7. Give expected values from the user's sample data, have them *Run* and paste the result, confirm
   the match, then *Save* — and *Add to board* only if a board and panel were named.
