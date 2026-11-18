# 11 - Admin

**Who:** Admin, Superadmin

---

## Purpose

Manage who can use Data DRB, and maintain the scope list: approve access requests, invite members, change roles, remove access, edit a scope description, or delete a scope. Admins and Superadmins also have every Editor capability.

## Who

**Admin** and **Superadmin** only. Route: `/admin/users`. Editors and Viewers do not see this page. The navbar scope picker is hidden here. Create and rename still happen from the picker on Monitor, Chart, Table, and History.

## When to use

- A coworker signed in and submitted an access request ([01](01-getting-started.md))
- Onboarding someone who already has a FlowScope account (invite by email)
- Promoting Viewer to Editor, or granting Admin
- Offboarding (remove membership)
- Editing a scope description, or deleting a scope that should no longer exist
- An empty workspace with no members yet (Superadmin bootstrap)

---

## Step by step

### Approve or reject an access request

1. Open `/admin/users`.
2. Find **Pending access requests** (requester email, or user id when email is missing). The list refreshes about every **30 seconds** while the page is open.
3. **Approve** — choose **viewer**, **editor**, or **admin**, then confirm. The user can open Monitor, Chart, and Table on the next load.
4. **Reject** — confirm. The user may submit a new request later.

<p class="shot-pending">Screenshot pending — assets/admin-pending-requests.png: Pending access requests</p>

### Invite a member

1. On `/admin/users`, use **Invite**.
2. Enter the person’s **email**. They need an auth account already, usually from having signed in once.
3. Choose **viewer**, **editor**, or **admin**.
4. Submit. Invite cannot assign **Superadmin**.

<p class="shot-pending">Screenshot pending — assets/admin-users.png: Members list and invite</p>

### Change role or remove a member

1. Find the membership row.
2. **Change role** to viewer, editor, or admin.
3. **Remove** deletes workspace membership. They keep their login and lose graph and Monitor access until invited or approved again.
4. The UI blocks removing or demoting **yourself** in a way that would leave the workspace with no admin. Keep at least one Admin or Superadmin.

### Scopes

1. On the same page, open the scopes list. Each row shows name, slug, and how many nodes are tagged.
2. **Save description** stores a short note (optional, trimmed). The description does not appear in the navbar picker.
3. **Delete** asks you to confirm and shows the name and tagged count. Nodes stay on the graph. Their membership in that scope is removed. If someone’s browser or a shared link still points at the deleted slug, the next load falls back to **All**.
4. To add a scope or rename one, use the navbar **Scope** picker on a workspace view. The slug is set from the name at create time and does not change later. A new scope starts empty and becomes the selected lens.

<p class="shot-pending">Screenshot pending — assets/admin-scopes.png: Scope list with description and delete</p>

### First-time superadmin bootstrap

1. If the workspace has **no members**, an eligible signed-in user may open `/admin/users` and **claim Superadmin**.
2. Use this for initial setup. After a Superadmin exists, use invite and approve for everyone else.
3. Invite does not create another Superadmin.

## Form / field standards

| Field | Standard |
|-------|----------|
| Invite email | The exact auth email. Invite fails when the user is not found. |
| Approve / invite role | `viewer` (read), `editor` (edit and job-author work), `admin` (members and scopes) |
| Superadmin | Bootstrap only |
| Scope description | Optional, short. It is not the picker label. |

Role meanings: [02 - Roles and permissions](02-roles-and-permissions.md).

## Edge cases

- **Invite email not found** — ask the person to sign in once, then invite again.
- **Last admin** — hand off before you remove the only Admin or Superadmin.
- Rejected users can request again. Check the pending list on a cadence your team agrees.
- Deleting a scope does not delete nodes. It only removes that tag.

## Related

- [01 - Getting started](01-getting-started.md)
- [02 - Roles and permissions](02-roles-and-permissions.md)
- [04 - Chart and Table](04-chart-and-table.md)
