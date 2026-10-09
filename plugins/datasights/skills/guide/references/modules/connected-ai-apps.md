# Connected AI Apps

Audience: End users
Last reviewed: 2026-10-08
Routes: `/connect/ai`, `/account`, `/settings`
Settings tab: **AI apps**
Generation target: Connected AI apps training material

## Purpose

You can connect an AI app such as Claude, ChatGPT or Cursor to your workspace. Once connected, the AI app works in the workspace as you: it can only see and change what you can see and change. It can look up your studios, rooms, sources, models, widgets, boards, triggers and schedules, and it can build, change and delete them for you. It can also tell you whether a trigger fired and whether a scheduled email went out. It cannot manage users or your sign-in.

An AI app works in one room at a time, its working room. It creates, changes and deletes widgets and boards only in that room, and models only in that room's studio. It starts in the room you last opened in DataSights, and it moves to another room only when you ask it to. It can still look at everything you can see.

Before an AI app creates, changes or deletes anything, it shows you the change it has prepared, and it saves the change only after you agree. The change it shows you always says where it will be made: the workspace, the studio and the room.

An AI app only gets access after you click **Allow** on the consent page. Being signed in to the workspace in your browser is not enough on its own.

## Prerequisites

- You must have an account in the workspace.
- AI app connections must be turned on for the workspace. They are on by default; an administrator can turn them off in **Settings** > **AI apps**.
- If you have been asked to reset your password, reset it before you connect an AI app.

## Main User Workflows

### Connect An AI App

1. In your AI app, add the connector `https://mcp.datasights.co.za/mcp`.
2. When the AI app opens a browser window, enter your workspace address, for example `https://acme.datasights.co.za`, or your workspace's own domain if it has one.
3. If you are not signed in, sign in. You return to the consent page afterwards.
4. Read the consent page: **Allow {app} to use {workspace}?** It shows who you are signed in as, what the app can do, and where it returns to.
5. Click **Allow**.
6. The page shows **Returning to {app}...** and sends you back to your AI app.

The AI app starts working in the room you last opened in DataSights. To check, ask it which room it is working in.

To stop the connection before it starts, click **Cancel** instead. The AI app is told you declined.

### Choose The Room The AI App Works In

1. Ask your AI app which room it is working in. It answers with the workspace, studio and room, for example **Acme Insights › Head office › Sales**.
2. To work somewhere else, tell it which room, for example "work in the Operations room". It switches and tells you the room it now works in.
3. When it prepares a change, read the first line it shows you. It names the change and where it will be made, for example **Create widget 'Revenue by month' (Bar chart) in Acme Insights › Head office › Sales**.

- The app never moves to another room on its own. It switches only when you want to work in another room, for example because you asked for a change there, and it tells you when it does.
- Opening another room in DataSights does not move the app. When the two differ, the app knows and can tell you.
- Models belong to a studio, so the app creates and changes models in the studio of its working room.
- If you had not opened a room in DataSights when you connected the app, or that room was deleted or you lost access to it, the app asks which room to work in before it changes anything.
- An AI app works in a studio through one of its rooms, so it cannot change the models of a studio that has no rooms.

### Check Where The App Returns To

The consent page shows one of these:

- **Returns to** a website name with a verified mark: the connection service has confirmed this app.
- A warning that the workspace **can't confirm this app**: only continue if you started this connection yourself in that AI app.
- **Returns to an app on your computer.**: the AI app runs on your own computer, such as a desktop app or code editor.

### Sign In As Someone Else

1. On the consent page, click **Not you?**.
2. Sign in with the right account.
3. You return to the consent page for the same request.

### See And Disconnect Your AI Apps

1. Open your account page (**Account**).
2. Find **Connected AI apps**. Each row shows the app name, where it returns to, when it was connected and when it was last used.
3. Click **Disconnect** on the app you want to remove.
4. In the confirmation, click **Disconnect**.

The app stops working with your account straight away. If you have no connected apps, the section shows **No AI apps connected** and the connector address to use.

### Build And Edit Models With An AI App

1. Ask your AI app which sources a studio has, and which tables and columns a source contains.
2. Ask it to create a model, or to change the SQL, name or source of an existing one. The app writes the SQL and runs it against the source, the same check as **Run** in the model editor. SQL that does not run is never saved; the app sees the error and can correct it.
3. Read the change the app shows you: the model's name, its source, the SQL and the first rows the SQL returns. Nothing has changed yet.
4. Tell the app to go ahead. It saves exactly the change it showed you. Your AI app may also ask for your permission at this point.
5. Open the model in **Models** to check it. A saved SQL change appears in the model's **Version history** with you as the author and the commit message the app gave.

Deleting a model works the same way. The app first shows which model it would delete and how many saved versions it has, and deletes it only after you agree. Deleting removes the model and its version history, like **Delete** in **Models**, and widgets built on that model stop working.

### Build Widgets And Boards With An AI App

