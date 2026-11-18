# 06 - Node forms

**Who:** Editor, Admin, Superadmin

---

## Purpose

Create, edit, and delete lineage nodes for all four types. A correct form keeps Chart and Table accurate and, for jobs, keeps Monitor mapped through the Livyatan `link`.

## Who

Editors, Admins, and Superadmins. **Apply** stages a local draft. Publish with [Save draft](05-editor-draft-and-history.md).

## When to use

- Register a table, job, Google Sheet, or Looker chart
- Fix owner, links, schedule, lineage, scope tags, or impact notes
- Remove a node that no longer exists

The detail sidebar does not contain the form. **Edit** opens a window over the page.

---

## Shared form rules

The window is one **tab per node**. The tab shows a type icon, the name (or “New {type}”), a dot when the tab is dirty, and **×** to close. A pinned **Add node** button on the tab strip starts another create tab without closing the ones you have. On a narrow window, extra tabs collapse into **+N**.

Header actions:

- **Apply** — stage this tab into the draft and close it
- **Close** — drop this tab. Confirm when it has unapplied edits
- **Delete** — edit mode only, with confirm. Then Apply or Save as the rest of the draft requires
- **Minimize** — the horizontal bar, Esc, or the backdrop. The tab is kept. Bottom-right **Forms · N** restores it. Esc does not discard.

**Ctrl+S** applies and keeps the tab. **Ctrl+Enter** applies and closes. **Alt+1** through **Alt+9** switch tabs.

On a wide window the body is three columns: a **section list**, the **fields**, and a live **Changes** panel (the same labels as Save draft review). A red dot on a section means a validation error. An accent dot means a field there differs from when the tab opened. Below 1280px, Changes opens from a **Changes (N)** button. Below 1024px, sections become **Jump to section**.

Sections, when the type uses them:

1. **Identity** — type, name, schema or placement, owner, scopes
2. **Links** — `link`, `link2` (labels vary by type)
3. **Resources** — CPU/RAM preset when the type has it
4. **Schedule** — refresh frequency, run times, or Depends on
5. **Lineage** — Source and Target
6. **Impact & notes** — business purpose, failure impact, notes

One field per row. Errors show under a field after you leave it, and again on Apply. Apply scrolls to the first invalid field and says how many to fix.

**Changes** lists old → new in edit mode, or filled fields as added in create mode, plus scope **+** / **−** chips. Apply still only stages the draft.

If someone else changed the node, the tab offers **Reload latest** or **Keep mine**. If the node was deleted, it offers **Create as new** or **Discard**.

<p class="shot-pending">Screenshot pending — assets/node-form-window.png: Form window with sections, fields, and Changes</p>

<p class="shot-pending">Screenshot pending — assets/forms-tray.png: Minimized Forms tray</p>

### `link` vs `link2`

| Field | Typical use |
|-------|-------------|
| **link** | Primary URL. For a CML job, the Livyatan job page ending in `/jobs/{id}` |
| **link2** | Secondary or mapping URL on job-like types |

A Livyatan URL must include `/jobs/{id}` or Monitor shows **No lineage link**. The form shows the parsed CML job id when the URL is valid. The map updates on **Save**, not on Apply alone.

### Scopes

**Scopes** sits in Identity, after Owner. A node can carry several tags. Create prefills the scope selected in the navbar. **All** starts with no tag. Chips publish with Save draft, together with any graph edits. The navbar still shows one scope or All. Tags are a lens, not a permission ([04](04-chart-and-table.md)).

---

## Step by step

### Create a node

1. On Chart or Table, use **+** or **Add node** and pick Table, Job, Google Sheet, or Looker.
2. Fill required Identity fields. Name is required (labeled **Chart name** on Looker and **Sheet Name** on Google Sheet). Tables also require Schema and Table.
3. Fill Links. For a job, put the Livyatan job URL in **link**.
4. Set Resources and Schedule when the type has them ([08](08-editor-schedule-standards.md)).
5. Pick Source and Target, and Depends on when the job is dependent ([07](07-editor-edges-and-lineage.md)).
6. Add impact notes and scope tags.
7. **Apply**, then **Save draft**.

### Edit a node

1. Select the node → **Edit**. If that node already has a tab, the window focuses it.
2. Change fields. Changing **type** clears type-specific fields that no longer apply. Moving between table, Looker, and Google Sheet keeps the schema and table text. Switching to a job clears them.
3. Changing **refresh frequency** away from dependent clears the schedule dependency when you save ([07](07-editor-edges-and-lineage.md)).
4. **Apply**, then **Save draft**.

