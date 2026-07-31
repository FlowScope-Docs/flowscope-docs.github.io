# 11 - Admin

**Who:** Admin, Owner

---

## Purpose

Manage who can use the shared FlowScope workspace: approve access requests, invite members, change roles, and remove access. Admins and Owners also inherit every Editor capability (forms, Monitor acknowledge, job monitoring).

## Who

**Admin** and **Owner** only. Route: `/admin/users`. Editors and Viewers do not see this page.

## When to use

- A coworker signed in and submitted an access request ([01](01-getting-started.md))
- Onboarding someone who already has a FlowScope auth account (invite by email)
- Promoting Viewer → Editor, or granting Admin
- Offboarding (remove membership)
- Empty workspace with no members yet (Owner bootstrap)

---

## Step by step

### Approve or reject an access request

1. Open `/admin/users`.
2. Find **Pending access requests** (requester email; falls back to user id if needed). The list auto-refreshes about every **30 seconds** while the page is open.
3. **Approve**  -  choose role **viewer**, **editor**, or **admin**, then confirm. The user can open Monitor/Chart/Table on next load.
4. **Reject**  -  confirm. The user may submit a new request later.

<p class="shot-pending">Screenshot pending — assets/admin-pending-requests.png: Pending access requests</p>

### Invite a member

1. On `/admin/users`, use **Invite**.
2. Enter the person’s **email** (must already exist in auth  -  they typically need to have signed in or been provisioned first).
3. Choose role: **viewer**, **editor**, or **admin**.
4. Submit. You **cannot** assign **Owner** via invite.

<p class="shot-pending">Screenshot pending — assets/admin-users.png: Admin users list and invite</p>

### Change role or remove a member

1. In the members list, find the membership row.
2. **Change role** to viewer / editor / admin as needed.
3. **Remove** to delete workspace membership (they keep their login but lose graph/Monitor access until invited or approved again).
4. Self-protection: the UI/API blocks removing or demoting **yourself** in ways that would lock out admin access. Keep at least one Admin/Owner who can manage the workspace.

### First-time owner bootstrap

1. If the workspace has **no members**, an eligible signed-in user may see a path to `/admin/users` to **claim Owner** (bootstrap).
2. Use this only for initial setup. After Owner exists, use invite/approve for everyone else.
3. Do not expect invite to create a second Owner  -  Owner is special-cased.

## Form / field standards

| Field | Standard |
|-------|----------|
| Invite email | Exact auth email; invite fails if the user is not found |
| Approve / invite role | `viewer` (read), `editor` (edit + job author duties), `admin` (members) |
| Owner | Bootstrap only  -  not via invite |

Role meanings: [02 - Roles and permissions](02-roles-and-permissions.md).

## Edge cases

- **Invite email not found**  -  ask the person to sign in once (or get provisioned), then invite again.
- **Last admin**  -  do not remove the only Admin/Owner without a handoff.
- Rejected users can re-request; check pending list periodically.

## Related

- [01 - Getting started](01-getting-started.md)
- [02 - Roles and permissions](02-roles-and-permissions.md)

<div class="chapter-nav">
<a href="#/">← Home</a>
<a href="#/12-standards-and-use-cases">Next: Standards and use cases →</a>
</div>
