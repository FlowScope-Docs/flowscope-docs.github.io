# 07  -  Edges and lineage

**Who:** Editor, Admin, Owner

---

## Purpose

Edges describe **how nodes relate**. Use data/delivery edges for lineage impact analysis. Use **`job_dependency` only** for schedule “Depends on”  -  never assume data lineage means run-after.

## Who

Editors, Admins, Owners.

## When to use

| Goal | Relation |
|------|----------|
| Job reads/writes tables (data flow) | `feeds` / `produces` (and create-time connection helpers) |
| Job writes a Google Sheet (or similar export sink) | `exports_to` |
| Job B must wait on job A’s schedule | Refresh = **dependent** + Depends-on → `job_dependency` |

---

## Step by step

### Add data lineage edges

1. Edit a job (or create one) and open **Lineage**.
2. Use **Source** / **Target** pickers: search by name, filter with type chips, clear filters if needed, remove chips to deselect.
3. Default chips depend on the node you are editing (e.g. jobs default toward **table** sources/targets).
4. On create, selecting connections can auto-create `feeds` / `produces` edges.
5. Alternatively use **Edge** create/edit for an explicit relation and endpoints.
6. **Save draft**.

### Set a dependent job (schedule dependency)

1. Set refresh frequency to **dependent** ([08](08-editor-schedule-standards.md)).
2. In **Schedule**, use **Depends on** only (no Schedule/Cron cards in this mode).
3. Pick the prerequisite job. That persists as `job_dependency` on save.
4. Do **not** also try to express the same dependency only with data edges  -  Monitor timeline order and expected slots use **explicit** `job_dependency`.
5. **Save draft**.

### Edit or delete an edge

1. Open the edge in the editor (Edge form) or change Source/Target/Depends-on on the node form.
2. Relation types: `feeds`, `produces`, `exports_to`, `job_dependency`.
3. New edges get auto-generated ids.
4. Delete unused edges; **Save draft**.
5. Check graph warnings for orphans after deletes.

### Read lineage on Chart

1. Select a node  -  upstream/downstream data highlighting shows impact.
2. Prerequisite / dependent jobs highlight separately when schedule deps exist.
3. `job_dependency` edges show in context (selection/hover), not always as a permanent full overlay.

<p class="shot-pending">Screenshot pending — assets/node-form-depends-on.png: Depends-on picker (schedule dependency)</p>

<p class="shot-pending">Screenshot pending — assets/chart-search-detail.png: Chart highlighting for selected node lineage</p>

## Form / field standards

| Rule | Detail |
|------|--------|
| Data vs schedule | `feeds`/`produces`/… ≠ run-after. Run-after = `dependent` + Depends on |
| Clear dependent | Changing frequency away from `dependent` clears `job_dependency` on save |
| Detail panel | Depends on shown under Schedule for dependent jobs  -  not duplicated under Lineage |
| Picker filters | Start from defaults; widen chips if the node you need is hidden |

## Edge cases

- **Graph warnings** after bad or orphan edges  -  fix before save-and-forget.
- Confusing “this table feeds that job” with “job B runs after job A”  -  use the right edge type.
- Dependent with no prerequisite selected  -  incomplete schedule setup; timeline expecteds will be wrong.

## Related

- [06  -  Node forms](06-editor-node-forms.md)
- [08  -  Schedule standards](08-editor-schedule-standards.md)
- [03 - Monitor](03-monitor.md) (dependency row order)