### Delete a node

1. Open Edit for the node.
2. **Delete** and confirm.
3. **Save draft**. Read graph warnings afterward and fix any leftover references.

### Schema catalog

Schema on a **table** node is required. You can create a schema name from the form and delete an unused catalog entry. Looker menu names and Google Sheet spreadsheet names are not added to that catalog. Team naming conventions stay TBD in [12](12-standards-and-use-cases.md).

---

## Per-type checklist

### Table (`table`)

**Use when:** A dataset identified by schema and table.

| Area | Guidance |
|------|----------|
| Required | Name, Schema, Table |
| Card | Small line schema, large line table |
| Links | Metadata or docs URL when you have one |
| Schedule | Usually none. Producers are jobs. |
| Lineage | Source and Target pickers. Producer and consumer jobs show on the detail Lineage tab. |

### Job (`etl_job`)

**Use when:** A Livyatan/CML batch or pipeline job. The on-screen type label is **Job**.

| Area | Guidance |
|------|----------|
| Required | Name |
| Card | Name, then owner |
| **link** | Livyatan URL with `/jobs/{id}` so Monitor can map the job |
| **link2** | Mapping or design doc when you use one |
| Resources | A realistic CPU/RAM preset |
| Schedule | Frequency plus run times, or Depends on ([08](08-editor-schedule-standards.md)) |
| Lineage | Source tables and target tables |
| Scopes | Tags for the lenses that should include this job |
| Monitoring | Telegram events ([09](09-editor-job-monitoring.md)) |

### Google Sheet (`google_sheet`)

**Use when:** A Sheet that is a source or sink, including Apps Script that lives with that Sheet. Apps Script is not its own node type.

Identity, before the required **Sheet Name**: optional **Spreadsheet name** and **Sheet page name**. Type them or pick names already used on other Sheet nodes in this draft. The page list follows the spreadsheet you typed. Both are stored as the node’s schema and table fields and are not catalog schemas. The same Sheet Name may be reused when the spreadsheet or page differs. With both blank, the name alone must be unique.

The card shows spreadsheet · page on the small line and Sheet Name large. Link to the Sheet. Jobs that write a Sheet use `exports_to` ([07](07-editor-edges-and-lineage.md)).

<p class="shot-pending">Screenshot pending — assets/node-form-sheet.png: Spreadsheet name, sheet page, and Sheet Name</p>

### Looker Studio (`looker_studio`)

**Use when:** One chart. One node is one chart.

Identity, before the required **Chart name**: optional **Looker menu** and **Looker submenu**. Type them or pick names already used on other Looker nodes in this draft. The submenu list follows the menu you typed. A blank menu lists every submenu in the draft. They are not schema-catalog entries. The same chart name may be reused when the menu or submenu differs. With both blank, the chart name alone must be unique.

The card shows menu · submenu on the small line and the chart name large. Link to the report. Lineage comes from the Source and Target pickers.

<p class="shot-pending">Screenshot pending — assets/node-form-looker.png: Looker menu, submenu, and Chart name</p>

---

## Form / field standards

| Field | Standard |
|-------|----------|
| Name | Clear enough to find in search. Team pattern is TBD in [12](12-standards-and-use-cases.md). |
| Owner | Who to contact on failure. Format is TBD in chapter 12. |
| Job `link` | Must parse as `…/jobs/{id}` before Monitor will map the job |
| Schedule times | WIB, at least 60 minutes apart when there are several ([08](08-editor-schedule-standards.md)) |
| Depends on | Only when frequency is **dependent** |
| Scopes | Optional. Several tags are allowed. Publish on Save. |
| Failure impact | Fill it when the job matters in an incident |

<p class="shot-pending">Screenshot pending — assets/node-form-schedule-multi.png: Several WIB run times on a timed job</p>

<p class="shot-pending">Screenshot pending — assets/node-form-depends-on.png: Depends on picker</p>

## Edge cases

- A wrong Livyatan link leaves Monitor runs unmapped until you fix it and save.
- An empty schema list — create a schema before saving a table.
- Switching frequency off dependent clears the run-after edge on save.
- Minimized forms survive switching among Chart, Table, Monitor, and History until you reload the page.
- Apply with the review already in mind: unapplied tabs are excluded from Save and the review says so.

## Related

- [05 - Draft and History](05-editor-draft-and-history.md)
- [07 - Edges and lineage](07-editor-edges-and-lineage.md)
- [08 - Schedule standards](08-editor-schedule-standards.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [12 - Standards](12-standards-and-use-cases.md)
