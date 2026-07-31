# 02 - Roles and permissions

**Who:** Everyone

---

## Purpose

Know what your role can do in FlowScope so you do not look for Edit, History, Acknowledge, or Admin controls that are hidden on purpose.

## Who

Everyone. Admins assign roles; Editors and Viewers should know their limits.

## When to use

- After you get access
- When a button or tab is missing
- Before asking an Admin to upgrade your role

## Capability matrix

Privilege increases down the table. **Editor = job author** (lineage edits and CML Telegram monitoring).

| Capability | Viewer | Editor | Admin | Owner |
|------------|:------:|:------:|:-----:|:-----:|
| Read Monitor / Chart / Table | Yes | Yes | Yes | Yes |
| Edit nodes/edges, Save draft | No | Yes | Yes | Yes |
| History (list / restore) | No | Yes | Yes | Yes |
| Acknowledge missed / stuck runs | No | Yes | Yes | Yes |
| Execution detail **Telegram** tab | No | Yes | Yes | Yes |
| `/admin/users` | No | No | Yes | Yes |

Monitor, Chart, and Table are available to every **member**. History, Edit, acknowledge, and the Telegram tab are Editor+. Admin UI is Admin/Owner only.

## Owner vs Admin

- **Admin** and **Owner** both manage members at `/admin/users`.
- Invite / approve can assign **viewer**, **editor**, or **admin**  -  not **Owner**.
- **Owner bootstrap** is only for the empty-workspace case (no members yet). Details: [11 - Admin](11-admin.md).

## What Viewers see on Monitor detail

Viewers can open execution and schedule-run sidebars, read Overview and Events, use **Open lineage** / **View on Timeline**, and open **Open runtime** when Engine ID is present.

Viewers do **not** get acknowledge forms or the Telegram tab. Those are Editor+.

## Step by step

1. After login, check whether **History** appears in the view switcher and whether Chart/Table show an **Edit** tab.
2. If you only see read-only Detail → you are likely a **Viewer**.
3. If you need edit, acknowledge, or History → ask an Admin to grant **Editor** (or higher).
4. If you need to invite coworkers → you need **Admin** or **Owner**.

## Form / field standards

N/A

## Screenshot

No dedicated screenshot. Role differences show up as missing History / Edit / Admin / Acknowledge UI.

## Edge cases

- **Signed in but not a member** → access request flow. That is not the Viewer role ([01](01-getting-started.md)).
- **Cannot demote/remove yourself** in a way that removes the last admin access  -  see Admin self-protection in [11](11-admin.md).

## Related

- [01 - Getting started](01-getting-started.md)
- [09 - Job monitoring](09-editor-job-monitoring.md) (Editor job-author duties)
- [10 - Acknowledge](10-editor-acknowledge.md)
- [11 - Admin](11-admin.md)

