# 02 - Roles and permissions

**Who:** Everyone

---

## Purpose

Know what your role can do so you do not look for Edit, History, Acknowledge, or Admin when those controls are hidden on purpose.

## Who

Everyone. Admins assign roles. Editors and Viewers should know their limits.

Scope is not a role. Every member can pick any scope in the navbar. Tagging nodes and managing the scope list follow the matrix below.

## When to use

- After you get access
- When a button is missing
- Before asking an Admin to upgrade your role

## Capability matrix

Privilege increases down the table. **Editor = job author** (lineage edits and CML Telegram monitoring).

| Capability | Viewer | Editor | Admin | Superadmin |
|------------|:------:|:------:|:-----:|:----------:|
| Read Monitor / Chart / Table | Yes | Yes | Yes | Yes |
| Pick a scope lens | Yes | Yes | Yes | Yes |
| Edit nodes, Apply, Save draft, bulk scope tags | No | Yes | Yes | Yes |
| History (list / restore) | No | Yes | Yes | Yes |
| Acknowledge missed / stuck runs | No | Yes | Yes | Yes |
| Execution detail **Telegram** tab | No | Yes | Yes | Yes |
| Create or rename a scope from the picker | No | No | Yes | Yes |
| `/admin/users` (members and scope description / delete) | No | No | Yes | Yes |

Monitor, Chart, and Table are available to every **member**. History, Edit, acknowledge, and the Telegram tab are Editor+. Admin UI is Admin and Superadmin only.

## Superadmin vs Admin

- **Admin** and **Superadmin** both manage members and scopes at `/admin/users`.
- Invite and approve can assign **viewer**, **editor**, or **admin**. They cannot assign **Superadmin**.
- **Superadmin bootstrap** is only for an empty workspace (no members yet). Details: [11 - Admin](11-admin.md).

## What Viewers see on Monitor detail

Viewers can open execution and schedule-run sidebars, read Overview and Events, and use **Open lineage**, **Open job page**, **View on Timeline**, **View on Table**, and **Open runtime** when an engine id is present.

Viewers do not get acknowledge forms or the Telegram tab.

On Chart and Table, viewers get the detail sidebar, including a **View only** badge, and no **Edit** button. They still see the Scope column. They do not see row checkboxes for bulk tags.

## Step by step

1. After login, open the account menu and read your role.
2. Check whether **History** is in the view switcher, and whether Chart or Table detail shows **Edit**.
3. Read-only detail and no History means you are a **Viewer**.
4. Ask an Admin for **Editor** when you need edit, acknowledge, or History.
5. Ask for **Admin** when you need to invite coworkers or edit the scope list.

## Screenshot

The account menu shows the role label. See [Getting started](01-getting-started.md).

## Edge cases

- **Signed in but not a member** is the access-request flow, not the Viewer role ([01](01-getting-started.md)).
- You cannot demote or remove yourself in a way that drops the last admin. See [11](11-admin.md).

## Related

- [01 - Getting started](01-getting-started.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [10 - Acknowledge](10-editor-acknowledge.md)
- [11 - Admin](11-admin.md)
