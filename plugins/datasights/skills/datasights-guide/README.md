# DataSights Guide — Claude Skill

A guide-only Claude skill for the DataSights BI platform. Once installed, Claude
can answer "how do I…" questions about every DataSights module (sources, models,
widgets, boards, schedules, triggers, settings) and can take you from raw data —
a database, a CSV, a spreadsheet, even pasted rows — to a finished dashboard:
interviewing you, proposing data models, writing the model SQL, specifying every
widget setting, laying out the board, and verifying each step against expected
values computed from your sample data.

## What "guide-only" means

Claude never touches your DataSights instance or your databases. It does not call
the app's API, open database connections, or run queries. Everything it produces —
steps, SQL, settings, layouts — is applied by **you** in the app UI, and Claude
verifies results only from output you paste back. The only "work" Claude performs
itself is in-chat data conversion (for example turning an XLSX or PDF into a CSV
you can import) and computing expected values for verification.

## Install

**Recommended — from the plugin marketplace** (gets you updates):

```
/plugin marketplace add <owner>/<repo>
/plugin install datasights@datasights-skills
```

Later, when a new version ships:

```
/plugin marketplace update datasights-skills
/plugin update datasights@datasights-skills
```

Private repositories work — Claude Code reuses your existing git credentials
(`gh auth login`, macOS Keychain, or a loaded `ssh-agent` key).

**Alternative — from a zip** (no repo access needed, but no auto-updates):

1. Unzip the package so that `SKILL.md` ends up at one of:
   - `~/.claude/skills/datasights-guide/SKILL.md` — available in all your projects
   - `<your-project>/.claude/skills/datasights-guide/SKILL.md` — this project only
2. Start a new Claude Code session (skills are discovered at session start).

Either way the folder must keep its `references/` subdirectory — that is where
the detailed module documentation lives. Claude.ai users can upload the same zip
under Settings → Capabilities → Skills.

## Verify the install

In a fresh session, ask Claude:

> Which DataSights widget types need extra setup before I can use them?

A working install answers with **Summarise** (studio AI assistant + tenant tier),
**Maps** (Google Maps API key + Map ID in Settings), **Data grid editing** and
**Form** (source writeback enabled by a studio admin, MySQL/PostgreSQL only). If
Claude answers generically or asks what DataSights is, the skill is not loading —
check the folder path and restart the session.

## Contents

- `SKILL.md` — routing, intake interview, and hard rules
- `references/modules/` — per-module user documentation (mirrored from the app
  repo; do not edit by hand)
- `references/widget-settings-reference.md` — generated widget-settings catalog
- `references/sql-contract.md`, `getting-data-in.md`, `dashboard-design.md`,
  `gotchas.md`, `recipe-template.md` — curated guides

## About

Every file in this package is generated and published automatically from the
DataSights source repository, so hand-edits are overwritten on the next release.
For questions or corrections, contact your DataSights representative.
