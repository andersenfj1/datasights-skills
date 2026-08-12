# Schedules

Audience: End users
Last reviewed: 2026-04-22
Routes: `/schedules`, `/schedules/scheduled-boards/:id`

## Purpose

Schedules define reusable days and times. Scheduled Boards use those schedules to send board exports to users by email during an active window.

## Prerequisites

- You must be signed in and have an active room selected.
- You need a schedule-capable role, such as **Admin** or **Editor**, to manage schedules and scheduled boards.
- A board must exist before a scheduled board can be fully configured.
- A schedule must exist before a scheduled board can run.

## Main User Workflows

### Find A Schedule

1. Open **Schedules** from the app navigation.
2. Type in **Search schedules** when needed.
3. Review **Name**, **Days**, **Times**, and **Actions**.

Default schedules are marked **Default** and are **Read only**.

### Add A Schedule

1. Open **Schedules**.
2. Click **Add** in the **Schedules** section.
3. In **Add schedule**, enter **Name**.
4. Select one or more **Days**.
5. Select one or more **Times**.
6. To add a custom time, enter it as `HH:MM` in **Add time (HH:MM)** and click **Add time**.
7. Click **Create schedule**.

Times use the local browser time zone when created.

### Edit A Schedule

1. Open **Schedules**.
2. Find the schedule row.
3. Click the edit icon.
4. In **Edit schedule**, update **Name**, **Days**, or **Times**.
5. Click **Save changes**.

Default schedules cannot be edited.

### Delete A Schedule

1. Open **Schedules**.
2. Find the schedule row.
3. Click the delete icon.
4. In **Delete schedule**, confirm **Delete**.

### Find A Scheduled Board

1. Open **Schedules**.
2. Go to **Scheduled Boards**.
3. Type in **Search scheduled boards** when needed.
4. Review **Name**, **Board**, **Schedule**, **Active**, **Active window**, and **Actions**.
5. Click the scheduled board name or edit icon to open details.

### Add A Scheduled Board

1. Open **Schedules**.
2. In **Scheduled Boards**, click **Add**.
3. In **Add scheduled board**, enter **Name**.
4. Click **Save**.
5. Open the scheduled board detail page.
6. Complete the board, schedule, active window, panels, and email settings.
7. Click **Save**.

### Configure A Scheduled Board

1. Open a scheduled board.
2. Use **Active** to enable or disable the scheduled board.
3. Enter **Name**.
4. Select **Board**.
5. Choose whether to **Include default board filters**.
6. Select **Panels**, or choose **All panels**.
7. Select **Schedule**.
8. Set **Active from**.
9. Set **Active to (optional)** or leave it blank for indefinite.
10. Turn on **Send email** when the run should email recipients.
11. Choose **Attachment format**: **PDF** or **PowerPoint (PPTX)**.
12. Enter **Email subject** and **Email content**.
13. Choose **Registered users**.
14. Add **Other emails** when needed.
15. Click **Save**.

## Feature Reference

- **Schedules** lists reusable schedule definitions.
- **Scheduled Boards** lists board email/export jobs.
- **Days** supports Mon, Tue, Wed, Thu, Fri, Sat, and Sun.
- **Times** offers 30-minute options and custom `HH:MM` entries.
- **Active** controls whether a scheduled board can run.
- **Active from** is required for scheduled boards.
- **Active to (optional)** limits the scheduled board end date.
- **Panels** controls which board panels are included.
- **Include default board filters** applies saved board filters during export.
- **Attachment format** controls whether emails attach PDF or PowerPoint.
- **Registered users** and **Other emails** define recipients.

## Permissions And Availability

- **Admin** and **Editor** style roles can manage schedules.
- Users without schedule access do not see the Schedules route.
- Default schedules are read-only.
- Scheduled boards need a valid board, schedule, active-from date, and active state to run as expected.

## Troubleshooting

### Schedules Is Missing In Navigation

Select a room or studio where you have schedule access.

### Create Schedule Fails

Confirm that **Name**, at least one **Day**, and at least one **Time** are selected.

### Custom Time Is Rejected

Use `HH:MM` format, such as `09:30` or `17:00`.

### Scheduled Board Does Not Send Email

Confirm **Active** is checked, **Send email** is enabled, a schedule is selected, the active window includes today, and recipients are selected or entered.

## Related Routes And Terms

- Routes: `/schedules`, `/schedules/scheduled-boards/:id`
- Related terms: schedule, scheduled board, days, times, active window, PDF email, PowerPoint email, board automation
