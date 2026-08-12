# Triggers

Audience: End users
Last reviewed: 2026-04-22
Routes: `/triggers`, `/triggers/:id`

## Purpose

Triggers monitor KPI widgets on a schedule. When a KPI value matches the configured condition, a trigger can send email notifications to registered users and other email addresses.

## Prerequisites

- You must be signed in and have an active room selected.
- You need a schedule-capable role, such as **Admin** or **Editor**, to manage triggers.
- A KPI widget must exist in the room before a trigger can be fully configured.
- Schedules can be reused by triggers, or a trigger can use a preset interval.

## Main User Workflows

### Find A Trigger

1. Open **Triggers** from the app navigation.
2. Type in **Search triggers** when needed.
3. Review **Name**, **Schedule**, **Active**, **Created at**, and **Actions**.
4. Click the trigger name to open details.

If no room is selected, the page shows **Select a room from the user menu to view triggers.** If no triggers exist, it shows **No triggers found.**

### Add A Trigger

1. Open **Triggers**.
2. Click **Add**.
3. In **Add trigger**, enter **Name**.
4. Click **Save**.
5. Open the trigger detail page.
6. Complete KPI, condition, run schedule, and email settings.
7. Click **Save**.

### Rename A Trigger From The List

1. Open **Triggers**.
2. Find the trigger row.
3. Click the edit icon.
4. In **Edit trigger**, update **Name**.
5. Click **Save**.

### Configure A Trigger

1. Open a trigger.
2. Use **Active** to enable or disable it.
3. Enter **Name**.
4. Select **KPI widget**.
5. Choose **Condition**.
6. Enter **Value**.
7. Choose **Match mode**.
8. Choose **Run trigger**.
9. Configure email settings if notifications should be sent.
10. Click **Save**.

### Choose A Condition

1. In the trigger detail page, select **Condition**.
2. Choose one of:
   - **Equal to**
   - **Not equal to**
   - **Less than**
   - **Less than or equal to**
   - **Greater than**
   - **Greater than or equal to**
3. Enter the numeric **Value**.
4. Choose **Match mode**:
   - **Any value**
   - **More than one value**
   - **All values**

### Choose When The Trigger Runs

1. Open the trigger detail page.
2. Under **Run trigger**, choose a preset or schedule.
3. Preset options include **Every 5 mins**, **Every 30 mins**, and **Every hour**.
4. Schedule options appear as **Schedule: Name** for saved schedules.
5. Click **Save**.

### Configure Trigger Email

1. Open a trigger.
2. Turn on **Send email**.
3. Choose **Email delivery**:
   - **Triggered + widget link**
   - **Custom text**
4. Enter **Email subject**.
5. If using **Custom text**, enter **Custom text**.
6. Select **Registered users**.
7. Add **Other emails** when needed.
8. Click **Save**.

In link mode, the email sends a standard message with a direct link to the triggered widget.

### Delete A Trigger

1. Open **Triggers**.
2. Find the trigger row.
3. Click the delete icon.
4. In **Delete trigger**, confirm **Delete**.

## Feature Reference

- **KPI widget** selects the widget being monitored.
- **Condition** controls the comparison operator.
- **Value** is the numeric threshold.
- **Match mode** controls how multiple KPI values must satisfy the condition.
- **Run trigger** controls the checking cadence.
- **Active** enables or disables the trigger.
- **Email delivery** controls whether the email uses a standard widget link or custom text.
- **Registered users** and **Other emails** define recipients.

## Permissions And Availability

- **Admin** and **Editor** style roles can manage triggers.
- Users without trigger access do not see the Triggers route.
- The detail page is read-only when you cannot edit.
- A trigger needs a KPI widget and a valid run schedule before it can run usefully.
- Email fields are disabled until **Send email** is enabled.

## Troubleshooting

### No KPI Widgets Available Appears

Create a KPI widget in the current room before configuring the trigger.

### Save Fails With Condition Value Must Be Numeric

Enter a numeric value in **Value**.

### Custom Text Is Required

When **Email delivery** is **Custom text**, enter content in **Custom text** before saving.

### Trigger Does Not Send Email

Confirm **Active** is enabled, **Send email** is enabled, recipients are selected or entered, and the KPI value actually satisfies the configured condition.

## Related Routes And Terms

- Routes: `/triggers`, `/triggers/:id`
- Related terms: trigger, KPI widget, condition, match mode, run trigger, email delivery, registered users, threshold
