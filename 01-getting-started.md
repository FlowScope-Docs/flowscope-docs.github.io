# 01 - Getting started

**Who:** Everyone

---

## Purpose

Get into FlowScope, request access if you need it, and learn the workspace chrome so you can switch views.

## Who

Anyone with a FlowScope login. Membership is separate from sign-in. You must be a **workspace member** before Monitor, Chart, and Table load data.

## When to use

- First time on the site
- After your password or GitHub account changes
- When you see the access request panel instead of Monitor

## Step by step

### Sign in

1. Open the FlowScope URL your team uses.
2. Sign in with **email and password**, or **GitHub**.
3. If login succeeds and you are a member, you land in the workspace. **Monitor** is the default view. The shell can appear before the graph finishes loading.

![Sign in with GitHub or email and password](./assets/login.png)

### Request access (non-members)

1. Sign in successfully.
2. If you are not a workspace member, you see the **access request** panel.
3. Submit a request. Your session email is stored for Admin review.
4. Wait for an Admin or Superadmin to approve or reject ([11 - Admin](11-admin.md)).
5. If rejected, you can submit a new request.

While the request is pending you see a timestamp and you cannot open Monitor, Chart, or Table.

<p class="shot-pending">Screenshot pending — assets/access-request.png: Access request panel for a signed-in non-member</p>

### Orient in the workspace

1. The navbar starts with **FlowScope**, then the **Scope** picker, then the view pills: Monitor, Chart, Table, and (Editor+) History.
2. **Scope** is a lens, not a role. **All** is the full Data DRB graph. A named scope shows nodes tagged with it, plus everything data-lineage upstream of those nodes. The choice is remembered in this browser. `?scope=` shares it. Admin and Superadmin can create and rename scopes from this menu. Description and delete live on [Admin](11-admin.md).
3. The top-right **account menu** shows your email, role, **Documentation** (this guidebook), and **Log out**.
4. The controls beside the view are icon-only: Refresh, and for editors Save, Revert, and Fullscreen. The **Last refresh** chip shows a relative time, or **Refreshing…** while a reload is in flight. Monitor also has **Auto** (Off / 5s / 10s / 30s / 60s). Chart and Table do not poll on a timer.
5. On Chart and Table, **Fullscreen** hides the top chrome. On Chart it also hides both sidebars until you exit.
6. Scrolling the page down hides the top chrome visually and brings it back when you scroll up. The timeline’s own scroll does not do this.
7. Drag sidebar edges to resize. Widths are remembered in this browser. Double-click a handle to reset the width where that gesture exists.
8. Admins open membership and scopes at **`/admin/users`**. That route is not a workspace view pill. The scope picker is hidden on the Admin page.

<p class="shot-pending">Screenshot pending — assets/workspace-chrome.png: Scope picker, view pills, Last refresh, and account menu</p>

<p class="shot-pending">Screenshot pending — assets/scope-picker-menu.png: Scope menu with All and named scopes</p>

<p class="shot-pending">Screenshot pending — assets/account-menu.png: Account menu with Documentation and Log out</p>

## Form / field standards

| Action | What to provide |
|--------|-----------------|
| Access request | Submit only. The email comes from your signed-in session. |

## Edge cases

- **Signed in, no membership** → access request panel, not an empty Monitor.
- **No members in the workspace yet** → an eligible user may see a path to `/admin/users` to claim the first **Superadmin**. See [11 - Admin](11-admin.md).
- **History missing from the switcher** → your role is Viewer ([02](02-roles-and-permissions.md)).
- **A named scope looks empty** → nothing is tagged into it yet. The empty state links back to **All**. Upstream nodes appear once a downstream seed is tagged.

## Related

- [02 - Roles and permissions](02-roles-and-permissions.md)
- [03 - Monitor](03-monitor.md)
- [04 - Chart and Table](04-chart-and-table.md)
- [11 - Admin](11-admin.md)
- [13 - FAQ](13-faq-and-troubleshooting.md)
