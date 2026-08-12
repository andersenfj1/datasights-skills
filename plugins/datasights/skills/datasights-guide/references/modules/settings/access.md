# Settings: Access

Audience: End users
Last reviewed: 2026-04-22
Routes: `/settings`
Settings tab: **Access**
Generation target: Settings Access training material

## Purpose

The **Access** tab lets studio admins manage who belongs to a studio or room and what role each person has.

## Prerequisites

- You must open **Settings**.
- You must be a studio admin for the studio or room you want to manage.
- Users must exist before they can be added to studio or room access.
- Studios and rooms must exist before memberships can be assigned.

## Main User Workflows

### Manage Studio Access

1. Open **Settings**.
2. Select **Access**.
3. In **Studio Access**, choose a studio from the dropdown.
4. Use **Search members** to filter the member table when needed.
5. To add a member, choose a user from **Select user**.
6. Choose a role: **Admin**, **Editor**, or **Viewer**.
7. Click **Add**.

The studio member table shows **User**, **Role**, and **Actions**.

### Change A Studio Member Role

1. In **Studio Access**, choose the studio.
2. Find the member row.
3. Use the **Role** dropdown.
4. Select **Admin**, **Editor**, or **Viewer**.

### Remove Studio Access

1. In **Studio Access**, choose the studio.
2. Find the member row.
3. Click the delete icon.
4. In **Remove studio access?**, click **Remove**.

### Manage Room Access

1. Open **Settings**.
2. Select **Access**.
3. In **Room Access**, choose a room from the dropdown.
4. Use **Search members** to filter the member table when needed.
5. To add a member, choose a user from **Select user**.
6. Choose a role: **Admin**, **Editor**, or **Viewer**.
7. Click **Add**.

The room member table shows **User**, **Role**, and **Actions**.

### Change A Room Member Role

1. In **Room Access**, choose the room.
2. Find the member row.
3. Use the **Role** dropdown.
4. Select **Admin**, **Editor**, or **Viewer**.

### Remove Room Access

1. In **Room Access**, choose the room.
2. Find the member row.
3. Click the delete icon.
4. In **Remove room access?**, click **Remove**.

## Feature Reference

- **Studio Access** controls membership at the studio level.
- **Room Access** controls membership for a single room.
- **Search members** filters the member list in each access card.
- **No available users** means every eligible user is already assigned or no eligible users are available.
- **Adding...** appears while a new access assignment is being saved.
- **Select a studio to view members.** appears before a studio is selected.
- **Select a room to view members.** appears before a room is selected.

## Permissions And Availability

- Only studio admins can manage studio access.
- Only studio admins can manage room access for rooms in that studio.
- When you cannot manage the selected studio or room, the tab shows **Only studio admins can manage access for this studio.** or **Only studio admins can manage access for this room.**
- Removing your own access can change your active studio or room after the update.

## Troubleshooting

### I Cannot Add A Member

Confirm that you selected a studio or room, that the user exists, and that your role is **Admin** for the studio.

### No Available Users Appears

All eligible users may already have access to the selected studio or room. Create or invite the user first from **Users** if needed.

### The Member List Is Empty

Clear **Search members**. If it is still empty, no members have been assigned yet.

## Related Routes And Terms

- Route: `/settings`
- Tab: **Access**
- Related terms: access, permissions, roles, membership, studio access, room access, admin, editor, viewer
