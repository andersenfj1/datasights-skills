---
name: datasights
description: Entry point for the DataSights toolkit. Invoke when you want to see what DataSights help is available, or you know you need DataSights help but not which command — it routes you to the right one. For a specific job, the dedicated commands are faster.
argument-hint: "[what you want to do, or leave blank to see the menu]"
---

# DataSights — where do you want to start?

The user typed: **$ARGUMENTS**

## If they gave you something

Do not show the menu. Read `../guide/SKILL.md` for the hard rules and the
mental model, then pick the closest specialist below and follow that file instead —
they are all siblings of this one:

| If it is about… | Follow |
| --- | --- |
| raw data → finished dashboard, end to end | `../build/SKILL.md` |
| something empty, wrong, greyed out, or failing | `../fix/SKILL.md` |
| writing or repairing model SQL | `../sql/SKILL.md` |
| connecting a source, CSV import, PDF/spreadsheet conversion | `../data/SKILL.md` |
| one widget: which type, how to configure it | `../widget/SKILL.md` |
| board layout, panels, filters, cross-filtering, exports | `../board/SKILL.md` |
| emailing a board on a schedule | `../schedule/SKILL.md` |
| KPI threshold alerts | `../trigger/SKILL.md` |
| users, access, roles, palettes, Maps keys, writeback | `../admin/SKILL.md` |
| anything else, or a plain how-to question | `../guide/SKILL.md` |

Say which one you picked in a single short line, then get on with the work. Do not
make the user re-state their request.

## If they gave you nothing

Show this, verbatim and unpadded, then stop and wait:

> **DataSights toolkit.** I answer how-to questions for every module and turn raw data
> into finished dashboards — instructions, model SQL, and exact widget settings that
> you apply in the app. I never touch your app or your databases.
>
> | Command | For |
> | --- | --- |
> | `/datasights:build` | raw data → finished dashboard, end to end |
> | `/datasights:fix` | something is empty, wrong, greyed out, or failing |
> | `/datasights:sql` | write or repair a model query |
> | `/datasights:data` | connect a source, import a CSV, convert a PDF/spreadsheet |
> | `/datasights:widget` | build or configure one widget |
> | `/datasights:board` | layout, panels, filters, cross-filtering, exports |
> | `/datasights:schedule` | recurring board delivery by email |
> | `/datasights:trigger` | KPI threshold alerts |
> | `/datasights:admin` | users, access, palettes, Maps keys, writeback enablement |
>
> You never have to use these — just ask a DataSights question in plain language and
> I pick the right one. Tell me what you are trying to do and we will start there.

Then wait for their answer and route as above. Do not start interviewing, do not guess
at a task, and do not dump reference material into the reply.
