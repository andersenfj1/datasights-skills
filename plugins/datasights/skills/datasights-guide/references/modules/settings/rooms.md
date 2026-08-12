# Settings: Rooms

Audience: End users
Last reviewed: 2026-04-22
Routes: `/settings`
Settings tab: **Rooms**
Generation target: Settings Rooms training material

## Purpose

The **Rooms** tab lets studio admins create, rename, search, and delete rooms inside a selected studio. Rooms are where boards, widgets, and room-specific access are organized.

## Prerequisites

- You must open **Settings**.
- You must be an admin for the selected studio.
- A studio must exist before rooms can be added.

## Main User Workflows

### Select A Studio

1. Open **Settings**.
2. Select **Rooms**.
3. Use the studio dropdown to choose a studio.

The dropdown shows **Loading studios...** while loading and **No studios** when none are available.

### Find A Room

1. Open **Settings**.
2. Select **Rooms**.
3. Choose a studio from the studio dropdown.
4. Type in **Search rooms**.
5. Review the table columns **Name**, **Studio**, **Boards**, and **Created At**.

### Add A Room

1. Open **Settings**.
2. Select **Rooms**.
3. Choose the studio that should contain the room.
4. Click **Add Room**.
5. In **Add room**, confirm the displayed **Studio**.
6. Enter **Name**.
7. Click **Save**.

### Rename A Room

1. Open **Settings**.
2. Select **Rooms**.
3. Choose the studio that contains the room.
4. Find the room row.
5. Click the edit icon.
6. In **Edit room**, update **Name**.
7. Click **Save**.

### Delete A Room

1. Open **Settings**.
2. Select **Rooms**.
3. Choose the studio that contains the room.
4. Find the room row.
5. Click the delete icon.
6. In **Delete room?**, confirm the delete action.

## Feature Reference

- The studio dropdown controls which studio's rooms are listed.
- **Search rooms** filters the room table.
- **Add Room** creates a room in the selected studio.
- The **Boards** column shows how many boards belong to each room.
- **View only** appears when you can see the room but cannot manage it.

## Permissions And Availability

- Only studio admins can add, edit, or delete rooms for that studio.
- **Add Room** is disabled when no studio is selected or you cannot manage rooms in the selected studio.
- Room-level access does not grant room creation or deletion rights; those actions are managed at the studio level.

## Troubleshooting

### Add Room Is Disabled

Select a studio and confirm that your role in that studio is **Admin**.

### No Rooms Appear

Confirm that the correct studio is selected. If the studio is new, create the first room with **Add Room**.

### A Room Row Shows View Only

You can see the room but are not a studio admin for the room's studio.

## Related Routes And Terms

- Route: `/settings`
- Tab: **Rooms**
- Related terms: room, add room, edit room, rename room, delete room, boards, studio
