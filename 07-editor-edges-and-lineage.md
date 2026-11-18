# 07 - Edges and lineage

**Who:** Editor, Admin, Superadmin

---

## Purpose

Edges describe how nodes relate. Data edges answer what feeds what. **`job_dependency`** is only the schedule **Depends on** link. Data lineage does not mean “runs after”.

## Who

Editors, Admins, and Superadmins.

## When to use

| Goal | Relation |
|------|----------|
| A job reads a table | `feeds` |
| A job writes a table | `produces` |
| A job writes a Google Sheet or similar sink | `exports_to` |
| Job B runs on job A’s schedule | Refresh **dependent** plus Depends on → `job_dependency` |

There is no separate edge screen. You set these from the node form. Save writes them with the rest of the draft.

---

## Step by step

### Add data lineage

1. Edit a job (or create one) and open **Lineage**.
2. Use **Source** and **Target**. Search by name, narrow with the type chips, and remove a chip to drop that node.
3. Default chips follow the node you are editing. Jobs start aimed at **table** sources and targets. Widen the chips when the node you need is hidden.
4. On create, the connections you pick become `feeds` and `produces` edges when the draft is saved.
5. **Apply**, then **Save draft**.

### Set a dependent job

1. Set refresh frequency to **dependent** ([08](08-editor-schedule-standards.md)).
2. In **Schedule**, use **Depends on**. There is no Schedule or Cron block in this mode.
3. Pick the prerequisite job. Save persists that as `job_dependency`.
4. Do not encode the same “runs after” idea with only a data edge. Monitor row order uses the explicit Depends-on edge. The timeline still draws the dependent’s real runs only, not a calculated expected bar.
5. **Apply**, then **Save draft**.

<p class="shot-pending">Screenshot pending — assets/node-form-depends-on.png: Depends on picker in the job form</p>

### Change or remove a relationship

1. Open the node form and change **Source**, **Target**, or **Depends on**. Clearing the picks clears those edges on save.
2. Relations you will see: Feeds, Produces, Exports to, Job dependency.
3. **Apply**, then **Save draft**.
4. If the detail header shows graph warnings, a reference is broken. Fix or remove it before you treat the graph as clean.

Moving frequency off **dependent** clears `job_dependency` on save. Data edges stay.

### Read lineage on Chart and in the sidebar

1. Select a node. Data highlighting shows upstream and downstream impact. **Focus** keeps the full upstream cone and one hop downstream ([04](04-chart-and-table.md)).
2. Prerequisite and dependent jobs highlight on their own when a schedule dependency exists. `job_dependency` lines appear in that context, not as a permanent drawing of every schedule edge.
3. In the detail sidebar, **Overview → Data flow** lists direct source and target tables. Jobs list the tables configured on the form. Tables, Sheets, and Looker list tables one job away, with a **via job** line.
4. The **Lineage** tab lists job relationships only: produced-by and feeds jobs, or prerequisite and dependent jobs. It does not list every transitive node. Click a row to select it.

<p class="shot-pending">Screenshot pending — assets/chart-search-detail.png: Selection highlighting and the detail sidebar</p>

## Form / field standards

| Rule | Detail |
|------|--------|
| Data vs schedule | Feeds, produces, and exports-to are lineage. Run-after is dependent plus Depends on. |
| Clear dependent | A frequency other than dependent clears `job_dependency` on save |
| Detail panel | Depends on is under Schedule for dependent jobs. The Lineage tab is job relationships, not a second copy of the table list. |
| Pickers | Start from the default type chips. Clear them when the node you need is filtered out. |

## Edge cases

- Graph warnings after a delete mean something still points at the missing node. Fix that in the form.
- “This table feeds that job” and “job B runs after job A” are different edges.
- Dependent with no prerequisite is an incomplete schedule. Timeline order cannot use a Depends-on edge you never saved.
- Scope cones follow data edges only. A Depends-on neighbor is not pulled in just because you picked a scope.

## Related

- [06 - Node forms](06-editor-node-forms.md)
- [08 - Schedule standards](08-editor-schedule-standards.md)
- [04 - Chart and Table](04-chart-and-table.md)
- [03 - Monitor](03-monitor.md)