1. Ask your AI app for the dashboard you want. It builds it in its working room, in this order: a model, then the widgets, then the board.
2. The app prefers one model that several widgets share. Each widget then groups and adds up the model's rows in its own query, and board filters and click filters work across all of those widgets. It creates a new model only when no existing model has the columns it needs.
3. The app prefers the built-in widget types, such as **KPI**, bar and line charts, **Pie chart**, **Data grid** and **Pivot table**. It uses **Custom HTML** only when you ask for it or no built-in type can show the result, and then tells you why.
4. For each widget, read what the app shows you: the type, the model, the query, the fields it uses, the first rows of data and any warnings. DataSights has already checked the widget the way the widget editor would. Agree, and the app saves it.
5. For the board, read the panels, where each widget sits on the 12-column grid, and the board filters. Agree, and the app builds the board.

**Maps** and **Summarise** widgets cannot be built through an AI app yet.

### Set Up An Alert With An AI App

1. Ask your AI app for the alert you want, for example "email me and Sam when open orders drop below 10". An alert is a trigger: it watches one **KPI** widget in the app's working room.
2. Say how often it should check: every 5, 30 or 60 minutes, or on a schedule. Every check that matches sends another email, so an alert that checks every 5 minutes and stays matched emails every 5 minutes. The app suggests every 60 minutes or a schedule unless you want more.
3. Name the recipients by email address. People with a role in the room or its studio are saved as members; anyone else is saved as an address.
4. Read the change the app shows you: the KPI, the condition, how often it checks and who gets the email. For a trigger that checks every few minutes, it also says how many emails a day it can send. Agree, and the app saves it.
5. To stop it for a while, ask the app to pause it; ask again to resume it.
6. To see whether it fired, ask the app. It shows each recent check: the value it read, whether it matched, and whether the email was sent.

### Email A Board On A Schedule With An AI App

1. Ask your AI app to email a board, for example "send the Revenue overview board to the finance team every Monday at 07:00".
2. The app uses a schedule of the working room's studio. Built-in schedules, such as **Daily 08:00**, are marked **Default** on the **Schedules** page and cannot be changed. For another cadence the app prepares a new schedule with its days, times and time zone.
3. Read the change the app shows you: the board and its panels, the schedule in words, the attachment, the recipients and the dates it may send between. Agree, and the app saves it.
4. Choose a PDF (A4 landscape) or a PowerPoint of picture slides. Each panel starts a new page or slide. All panels the board has now are sent unless you name some; panels added later are not sent until the scheduled email is changed.
5. To stop it for a while, ask the app to pause it.
6. To check whether it went out, ask the app. It shows each recent run and whether the email was sent, failed or was held back, with the reason.

## Feature Reference

- A connection lasts until it is disconnected, or until it has not been used for 30 days.
- Changing or resetting your password disconnects all of your AI apps. Connect them again afterwards.
- When an administrator deletes your account, all of your AI apps are disconnected.
- An administrator can turn AI apps off for the whole workspace. Connected apps then stop working until AI apps are turned back on; they are not disconnected.
- An AI app cannot open user management, provisioning, the in-app AI Assistant, or AI app settings.
- A connection request is valid for 10 minutes and can be used once.
- AI apps can connect to any active workspace, on a DataSights address or on the workspace's own domain. A suspended workspace cannot be connected to, and connected apps stop working while it is suspended.
- An AI app makes changes in one room at a time, its working room: widgets and boards in that room, models in its studio. It starts in the room you last opened in DataSights when you connected it, and moves only when you ask it to switch rooms.
- The working room belongs to the connection, not to one conversation. If you use one connected AI app in several conversations, switching rooms in one of them switches it for all of them.
- Every change an AI app prepares names where it will be made: the workspace, the studio and the room. A change prepared in one room cannot be saved after the app has switched to another room.
- An AI app can list your studios, rooms, sources, models, widgets, boards, triggers, schedules and scheduled emails. It can show a source's tables and columns, a model's columns and version history, a widget's settings and the boards that show it, a board's panels and layout, and the members of a room. It can preview the first 10 rows of a query.
- To create, change or delete a model, widget, board, trigger, schedule or scheduled email, an AI app first prepares the change and shows it to you. It can save the change only after you agree, within 10 minutes, and each go-ahead works once. If someone saves the same thing in the meantime, nothing is changed and the app has to prepare the change again.
- An AI app can show the recent runs of a trigger or a scheduled email: when it ran, what it found, and whether the email was sent, failed or was held back. Run times are in the DataSights server's local time.
- Triggers and scheduled emails go to email addresses. Recipients with a role in the room, or in its studio, are saved as members, so a changed email address keeps working. For a scheduled email, that is the board's room. All recipients are in the email's To field.
- A trigger checks its KPI with its creator's access and without board filters.
- A scheduled email shows the board as its creator sees it. If the creator leaves the board's room and studio, the member with the highest role there is used instead.
- Built-in schedules cannot be changed or deleted. When an AI app changes or deletes another schedule, it first lists the scheduled emails and triggers that use it.
- An AI app always runs SQL against the source before it saves a model, and never saves SQL that fails. It checks a widget's settings against the columns of the widget's query before it saves the widget.
- A board change takes several steps. If one fails, the app reports which steps were done, and a new change can finish the board.

