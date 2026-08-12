---
name: trigger
description: Set up a DataSights KPI threshold alert — bind a trigger to a KPI widget, pick the condition, value and match mode, choose how often it runs, and configure the notification email. Pick this for alerting ("email me when open tickets go above 50"), not for scheduled board emails.
argument-hint: "[what should alert you, e.g. 'email ops when daily revenue drops below 10000']"
disable-model-invocation: true
---

# KPI threshold alert

Task: **$ARGUMENTS**

If that is empty, ask for: the number to watch and which KPI widget already shows it (or the model
behind it), the room, the comparison and threshold value, how often it should be checked, and who
should be emailed.

**Lead with the constraint:** triggers monitor **KPI widgets only** — a chart, data grid, pivot,
or board cannot be watched, and non-KPI widgets never appear in the trigger's widget dropdown. If
the user has no KPI widget exposing the number, building one is step one, before `/triggers` is
opened at all. Managing triggers also needs an editor/admin-style role.

## Read before answering, in this order

1. `../datasights-guide/SKILL.md` — hard rules (guide-only, verify with the user), the
   *italic label* → **bold value** convention, and the roles digest. Invoking this command does
   not load it, so read it explicitly.
2. `../datasights-guide/references/modules/triggers.md` — the literal flows and labels:
   *Add A Trigger*, *Configure A Trigger*, *Choose A Condition*, *Choose When The Trigger Runs*,
   *Configure Trigger Email*, plus *Permissions And Availability* and *Troubleshooting*.
3. `../datasights-guide/references/modules/widgets.md` — *Widget Type Reference* → *KPI* for the
   column-count semantics, and *Build A Query*, whenever the KPI widget still has to be built.

Read only if the task touches them: `../datasights-guide/references/gotchas.md` (*KPI shows the
wrong thing / caption missing*; *My trigger can't select the widget I want to monitor*),
`../datasights-guide/references/widget-settings-reference.md` (the `kpi` widgets section, for exact
setting keys), `../datasights-guide/references/sql-contract.md` (if the model behind the KPI must
be written or reshaped), `../datasights-guide/references/modules/schedules.md` (if *Run trigger*
should reuse a saved schedule).

## How to work the task

1. Confirm the user's role and the room. The KPI widget and the trigger must live in the same room.
2. Confirm the KPI widget exists and returns the number in the shape you need. KPI meaning depends
   on column count and row count — check that against widgets.md before trusting it. If no KPI
   widget exists, build it first: model → query → *Run* → user pastes the result → you confirm the
   number against their sample data → *Save*.
3. State the alert in one plain sentence, then map it to *Condition*, a numeric *Value*, and
   *Match mode* using only the option lists in triggers.md. Never invent an operator or mode name.
4. *Match mode* only decides anything when the KPI returns more than one value — say which mode you
   chose and why, or say it is moot for a single-value KPI.
5. Choose *Run trigger*: a preset interval or a saved schedule. Whatever you pick is the alert's
   latency floor — tell the user that number explicitly.
6. Configure email in the documented order (the email fields stay disabled until *Send email* is
   on), then set recipients.
7. *Save*, then *Active*, then verify: have the user confirm the trigger row and, where feasible,
   force the condition true and paste back whether the email arrived. If nothing fires, walk the
   *Troubleshooting* checks in triggers.md instead of guessing.
