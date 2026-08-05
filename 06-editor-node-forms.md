# 06  -  Node forms

**Who:** Editor, Admin, Owner

---

## Purpose

Create, edit, and delete lineage nodes for all four types. Correct forms keep Chart/Table accurate and  -  for jobs  -  keep Monitor mapped via the Livyatan `link`.

## Who

Editors (= job authors), Admins, Owners. Save through the [draft workflow](05-editor-draft-and-history.md).

## When to use

- Register a new table, job, Google Sheet, or Looker asset
- Fix owner, links, schedule, lineage, or impact metadata
- Remove a node that no longer exists

---

## Shared form rules

Sidebar section order (job-like nodes):

1. **Identity**  -  type, name, schema/table when applicable  
2. **Links**  -  `link`, `link2` (labels vary by type)  
3. **Resources**  -  resource preset (CPU/RAM) when shown  
4. **Schedule**  -  refresh frequency, run times / Depends on  
5. **Lineage**  -  Source / Target pickers  
6. **Impact & notes**  -  business purpose, failure impact, notes  

One field per row (full width). Detail panel uses the same section order for read-only view.

<p class="shot-pending">Screenshot pending — assets/node-form-sections.png: Node form showing section order (Identity through Impact)</p>

### `link` vs `link2`

| Field | Typical use |
|-------|-------------|
| **link** | Primary external URL  -  for CML jobs, the Livyatan job page ending in `/jobs/{id}` |
| **link2** | Secondary / mapping URL for job-like types |

If the Livyatan URL is wrong or missing `/jobs/{id}`, Monitor shows **No lineage link**. The form shows a parsed CML job id when the URL is valid; the map syncs on **Save**.

---

## Step by step

### Create a node

1. On Chart or Table, start **create** for the node type you need.
2. Fill required Identity fields (Type, Name; Schema + Table for tables).
3. Fill Links  -  for jobs put the Livyatan job URL in **link**.
4. Set Resources and Schedule as needed ([08](08-editor-schedule-standards.md)).
5. Pick Source/Target (and Depends on if dependent) with the searchable pickers ([07](07-editor-edges-and-lineage.md)).
6. Add Impact & notes.
7. **Save draft**.

### Edit a node

1. Select the node → **Edit** tab.
2. Change fields; changing **type** clears type-specific fields that no longer apply.
3. Changing **refresh frequency** away from `dependent` clears schedule dependency edges on save ([07](07-editor-edges-and-lineage.md)).
4. **Save draft**.

### Delete a node

1. Open Edit for the node.
2. Delete and confirm.
3. **Save draft**. Fix any graph warnings for orphaned edges afterward.

### Schema catalog

- **Schema** on table nodes is required.
- You can **create** a new schema name from the form and **delete** a schema from the catalog when unused  -  prefer team naming conventions ([12](12-standards-and-use-cases.md) TBD).

---

## Per-type checklist

### Table (`table`)

**Use when:** A physical or logical dataset in a schema.

| Area | Guidance |
|------|----------|
| Required | Type, Name, Schema, Table |
| Links | Metadata / docs URL as needed |
| Schedule | Usually N/A or light metadata  -  producers are jobs |
| Lineage | Producer/consumer jobs appear from edges |

### Job (`etl_job`)

**Use when:** A Livyatan/CML (or equivalent) batch/pipeline job.

| Area | Guidance |
|------|----------|
| Required | Type, Name |
| **link** | Livyatan URL with `/jobs/{id}` (required for Monitor mapping) |
| **link2** | Mapping / design doc if used |
| Resources | Set realistic preset |
| Schedule | Frequency + run times or Depends on ([08](08-editor-schedule-standards.md)) |
| Lineage | Source tables → Target tables |
| Monitoring | Instrument Telegram events ([09](09-editor-job-monitoring.md)) |

### Google Sheet (`google_sheet`)

**Use when:** A Sheet that is a source or sink in the pipeline (including Apps Script that lives with the Sheet — do not model Apps Script as a separate node type).

Link to the Sheet; lineage to jobs that read/write it. Jobs that write a Sheet typically use `exports_to`.

### Looker Studio (`looker_studio`)

**Use when:** A dashboard/report asset fed by lineage tables or jobs.

Link to the report; lineage from upstream datasets/jobs.

---

## Form / field standards

| Field | Standard |
|-------|----------|
| Name | Clear, unique enough to find in search; team naming TBD in [12](12-standards-and-use-cases.md) |
| Owner | Who to ping on failure (format TBD in 12) |
| Job `link` | Must parse as `…/jobs/{id}` for Monitor |
| Schedule times | WIB; 60-minute minimum spacing for multi run-times ([08](08-editor-schedule-standards.md)) |
| Depends on | Only when frequency is **dependent** |
| Failure impact | Fill when the job matters for incidents |

<p class="shot-pending">Screenshot pending — assets/node-form-schedule-multi.png: Multi run-times (WIB) for a timed job</p>

<p class="shot-pending">Screenshot pending — assets/node-form-depends-on.png: Dependent job Depends-on picker</p>

## Edge cases

- Wrong Livyatan link → unmapped Monitor runs until fixed and saved.
- Empty schema list → create a schema before saving a table.
- Switching frequency from dependent → run-after edges cleared on save.

## Related

- [07  -  Edges and lineage](07-editor-edges-and-lineage.md)
- [08  -  Schedule standards](08-editor-schedule-standards.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [12  -  Standards](12-standards-and-use-cases.md)