## Permissions And Availability

- Anyone with an account can connect AI apps for themselves and see and disconnect their own connections.
- Only administrators see **Settings** > **AI apps**, where they can turn AI apps off and disconnect anyone's AI app.
- An AI app can only do what its user is allowed to do. If you have view-only access to a room, so does the AI app.
- Creating, changing or deleting a model, and previewing a query, need the editor or admin role in the studio itself. A role in one of the studio's rooms is not enough. With view-only access, the AI app can only look at models, their columns and their version history.
- Creating, changing or deleting widgets and boards needs the editor or admin role in their room or in its studio.
- Triggers need the editor or admin role in their room or in its studio, also to be seen.
- Schedules and scheduled emails need the editor or admin role in the studio or in one of its rooms, also to be seen.
- If AI app connections are not set up on the server, the **Connected AI apps** section does not appear.

## Troubleshooting

### The Connect Page Says That Isn't A DataSights Address

The address you entered is not the address of a DataSights workspace. Check the spelling, and use the address you open DataSights on in your browser. If your workspace has its own domain and it still fails, ask an administrator to check that the domain is registered for your workspace.

### The Connect Page Says It Couldn't Find An Active Workspace

The workspace exists but is not active, for example because it is suspended. Ask an administrator.

### The Connect Page Says It Can't Check Addresses Right Now

The connection service cannot check workspace addresses at the moment. Wait a minute and try again. AI apps that are already connected keep working.

### This Connection Request Has Expired Or Was Already Used

The consent page shows **This connection request has expired or was already used. Go back to your AI app and connect again.** Requests last 10 minutes and work once. Start the connection again from your AI app.

### AI App Connections Are Turned Off For This Workspace

The consent page shows **AI app connections are turned off for this workspace. Ask an administrator.** An administrator can turn them on in **Settings** > **AI apps**.

### Reset Your Password First

The consent page shows **Reset your password first, then connect again.** Click **Reset password**, set a new password on your account page, then start the connection again from your AI app.

### Open This Page Directly

The consent page cannot be used inside another website. Open the link in its own browser tab.

### My AI App Stopped Working

- Check **Account** > **Connected AI apps**. If the app is not listed, it was disconnected, your password changed, or it was unused for 30 days. Connect it again.
- If it is listed, an administrator may have turned AI apps off for the workspace.

### The AI App Says It Cannot Change Models In A Studio

The AI app needs the editor or admin role in that studio. If you are a viewer, or you reach the studio only through one of its rooms, ask a studio admin for the editor role in **Settings** > **Access**.

### The AI App Says It Cannot Change Widgets Or Boards In A Room

The AI app needs the editor or admin role in that room or in its studio. Ask a studio admin for the editor role under **Settings** > **Access**.

### The AI App Says It Cannot See Triggers Or Schedules

DataSights shows triggers, schedules and scheduled emails only to people with the editor or admin role. For triggers, that role is needed in the room or its studio; for schedules, in the studio or one of its rooms. Ask a studio admin for the editor role under **Settings** > **Access**.

### The AI App Says A Trigger Or Scheduled Email Changed After The Change Was Prepared

Someone saved it after the AI app prepared its change, so nothing was changed. Ask the app to prepare the change again against how it is now.

### The AI App Says It Is Working In A Different Room

The AI app only changes things in its working room. You asked for a change in another room, so nothing was prepared. If you want to work in that room, tell the app to switch. It confirms the new room, then prepares the change again.

### The AI App Says No Room Is Selected

You had not opened a room in DataSights when you connected the app, or that room was deleted, or you no longer have access to it. Tell the app which room to work in.

### The AI App Says The Change Was Prepared For Another Room

The app switched rooms after it prepared the change, so nothing was changed. Ask it to prepare the change again in the room it now works in, or to switch back first.

### The AI App Says A Board Change Stopped Part-Way

One of the steps that build the board failed, for example because a widget was deleted in the meantime. The app lists the steps that were done and the one that failed. Ask it to prepare the change again against the board as it is now.

### The AI App Says The Confirmation Expired Or Was Already Used

A prepared change can be saved once, within 10 minutes of being prepared. Ask the AI app to prepare the change again, check it, and agree within 10 minutes.

### The AI App Says The Model Changed After The Change Was Prepared

Someone saved the model after the AI app prepared its change, so nothing was changed. Ask the app to prepare the change again. It then shows the change against the current model.

## Related Routes And Terms

- Routes: `/connect/ai`, `/account`, `/settings`
- Terms: connected AI apps, connect Claude, connect ChatGPT, connect Cursor, MCP, connector, consent, Allow, disconnect, working room, switch room, which room, models, SQL, preview, version history, confirm a change, widgets, boards, dashboard, trigger, alert, KPI alert, schedule, scheduled email, scheduled board, recipients, run history
