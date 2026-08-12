---
name: build
description: Take a user from raw data (database, CSV, spreadsheet, PDF, pasted rows) to finished DataSights boards — runs the intake interview, then produces a personalized end-to-end build guide with model SQL, widget recipes, grid coordinates, and a verification checklist.
argument-hint: "[what you want built + where the data lives]"
---

# Build a DataSights dashboard

The request is `$ARGUMENTS`. If it is empty, ask for these four things before anything else:
what the dashboard must answer, where the data lives right now, 10–30 sample rows, and the
user's role plus the studio/room it will live in.

## 1. Read before you write

Load these in order; do not answer from memory.

1. `../datasights-guide/SKILL.md` — **always first**. Hard rules (guide-only), the app mental
   model, the intake interview, deliverable rules, roles, gates, and the *italic label* →
   **bold value** convention this whole deliverable is written in.
2. `../datasights-guide/references/recipe-template.md` — the output format: required section
   order, recipe heading grammar, checklist rules. Your guide must match it.
3. `../datasights-guide/references/getting-data-in.md` — only if the data is not already in a
   MySQL/PostgreSQL database: size caps, CSV import path, in-chat conversion, the all-TEXT caveat.
4. `../datasights-guide/references/dashboard-design.md` — grid constants, proven layout
   templates, and the shared-model vs model-per-widget decision that controls cross-filtering.
   Read it **before** proposing models, not after.
5. `../datasights-guide/references/sql-contract.md` — every model query; finish with its
   pre-flight checklist.
6. `../datasights-guide/references/modules/widgets.md` and `../datasights-guide/references/modules/boards.md` — the literal
   UI steps and labels for the editor flow, panels, filters, sharing, and export.
7. `../datasights-guide/references/widget-settings-reference.md` — verify every settings label
   and enum value you write. Never invent one.
8. `../datasights-guide/references/gotchas.md` — when a pasted result does not match expectation.

## 2. Sequence

1. Run the intake interview from the main SKILL.md, in compact batches. Do not skip the sample
   data step — you cannot ship verification without it.
2. **Propose the model list before writing any SQL.** For each: name, purpose, grain, and exact
   output aliases with types. Say which models are deliberately shared for click cross-filtering
   and which widgets sit on which panel. Get explicit confirmation, then write SQL.
3. Write the guide in the recipe-template format. Every widget recipe carries `pos x,y,w,h` on
   the 12-column grid; keep export-bound content inside the page-row budget.
4. Ship the verification checklist with real expected values computed in chat from the user's
   sample data, then work the instruct → user runs → user pastes → you confirm loop.
