---
name: schedule
description: Set up recurring email delivery of a DataSights board — days/times, recipients, PDF or PowerPoint attachment, and the active window. Pick this when a board should land in someone's inbox on a cadence.
argument-hint: "[board + cadence + recipients, e.g. 'email the Sales board as PDF every Mon 08:00 to the ops team']"
---

# Schedule a board email

Request: $ARGUMENTS

If that is empty, ask for: which **board** (and which panels), the **days and times**, the
**recipients** (registered users and/or other emails), **PDF** or **PowerPoint (PPTX)**, and the
**active from / active to** window. Do not proceed on guesses — every one of these is a required
field on the scheduled board.

## Read first, in this order

1. `../datasights-guide/SKILL.md` — hard rules (guide-only: you never create the schedule
   yourself), the *italic label* → **bold value** convention, the roles digest, and verification
   loops. Invoking this command does not load it, so read it now.
2. `../datasights-guide/references/modules/schedules.md` — the authority for this task: the
   two-part model (*Schedules* = reusable days/times; *Scheduled Boards* = the email job), the
   exact field names on the detail page, and the troubleshooting entries.
3. `../datasights-guide/references/dashboard-design.md` §1 *The grid system* — a scheduled
   attachment IS an export, so the 22-row page-break guide applies. Check the board's layout
   before the first send.
4. `../datasights-guide/references/modules/boards.md` — only if the board doesn't exist yet or
   its panels/filters need changing first.

## Task frame

1. **Check the role.** Schedules need **editor** or **admin**. A **member** has every other build
   permission but not `/schedules` — if the user is a member or viewer, say so, name the role, and
   point at *Settings* → *Access*. Also confirm they are in the room that holds the board.
2. **Schedule before scheduled board.** Reuse an existing schedule if the days/times match; the
   built-in **Default** ones are read-only, so a custom cadence means creating a new schedule.
3. **Then the scheduled board**, following the configure workflow in the module doc field by
   field — don't paraphrase from memory, quote the real labels.
4. **Audit the layout for export** before declaring done: any tile straddling a 22-row multiple is
   cut across two pages in the attachment. Propose the nudge (shrink or move) rather than shipping
   a split tile.
5. **Verify.** Have the user set a near-future time on an active schedule, confirm delivery, and
   paste back what arrived. If nothing sends, walk the module doc's *Scheduled Board Does Not Send
   Email* checklist rather than re-deriving it.
