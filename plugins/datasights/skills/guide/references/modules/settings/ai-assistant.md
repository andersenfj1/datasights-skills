# Settings: AI Assistant

Audience: End users
Last reviewed: 2026-04-22
Routes: `/settings`
Settings tab: **AI Assistant**
Generation target: Settings AI Assistant training material

## Purpose

The **AI Assistant** tab lets studio admins choose the assistant model, store or clear the assistant API key, and enable or disable the assistant for a studio.

## Prerequisites

- You must open **Settings**.
- You must be a studio admin.
- A studio must exist.
- At least one active assistant model must be available before the settings can be saved.

## Main User Workflows

### Select A Studio

1. Open **Settings**.
2. Select **AI Assistant**.
3. Under **Studio**, choose the studio to configure.

The studio dropdown shows **Loading studios...**, **No studios**, or **Select studio** depending on the current state.

### Choose An Assistant Model

1. Open **Settings**.
2. Select **AI Assistant**.
3. Choose a **Studio**.
4. Under **Model**, choose an active model.

If there are no active models, the field shows **No active models available.**

### Save Or Replace The API Key

1. Open **Settings**.
2. Select **AI Assistant**.
3. Choose a **Studio** and **Model**.
4. In **API Key**, enter the key.
5. Click **Save**.

When a key is already stored, the API key field uses the saved-key placeholder. Entering a new key replaces the stored key when you save.

### Clear The Stored API Key

1. Open **Settings**.
2. Select **AI Assistant**.
3. Choose the studio.
4. Click **Clear stored key**.
5. Confirm that **Key will be removed on save.** appears.
6. Click **Save**.

### Enable Or Disable The Assistant

1. Open **Settings**.
2. Select **AI Assistant**.
3. Choose the studio.
4. Use **Assistant Enabled** to switch between **Enabled** and **Disabled**.
5. Click **Save**.

## Feature Reference

- **Studio** selects which studio's assistant settings are being edited.
- **Model** selects the active assistant model.
- **API Key** stores the studio-level assistant key.
- **Clear stored key** marks the existing key for removal on the next save.
- **Assistant Enabled** controls whether assistant features are available for the studio.
- Successful saves show **AI Assistant settings saved.**
- Failed saves show **AI Assistant settings were not saved.**

## Permissions And Availability

- Only studio admins can configure AI Assistant settings.
- A model must be selected before saving.
- If no active assistant models are available, the assistant settings cannot be completed from this tab.
- Disabling the assistant can make assistant-dependent features unavailable in other modules.

## Troubleshooting

### Save Fails With Please Select A Model

Choose a value in **Model** before clicking **Save**.

### No Active Models Available Appears

An administrator needs to make at least one assistant model active before this tab can be configured.

### The Stored Key Was Not Removed

Click **Clear stored key**, confirm that **Key will be removed on save.** appears, then click **Save**.

## Related Routes And Terms

- Route: `/settings`
- Tab: **AI Assistant**
- Related terms: AI Assistant, assistant model, API key, clear stored key, enabled, disabled, summarise
