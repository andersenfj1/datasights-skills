# DataSights Claude plugins

Claude Code marketplace for the DataSights skills. Install once, then `/plugin update` whenever a new version ships.

## Install

```
/plugin marketplace add https://github.com/andersenfj1/datasights-skills
/plugin install datasights@datasights-skills
```

Then start a new session and ask a DataSights question — for example
"Which DataSights widget types need extra setup before I can use them?".

## Update

```
/plugin marketplace update datasights-skills
/plugin update datasights@datasights-skills
```

The `andersenfj1/datasights-skills` shorthand also works, but it clones over SSH by default, so the
full URL above is the safer choice if you have no GitHub SSH key configured.

## Contents

- `plugins/datasights/skills/datasights-guide/` — the DataSights build guide skill

## About

Every file here is generated and published automatically from the DataSights
source repository. Please do not open pull requests against them — edits are
overwritten on the next release. For questions or corrections, contact your
DataSights representative.
