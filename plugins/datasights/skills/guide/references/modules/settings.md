# Settings

Audience: End users
Last reviewed: 2026-09-18
Routes: `/settings`

## Purpose

Settings is where administrators manage users, studios, rooms, access, custom color palettes, AI Assistant settings, Maps settings, and AI apps. The Settings page is split into tabs so each area can be documented, exported, and trained separately.

This overview is the module index. Use the tab-level Markdown files as the source for separate PDFs, slide decks, in-app help, or assistant answers.

## Tab Training Sources

- [Users](settings/users.md): create users, edit profiles, resend invites, reset passwords, and delete users.
- [Studios](settings/studios.md): create, rename, and delete studios.
- [Rooms](settings/rooms.md): create, rename, and delete rooms inside a studio.
- [Access](settings/access.md): add, change, and remove studio or room membership.
- [Color Palettes](settings/color-palettes.md): create and maintain custom widget color palettes.
- [AI Assistant](settings/ai-assistant.md): choose the assistant model, store the API key, and enable or disable the assistant.
- [Maps](settings/maps.md): configure Google Maps keys, map IDs, and default map locations.
- [AI apps](settings/ai-apps.md): allow or stop AI apps (Claude, ChatGPT, Cursor) connecting to the workspace, and disconnect them.

## Prerequisites

- You must be signed in.
- The Settings navigation item is available when you are an admin for the active studio.
- A studio must exist before you can manage rooms, users, access, palettes, assistant settings, or maps settings.
- **Color Palettes**, **Maps** and **AI apps** are shown only when you are an admin for at least one studio.
- Actions inside each tab may be disabled when you do not have admin access to the selected studio.

## Main User Workflows

### Open Settings

1. Sign in to the application.
2. Select an active room or studio where you have admin access.
3. Open **Settings** from the app navigation.
4. Select a tab: **Users**, **Studios**, **Rooms**, **Access**, **Color Palettes**, **AI Assistant**, **Maps**, or **AI apps**.

### Use A Tab-Specific Training Source

1. Choose the Settings tab you want to document or train.
2. Use the matching help page for that Settings tab.
3. Generate the required output from that Markdown file, such as a PDF, deck, help article, or assistant retrieval resource.
4. Keep generated outputs outside the source-of-truth path unless they are intentionally permanent reference material.

## Feature Reference

- **Users** manages account invitations, profile fields, activation follow-up, passwords, and user deletion.
- **Studios** manages the top-level workspaces that contain rooms and studio-scoped settings.
- **Rooms** manages the room list inside each studio.
- **Access** manages which users can access a studio or room and what role they have.
- **Color Palettes** manages custom palettes used by widget chart settings.
- **AI Assistant** manages the studio-level assistant model, API key, and enabled state.
- **Maps** manages studio-level map runtime settings used by map widgets.
- **AI apps** turns AI app connections on or off for the whole workspace and lists every connected AI app. It is different from **AI Assistant**, which is the assistant built into the app.

## Permissions And Availability

- Settings itself requires admin access for the active studio.
- Studio admins can manage users, studios, rooms, access, palettes, assistant settings, and maps settings for studios they administer.
- Users without admin access may see disabled actions, **View only**, or messages explaining that only studio admins can manage the selected item.
- **Color Palettes**, **Maps** and **AI apps** do not appear when the signed-in user is not an admin for any studio.

## Troubleshooting

### I Cannot See Settings In The Navigation

Select a room or studio where you have admin access. If Settings still does not appear, ask an administrator to update your role.

### A Tab Is Missing

**Color Palettes**, **Maps** and **AI apps** are only shown to users who administer at least one studio.

### An Add, Edit, Delete, Or Save Button Is Disabled

Confirm that a studio is selected and that you are an admin for that studio.

## Related Routes And Terms

- Route: `/settings`
- Tabs: **Users**, **Studios**, **Rooms**, **Access**, **Color Palettes**, **AI Assistant**, **Maps**, **AI apps**
- Related terms: settings, user management, studio management, room management, permissions, access, roles, assistant settings, maps settings, palettes, AI apps, connected apps
