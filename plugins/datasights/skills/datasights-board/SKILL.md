---
name: board
description: Assemble or improve a DataSights board — panel layout on the grid, board filters, click cross-filtering, sharing and embedding, and export-safe design. Pick this when the widgets already exist (or are planned) and the question is how they fit together on a board.
argument-hint: "[board name or goal, e.g. 'lay out the ops board for PDF export']"
---

# Board assembly

Task: **$ARGUMENTS**

If that is empty, ask for: which board (or "new board") and which room; the widgets that exist
and the model behind each; who consumes it (on screen / share link / iframe embed / PDF or
PowerPoint export / scheduled email); and which tiles should react to each other when clicked.

## Read before answering, in this order

1. `../datasights-guide/SKILL.md` — hard rules (guide-only, verify with the user), the
   *italic label* → **bold value** convention, and the roles digest. Invoking this command does
   not load it, so read it explicitly.
2. `../datasights-guide/references/dashboard-design.md` — §1 grid constants and the export page
   math, §2 proven layout templates, §3 the model-sharing decision that controls cross-filtering
   (plus the emitter / never-emit lists), §4 board filters, §5 why there is no drill-through.
3. `../datasights-guide/references/modules/boards.md` — the literal UI flows and labels:
   *Add Or Rename Panels*, *Add Widgets To A Board*, *Configure Board Filters*, *Share A Board*,
   *Export Or Download A Board*.

Read only if the task touches them: `../datasights-guide/references/gotchas.md` (click filters
not propagating; deleting a widget affecting other boards),
`../datasights-guide/references/widget-settings-reference.md` (exact keys when a tile must be
reconfigured to emit — pivot click filtering, maps cross filter),
`../datasights-guide/references/recipe-template.md` (heading grammar if you are delivering a
full build guide), `../datasights-guide/references/modules/schedules.md` (emailed exports).

## How to work the task

1. Confirm the room, the user's edit access, and the current inventory: panels, widgets, and the
   **model behind each widget**. The model is the cross-filter key — ask if it is unknown.
2. Decide interactivity before layout. Which tiles must respond to a click together? Apply the
   decision rule in dashboard-design.md §3 and regroup models or panels first if it fails.
3. Lay out numerically — every tile as `(x, y, w, h)`. Start from a template in §2 instead of
   inventing spacing, and state the panel each tile belongs to.
4. If export or a scheduled email is in scope, check every tile against the page guide in §1 and
   shift bands down rather than letting one straddle a page break.
5. Add board filters last, one row per model that must be filtered, then sharing or embedding.
6. Deliver numbered UI steps against real labels from boards.md, then have the user apply them
   and paste back what they see before you move on.
