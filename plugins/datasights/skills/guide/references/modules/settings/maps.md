# Settings: Maps

Audience: End users
Last reviewed: 2026-04-22
Routes: `/settings`
Settings tab: **Maps**
Generation target: Settings Maps training material

## Purpose

The **Maps** tab lets studio admins configure the Google Maps API key, optional Map ID, and default map location used by map widgets in a studio.

## Prerequisites

- You must open **Settings**.
- You must be an admin for at least one studio.
- The **Maps** tab only appears when you administer at least one studio.
- A studio must be selected before maps settings can be saved.

## Main User Workflows

### Select A Studio

1. Open **Settings**.
2. Select **Maps**.
3. Under **Studio**, choose the studio to configure.

The studio dropdown shows **Loading studios...**, **No studios**, or **Select studio** depending on the current state.

### Add Or Replace The Google Maps API Key

1. Open **Settings**.
2. Select **Maps**.
3. Choose a **Studio**.
4. In **Google Maps API Key**, enter the key.
5. Click **Save**.

When a key is already stored, the field uses the saved-key placeholder. Entering a new key replaces the stored key when you save.

### Clear The Stored Google Maps API Key

1. Open **Settings**.
2. Select **Maps**.
3. Choose a **Studio**.
4. Click **Clear stored key**.
5. In **Delete Google Maps API Key**, click **Delete**.

The tab shows **Google Maps API key deleted successfully.** after a successful delete.

### Set A Map ID

1. Open **Settings**.
2. Select **Maps**.
3. Choose a **Studio**.
4. Enter a value in **Map ID**.
5. Click **Save**.

The **Map ID** field is optional and must be 255 characters or fewer.

### Choose The Default Map Location

1. Open **Settings**.
2. Select **Maps**.
3. Choose a **Studio**.
4. Under **Default map location**, choose a city.
5. Click **Save**.

Available locations are **Johannesburg**, **Cape Town**, **London**, **New York**, **Copenhagen**, **Dubai**, **Singapore**, and **Sydney**.

## Feature Reference

- **Studio** selects which studio's maps settings are being edited.
- **Google Maps API Key** stores the key used for map rendering.
- **Not configured** appears when no key is stored.
- **Clear stored key** deletes the stored Google Maps API key after confirmation.
- **Map ID** stores an optional Google Map ID.
- **Default map location** sets the fallback map center for map workflows.
- Successful saves show **Maps settings saved.**
- Failed saves show **Maps settings were not saved.**

## Permissions And Availability

- Only studio admins can see and use the **Maps** tab.
- **Save** is disabled until a studio is selected and while settings are loading or saving.
- Map widgets may fail to render maps if the studio does not have valid Maps settings.

## Troubleshooting

### Maps Is Missing

You are not an admin for any studio.

### Not Configured Appears

No Google Maps API key is stored for the selected studio.

### Save Fails Because The Map ID Is Too Long

Shorten **Map ID** to 255 characters or fewer.

### Map Widgets Do Not Render Maps

Confirm that the selected studio has a valid **Google Maps API Key** and that the relevant widget is in a room belonging to that studio.

## Related Routes And Terms

- Route: `/settings`
- Tab: **Maps**
- Related terms: maps, Google Maps API key, Map ID, default map location, basemap, map widgets
