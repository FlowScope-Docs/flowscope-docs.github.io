# Reference cheat sheet

**Who:** Everyone  
Quick lookup for roles, chapter index, deep links, glossary, and timezone. For reading paths by role, start on [Home](README.md).

---

## Role map

| Role | Can do |
|------|--------|
| **Viewer** | Read Monitor, Chart, Table. No edit/save, History, acknowledge, Admin, or Telegram tab on execution detail. |
| **Editor** (= job author) | Everything Viewer can do, plus draft/save, History restore, acknowledge, Telegram tab; owns lineage nodes and CML Telegram monitoring payloads. |
| **Admin / Owner** | Everything Editor can do, plus `/admin/users` (invite, approve requests, roles). |

There is **no separate “job author” product role** — Editors write and instrument jobs.

---

## Chapter index

| # | Chapter | Who |
|---|---------|-----|
| 00 | [What is FlowScope](00-what-is-flowscope.md) | Everyone |
| 01 | [Getting started](01-getting-started.md) | Everyone |
| 02 | [Roles and permissions](02-roles-and-permissions.md) | Everyone |
| 03 | [Monitor](03-monitor.md) | Everyone (edit actions: Editor+) |
| 04 | [Chart and Table](04-chart-and-table.md) | Everyone (edit: Editor+) |
| 05 | [Draft workflow and History](05-editor-draft-and-history.md) | Editor+ |
| 06 | [Node forms](06-editor-node-forms.md) | Editor+ |
| 07 | [Edges and lineage](07-editor-edges-and-lineage.md) | Editor+ |
| 08 | [Schedule standards](08-editor-schedule-standards.md) | Editor+ |
| 09 | [Job monitoring (CML + Telegram)](09-editor-job-monitoring.md) | Editor+ |
| — | [Incorporate Flow Report](incorporate-flow-report.md) | Editor+ (job instrumentation) |
| 10 | [Acknowledge missed / stuck runs](10-editor-acknowledge.md) | Editor+ |
| 11 | [Admin](11-admin.md) | Admin / Owner |
| 12 | [Standards and use cases](12-standards-and-use-cases.md) | Everyone |
| 13 | [FAQ and troubleshooting](13-faq-and-troubleshooting.md) | Everyone |

---

## Timezone

All Monitor times and schedule run times use **WIB (Asia/Jakarta)** unless a field says UTC (Telegram payload timestamps are UTC ISO 8601).

---

## Deep-link cheat sheet

| URL param | Effect |
|-----------|--------|
| `?view=monitor\|chart\|table\|history` | Workspace view (default: Monitor) |
| `?monitorView=table\|timeline` | Monitor sub-view (default: table) |
| `?job=<node_external_id>` | Focus/filter job on Monitor; legacy without `monitorView` opens timeline then normalizes |
| `?execution=<execution_id>` | Monitor execution filter (typed or deep link only — row click does **not** set this) |
| `?node=<id>` | Select/focus a Chart/Table node |

**Keep / clear rules (summary):**

- Switching Monitor table ↔ timeline **keeps** `job`, **clears** `execution` when leaving table
- Chart/Table → Monitor (Monitor split) sets `job` + `monitorView`, clears `node` / `execution`

---

## Glossary

| Term | Meaning |
|------|---------|
| **Lineage** | Graph of tables, jobs, and delivery nodes connected by data edges |
| **Node** | One registry item (table, ETL job, script, export, Telegram, Sheet, Looker) |
| **Edge** | Relationship between nodes (`feeds`, `produces`, `exports_to`, `notifies`, or schedule-only `job_dependency`) |
| **Draft** | Unsaved local edits until you Save |
| **Revision** | Saved snapshot in History; restore creates a new revision |
| **CML / Livyatan** | Job runtime platform; job `link` URL contains `/jobs/{id}` |
| **cml_job_id** | Livyatan job id parsed from the node link; used in Telegram payloads |
| **execution_id** | Id for one run; must stay the same across all events in that run |
| **scheduled_run** | Expected run slot materialised from the job’s cron for the timeline |
| **job_dependency** | Explicit “Depends on” schedule edge (not inferred from data lineage) |
| **WIB** | Asia/Jakarta wall-clock timezone |
| **Acknowledge** | Editor+ marks a missed slot or closes a stuck `running` execution |

---

## Related

- [Home](README.md) — role-based reading paths
- [FAQ and troubleshooting](13-faq-and-troubleshooting.md)
- [Incorporate Flow Report](incorporate-flow-report.md)
