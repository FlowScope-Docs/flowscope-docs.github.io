# Reference cheat sheet

**Who:** Everyone  
Quick lookup for roles, chapters, deep links, glossary, and timezone. Reading paths by role are on [Home](README.md).

---

## Role map

| Role | Can do |
|------|--------|
| **Viewer** | Read Monitor, Chart, and Table. Pick a scope. Export. Chart Focus and PNG export. No edit, Save, History, acknowledge, Admin, bulk scope checkboxes, or Telegram tab. |
| **Editor** (= job author) | Viewer capabilities, plus Apply, Save draft, History restore, acknowledge, Telegram tab, and bulk scope tags. Owns lineage nodes and CML Telegram payloads. |
| **Admin / Superadmin** | Editor capabilities, plus `/admin/users` (invite, approve, roles, scope description and delete) and create/rename scopes from the picker. |

There is no separate “job author” product role. Editors write and instrument jobs. Scope is a lens, not a role.

---

## Chapter index

| # | Chapter | Who |
|---|---------|-----|
| 00 | [What is FlowScope](00-what-is-flowscope.md) | Everyone |
| 01 | [Getting started](01-getting-started.md) | Everyone |
| 02 | [Roles and permissions](02-roles-and-permissions.md) | Everyone |
| 03 | [Monitor](03-monitor.md) | Everyone (acknowledge and Telegram: Editor+) |
| 04 | [Chart and Table](04-chart-and-table.md) | Everyone (edit: Editor+) |
| 05 | [Draft workflow and History](05-editor-draft-and-history.md) | Editor+ |
| 06 | [Node forms](06-editor-node-forms.md) | Editor+ |
| 07 | [Edges and lineage](07-editor-edges-and-lineage.md) | Editor+ |
| 08 | [Schedule standards](08-editor-schedule-standards.md) | Editor+ |
| 09 | [Job monitoring (CML + Telegram)](09-editor-job-monitoring.md) | Editor+ |
| — | [Incorporate Flow Report](incorporate-flow-report.md) | Editor+ (job instrumentation) |
| 10 | [Acknowledge missed / stuck runs](10-editor-acknowledge.md) | Editor+ |
| 11 | [Admin](11-admin.md) | Admin / Superadmin |
| 12 | [Standards and use cases](12-standards-and-use-cases.md) | Everyone |
| 13 | [FAQ and troubleshooting](13-faq-and-troubleshooting.md) | Everyone |

---

## Timezone

Monitor times and schedule run times use **WIB (Asia/Jakarta)**. Telegram payload timestamps are UTC ISO 8601.

---

## Deep-link cheat sheet

| URL param | Effect |
|-----------|--------|
| `?view=monitor\|chart\|table\|history` | Workspace view. Default: Monitor. |
| `?monitorView=table\|timeline` | Monitor sub-view. Default: table. |
| `?scope=<slug>` | Scope lens. Overrides the saved scope cookie. |
| `?job=<node_external_id>` | Monitor: seeds the table Job chip and focuses the timeline row when that job has reported. Legacy links without `monitorView` open the timeline, then the URL normalizes. |
| `?execution=<execution_id>` | Opens that execution’s detail panel. A row click does not set this. |
| `?node=<id>` | Shareable **Chart Focus** link. Inbound links open Chart in Focus. Selecting a node does not write `?node=`. `?node=` without `?scope=` uses **All**. |

**Keep / clear:**

- Switching Monitor table and timeline **keeps** `job` and **clears** `execution` when leaving the table.
- Chart or Table → Monitor (Monitor split) sets `job` and `monitorView`. It does not leave a Focus `?node=` behind.
- Table **View chart** and Monitor **Open lineage** select and pan on Chart. They do not enable Focus and they do not write `?node=`.

---

## Glossary

| Term | Meaning |
|------|---------|
| **Lineage** | Graph of tables, jobs, Sheets, and Looker charts connected by data edges |
| **Node** | One registry item: table, job (`etl_job`), Google Sheet, or Looker Studio chart |
| **Edge** | Feeds, produces, exports to, or schedule-only job dependency |
| **Scope** | Named tag set inside Data DRB. The picker shows tagged nodes plus data upstream. |
| **Draft** | Browser-only edits after Apply, until Save draft is confirmed |
| **Apply** | Stages the open form tab into the draft. Does not publish. |
| **Revision** | A saved snapshot in History. Restore creates a new revision and does not roll tags back. |
| **Focus** | Chart view of one node, its data upstream, and one hop of data downstream |
| **CML / Livyatan** | Job runtime. The job `link` contains `/jobs/{id}`. |
| **cml_job_id** | Livyatan job id parsed from the node link. Telegram payloads send this, not a FlowScope node id. |
| **execution_id** | Id for one run. Keep it the same across events in that run. A later START after a terminal event can be stored as `{id}~a2`. |
| **scheduled_run** | An expected slot materialised from a timed job’s cron |
| **job_dependency** | Explicit Depends on edge. Saved only when frequency is dependent. |
| **WIB** | Asia/Jakarta |
| **Acknowledge** | Editor+ marks a missed timed slot, or closes a stuck `running` execution |
| **No lineage link** | A Monitor row whose CML id is not mapped to a node |

---

## Related

- [Home](README.md)
- [FAQ and troubleshooting](13-faq-and-troubleshooting.md)
- [Incorporate Flow Report](incorporate-flow-report.md)
