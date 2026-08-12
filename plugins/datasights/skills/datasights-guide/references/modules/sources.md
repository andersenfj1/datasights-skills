# Sources

Audience: End users
Last reviewed: 2026-04-22
Routes: `/sources`

## Purpose

Sources define database connections used by models. The Sources module lets editors add and maintain SQL sources, test connections, and import CSV files into source tables.

## Prerequisites

- You must be signed in and have an active room selected.
- You need edit access in the active room or studio to add, edit, delete, or import sources.
- MySQL and PostgreSQL are available when adding a source. Microsoft SQL Server can appear for existing source records but is not currently offered in the add-source selector.
- CSV import requires an existing SQL source.

## Main User Workflows

### Find A Source

1. Open **Sources** from the app navigation.
2. Type in **Search sources** when needed.
3. Review the table columns **Name**, **Type**, **IP**, and **Actions**.

If no room is selected, the page shows **Select a room from the user menu to view sources.** If there are no sources, it shows **No sources found.**

### Add A Source

1. Open **Sources**.
2. Click **Add**.
3. Choose **MySQL** or **PostgreSQL** from the source type rail.
4. Complete required fields:
   - **Name**
   - **Host**
   - **Port**
   - **Database**
   - **Username**
   - **Password**, when required by the database
5. Click **Test Connection**.
6. Confirm **Connection successful.**
7. Click **Save**.

### Edit A Source

1. Open **Sources**.
2. Find the source row.
3. Click the edit icon.
4. In **Edit Source**, update the connection fields.
5. If a password already exists, the modal shows that the existing password is hidden. Enter a new password only when you want to replace it.
6. Studio administrators can switch on **Allow data-grid writeback** to let editable data-grid widgets save changes back to this source. See [Allow data-grid writeback](#allow-data-grid-writeback).
7. Click **Test Connection** when needed.
8. Click **Save**.

#### Allow Data-grid Writeback

By default a source is read-only and data-grid widgets cannot change its data. A studio administrator can enable writeback per source:

1. Open **Sources** and edit the source (writeback supports MySQL and PostgreSQL sources).
2. Switch on **Allow data-grid writeback**.
3. Click **Save**.

Once enabled, editors can turn on editing for individual data-grid widgets built on models that use this source. The grid then lets people change cells, add rows, and delete rows, with changes saved on demand. For safety, prefer a least-privilege database account for the source — writeback uses the same stored credentials, so the account's own grants are the final limit on what can be changed. The toggle is visible to administrators only.

### Delete A Source

1. Open **Sources**.
2. Find the source row.
3. Click the delete icon.
4. In **Delete source?**, confirm **Delete**.

Deleting a source can affect models and widgets that depend on it.

### Import A CSV Into A New Table

1. Open **Sources**.
2. Click **Import CSV**.
3. Choose **Source**.
4. Choose **Create New Table** under **Mode**.
5. Click **Choose File** and select a `.csv` file.
6. Review **CSV Preview**.
7. Confirm or edit **New Table Name**.
8. Click **Import CSV**.
9. Wait for the progress message to complete.

New table names can use letters, numbers, and underscores only.

### Append A CSV To An Existing Table

1. Open **Sources**.
2. Click **Import CSV**.
3. Choose **Source**.
4. Choose **Append Existing** under **Mode**.
5. Click **Choose File** and select a `.csv` file.
6. Choose **Existing Table**.
7. Review **CSV Preview**.
8. Click **Import CSV**.
9. If the CSV columns do not match the selected table, review the warning and choose **Yes, continue** or **No, cancel**.

When continuing after a column mismatch, matching columns are inserted, missing table columns become `NULL`, and extra CSV columns are ignored.

## Feature Reference

- **Search sources** filters the source list.
- **Add** opens the database connection modal.
- **Import CSV** creates or appends source data from a CSV file.
- **Test Connection** validates the connection details before saving.
- **Type** shows the source database type.
- **IP** shows the stored source host/IP.
- **CSV Preview** shows detected columns and sample rows before import.
- **Create New Table** imports CSV data into a new table.
- **Append Existing** imports CSV rows into an existing table.
- CSV import progress reports inserted and parsed rows while the job runs.
- **Allow data-grid writeback** (administrators only) lets editable data-grid widgets save changes back to this MySQL/PostgreSQL source.

## Permissions And Availability

- View-only users can see sources but cannot add, edit, delete, or import.
- **Add** and **Import CSV** are disabled until a room/studio context is active.
- CSV import is available for SQL sources only.
- Password values are hidden after saving and must be re-entered only when replacing them.
- **Allow data-grid writeback** is shown and editable to studio administrators only; editors with edit access do not see it.

## Troubleshooting

### Add Or Import CSV Is Missing

You likely do not have edit access in the active room or studio.

### Test Connection Fails

Check **Host**, **Port**, **Database**, **Username**, and **Password**. Confirm that the application environment can reach the database.

### CSV File Is Rejected

Use a `.csv` file with a valid header row and consistent column counts.

### New Table Name Is Rejected

Use a name that starts with a letter or underscore and contains only letters, numbers, and underscores.

### Allow Data-grid Writeback Is Not Shown

The toggle is available to studio administrators only, and only for MySQL and PostgreSQL sources. Ask a studio administrator if you need a source enabled for grid editing.

## Related Routes And Terms

- Route: `/sources`
- Related terms: source, database connection, MySQL, PostgreSQL, Microsoft SQL Server, CSV import, test connection, source tables, writeback, data-grid editing
