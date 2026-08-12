---
name: admin
description: Administer a DataSights studio — invite users, create studios and rooms, grant roles, manage color palettes, and switch on the gated features (AI assistant/Summarise, Maps keys, source writeback for editable grids and Forms). Pick this for anything under /settings or "who can do what".
argument-hint: "[the admin task, e.g. 'invite 3 viewers to the Ops room' or 'turn on Maps for the Retail studio']"
disable-model-invocation: true
---

# Studio administration

Request: $ARGUMENTS

If that is empty, ask which **studio** (and room, if room-scoped), what the **outcome** should be
(new user, role change, new room, palette, a feature switched on), and whether the user is an
**admin on that studio** — every action below requires it.

## Read first, in this order

1. `../datasights-guide/SKILL.md` — hard rules (guide-only: you never open `/settings` or change
   access yourself), the *italic label* → **bold value** convention, the roles digest, and the
   gated-widget table. Invoking this command does not load it, so read it now.
2. `../datasights-guide/references/modules/settings.md` — the tab index. Use it to decide which
   tab owns the request and to check tab visibility rules before giving steps.
3. The one matching tab file — the authority for the exact labels, dialogs, and messages:
   `../datasights-guide/references/modules/settings/users.md` (invites, profiles, resend,
   passwords, delete) · `../datasights-guide/references/modules/settings/studios.md` · `../datasights-guide/references/modules/settings/rooms.md` · `../datasights-guide/references/modules/settings/access.md` (studio + room
   membership and roles) · `../datasights-guide/references/modules/settings/color-palettes.md` · `../datasights-guide/references/modules/settings/ai-assistant.md` (model, API key,
   *Assistant Enabled*) · `../datasights-guide/references/modules/settings/maps.md` (API key, *Map ID*, default location).
4. `../datasights-guide/references/modules/sources.md` §*Allow Data-grid Writeback* — writeback
   lives on the source, not in `/settings`, but it is an admin-only toggle. Read it for any
   editable-grid or Form request.
5. `../datasights-guide/references/gotchas.md` §*Feature is missing from the type list / settings
   (availability gates)* — the layer-by-layer gate table when something is greyed out or absent.

## Task frame

1. **Confirm admin on the right studio first.** `/settings` only appears for admins of the active
   studio, and *Color Palettes* and *Maps* are hidden entirely from non-admins. If the user is not
   an admin, stop and name who must do it — do not walk them through a tab they cannot open.
2. **Order matters.** Users must exist before *Access* can assign them; a studio must exist before
   rooms, palettes, assistant, or maps settings; a room must exist before a user can be invited
   into one.
3. **This is where gated features get switched on.** Summarise needs the tenant tier *plus*
   *Settings* → *AI Assistant* enabled; Maps needs *Settings* → *Maps*; editable data grids and
   Forms need *Allow data-grid writeback* on the source (MySQL/PostgreSQL only), then the
   per-widget and per-column toggles an editor sets. Walk the gate table top-to-bottom — each
   layer gates the next, so enabling only one changes nothing visible.
4. **Grant the least role that works.** Map the need to a role from the digest (viewer → view
   only; editor → build plus schedules and triggers; admin → all of the above plus `/settings`),
   and remember room role wins over studio role.
5. **Verify.** Have the user re-open the tab (or the affected widget/type picker) and paste back
   what they see — the saved-settings toast, the member row, or the feature now appearing. Access
   and gate changes can need a reload or a studio/room switch before they take effect.
