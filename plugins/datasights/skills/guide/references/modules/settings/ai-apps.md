# Settings: AI Apps

Audience: End users
Last reviewed: 2026-09-18
Routes: `/settings`
Settings tab: **AI apps**
Generation target: Settings AI apps training material

## Purpose

The **AI apps** tab lets administrators decide whether people can connect AI apps such as Claude, ChatGPT or Cursor to the workspace, and see and disconnect every connected AI app. For what users see when they connect, see [Connected AI apps](../connected-ai-apps.md).

## Prerequisites

- You must open **Settings**.
- You must be an admin for at least one studio.
- The **AI apps** tab only appears when you administer at least one studio.

## Main User Workflows

### Turn AI Apps Off Or On

1. Open **Settings**.
2. Select **AI apps**.
3. Click the **Allow AI apps to connect** switch.

The change is saved straight away. Turning it off stops all connected apps immediately and blocks new connections. Connections are kept, and work again when you turn the switch back on.

### Disconnect Someone's AI App

1. Open **Settings**.
2. Select **AI apps**.
3. Under **Connected AI apps**, find the app. Each row shows the app name, the person it belongs to, where it returns to, when it was connected and when it was last used.
4. Click **Disconnect**.
5. In the confirmation, click **Disconnect**.

The app stops working for that person straight away. They can connect it again from their AI app.

## Feature Reference

- **Allow AI apps to connect** is on by default for every workspace.
- **Connected AI apps** lists every active connection in the workspace.
- **No AI apps connected** appears when nobody has connected an AI app.
- **AI app connections are not configured on this server.** appears when the server has not been set up for AI apps.

## Permissions And Availability

- Only studio admins can see and use the **AI apps** tab.
- Anyone can disconnect their own AI apps under **Account** > **Connected AI apps**.
- AI apps themselves cannot open or change this setting.

## Troubleshooting

### AI Apps Is Missing

You are not an admin for any studio.

### AI App Connections Are Not Configured On This Server

The server administrator has not set up AI app connections. Ask whoever runs your deployment.

### An App Still Appears After Turning AI Apps Off

That is expected. Turning AI apps off pauses every connection without removing it. Disconnect the app to remove it.

## Related Routes And Terms

- Route: `/settings`
- Tab: **AI apps**
- Related terms: AI apps, connected apps, MCP, Claude, ChatGPT, Cursor, disconnect, allow AI apps
