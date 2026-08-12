# Settings: Users

Audience: End users
Last reviewed: 2026-06-13
Routes: `/settings`
Settings tab: **Users**
Generation target: Settings Users training material

## Purpose

The **Users** tab lets a studio admin invite users, update user profile details, resend pending invitations, change passwords, and delete users from the application.

## Prerequisites

- You must open **Settings**.
- You must be an admin for the active studio.
- A studio and room must exist before a new user can be assigned.
- The **Add User** button is disabled when there is no active studio or you cannot manage users.

## Main User Workflows

### Find A User

1. Open **Settings**.
2. Select **Users**.
3. Type in **Search users**.
4. Review the table columns **Name**, **Email**, and **Created At**.

If there are no users, the table shows **No users yet.** If there is no active studio, it shows **No studio available.**

### Add A User

1. Open **Settings**.
2. Select **Users**.
3. Click **Add User**.
4. In **Add user**, optionally upload an image.
5. Enter **Name**.
6. Enter **Surname**.
7. Enter **Email**.
8. Select **Studio**.
9. Select **Room**.
10. Select **Role**: **Admin**, **Editor**, or **Viewer**.
11. Enter the **Mobile number** when needed (see **Mobile Number** below).
12. Click **Send invite**.

The new user is assigned to the selected studio and room. If the email already exists, the app uses the existing account and updates access where allowed.

### Edit A User

1. Open **Settings**.
2. Select **Users**.
3. Find the user row.
4. Click the edit icon.
5. In **Edit user**, update **Name**, **Surname**, **Mobile number**, or the profile image.
6. Click **Save**.

The **Email** field is shown for reference and cannot be edited from this modal.

### Change A User Password

1. Open the user's **Edit user** modal.
2. Click **Change password**.
3. Enter **New password**.
4. Enter **Confirm password**.
5. Click **Save**.

The password must include at least one lowercase letter, one uppercase letter, one number, and one special character.

### Resend An Invite

1. Open **Settings**.
2. Select **Users**.
3. Find a user who has not activated their account.
4. Click the resend invite action.
5. In **Resend Invite?**, click **Resend invite**.

The resend action is only available for users who have not activated their account.

### Delete A User

1. Open **Settings**.
2. Select **Users**.
3. Find the user row.
4. Click the delete icon.
5. In **Delete user?**, confirm the delete action.

The confirmation explains that deleting the user removes the user and their access.

### Mobile Number

The **Mobile number** field has two parts: a **country selector** (flag and dialing code, for example 🇿🇦 +27) and the number itself.

1. The country defaults to your browser's region; change it by clicking the flag and searching by country name or dialing code.
2. Type the local number next to it — you do not need to add the country code yourself.

You can enter the number however is natural and it is corrected automatically before saving:

- A leading **0** (for example `082 123 4567`) is replaced by the selected country's code.
- A number without the **0** (for example `82 123 4567`) has the country code added.
- If you include the country code (for example `+27 82 123 4567` or `27 82 123 4567`), it is not duplicated.
- Pasting a full international number (for example `+1 415 555 2671`) switches the flag to match.

The saved number is always stored in full international format (for example `+27821234567`). If the number is not valid for the chosen country, the field shows **Phone number must be valid.** and the form cannot be saved until it is fixed. The mobile number is optional and may be left blank.

## Feature Reference

- **Search users** filters the user table.
- **Add User** opens the invitation workflow.
- **Edit user** updates profile details and optional password changes.
- **Send invite** creates or updates the user assignment and sends the invitation.
- **Resend invite** is available only for users who are not activated.
- **View only** appears when the row cannot be managed with your current access.
- **Not in studio** appears for a listed user who is not a member of the selected studio.

## Permissions And Availability

- Only studio admins can manage users in the active studio.
- Non-admin users cannot create, edit, resend, or delete users from this tab.
- A room must be selected when adding a user so the user has a valid room assignment.

## Troubleshooting

### Add User Is Disabled

Confirm that a studio is active and that your role for the studio is **Admin**.

### No Studio Available Appears

Select or create a studio before adding users.

### The Invite Cannot Be Resent

The user has probably already activated their account. Resend is only for pending invitations.

### The Mobile Number Is Rejected

Check that the country selector matches the number and that the number has the right length for that country. The field shows **Phone number must be valid.** until the number is correct. Leave it blank if you do not want to store a number.

## Related Routes And Terms

- Route: `/settings`
- Tab: **Users**
- Related terms: add user, invite user, edit user, resend invite, delete user, password, role, admin, editor, viewer, mobile number, phone number, country code, dialing code
