# Models

Audience: End users
Last reviewed: 2026-06-12
Routes: `/models`, `/models/:modelId`

## Purpose

Models are saved SQL query definitions that sit between sources and widgets. A model connects to a selected source, stores the SQL query, can be previewed with **Run**, and can then be used by widgets and board filters.

## Prerequisites

- You must be signed in and have an active room selected.
- You need edit access in the active room or studio to add, rename, delete, or save models.
- A source should exist before a model can be fully configured.
- The AI Assistant panel in the model editor is available only to users with admin access for the model's studio.

## Main User Workflows

### Find A Model

1. Open **Models** from the app navigation.
2. Type in **Search models** when needed.
3. Review the table columns **Name**, **Source Type**, **Created At**, **Updated At**, and **Actions**.
4. Click a model name to open the editor.

If no room is selected, the page shows **Select a room from the user menu to view models.** If there are no models, it shows **No models found.**

### Add A Model

1. Open **Models**.
2. Click **Add**.
3. In **Add Model**, enter **Name**.
4. Click **Save**.
5. Open the new model from the list.
6. Choose a **Source**.
7. Write the SQL in **Query**.
8. Click **Run** to preview the data.
9. Click **Save**.

Names are required and must be 255 characters or fewer.

### Edit A Model Query

1. Open **Models**.
2. Click the model name.
3. Choose or change **Source**.
4. Use the table list on the left to inspect tables and columns.
5. Click **View ERD** when a source is selected and you want to inspect relationships visually.
6. Edit **Query**.
7. Click **Run**.
8. Review **Data**.
9. Click **Save**.
10. If the SQL query changed, enter a **Commit message** and confirm **Save**.

Only SELECT-style preview queries are allowed. If the query fails, the editor shows the query error in the **Data** area.

### Rename A Model From The List

1. Open **Models**.
2. Find the model row.
3. Click the edit icon.
4. In **Edit name**, enter the new **Name**.
5. Click **Save**.

### Rename A Model From The Editor

1. Open the model.
2. Click **Save As**.
3. In **Save as**, enter **Name**.
4. Click **Save**.

In the current editor this changes the model name and keeps the model open.

### Run And Inspect Data

1. Open a model.
2. Select **Source**.
3. Enter a SQL query in **Query**.
4. Click **Run**.
5. Expand **Data** if needed.
6. Use the column sort controls to sort ascending or descending.
7. Drag column edges to resize columns when needed.

If no rows are returned, the data area shows **No rows returned.**

### Use Version History

1. Open a model.
2. Expand **Version history**.
3. Review **Commit**, **Author**, **Saved**, and **Actions**.
4. Click **View diff** to compare a saved version with the current working copy.
5. Click **Restore** to load that version into the draft.
6. Click **Save** to make the restored draft the latest version.

If there are no saved SQL or source versions, the section shows **No saved SQL or source versions yet.**

### Use The AI Assistant

1. Open a model.
2. Select a **Source** and run a query if you want to provide a data preview.
3. Use the assistant panel to request help with SQL.
4. Apply the returned query when appropriate.
5. Click **Run** and then **Save**.

Assistant use requires admin access and the studio's AI Assistant settings must be configured.

The assistant can also answer how-to questions about the Models module ("How do I use version history?"); it answers from this documentation. Assistant chats are private to your account.

### Delete A Model

1. Open **Models**.
2. Find the model row.
3. Click the delete icon.
4. In **Delete model?**, confirm **Delete**.

## Feature Reference

- **Source** selects the database source used by the model.
- **View ERD** opens a visual source relationship view for the selected source.
- The source table list shows available tables and column types.
- **Query** is the SQL editor.
- **Run** previews results without saving.
- **Save** stores changes to the source, query, or name.
- **Commit message** is required when the SQL query changes.
- **Version history** tracks saved SQL/source versions.
- **View diff** compares a saved version with the current working copy.
- **Restore** loads a saved version into the draft.
- **Save As** opens the model name modal.
- Writeback eligibility for a model (used by editable Data grids and Form widgets) is resolved from driver metadata for the model's live query, including the insertable columns a Form widget can map to.

## Permissions And Availability

- View-only users can open models but cannot save, rename, delete, restore versions, or edit SQL.
- The model list shows **View only** instead of edit/delete actions when you cannot edit.
- Sources and SQL editing are disabled when you do not have edit access.
- The AI Assistant panel is usable only for admin users when assistant settings are available.
- Models require a selected source before queries can be run.

## Troubleshooting

### Add Is Disabled

Select a room from the user menu and confirm that you have edit access.

### No Sources Appears

Create a source in **Sources** before configuring the model.

### Run Shows Query Failed Or Only SELECT Allowed

Check that the SQL is a read-only SELECT query and that the selected source is reachable.

### Save As Did Not Create A Separate Copy

In this editor, **Save As** is used to save the model under a new name. Create a new model first if you need a separate model record.

## Related Routes And Terms

- Routes: `/models`, `/models/:modelId`
- Related terms: model, SQL model, source, query, run query, version history, commit message, ERD, AI Assistant
