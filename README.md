# FlowScope Coworker Guidebook

**Audience:** BRI coworkers using FlowScope (Viewer, Editor, Admin)  
**Language:** English (Indonesian translation later)

**Site:** [https://flowscope-docs.github.io/](https://flowscope-docs.github.io/)  
**GitHub:** [FlowScope-Docs/flowscope-docs.github.io](https://github.com/FlowScope-Docs/flowscope-docs.github.io) (local folder: `FlowScope-Guidebook`)

English is the source of truth. Screenshots are placeholders until PNGs are dropped into [`assets/`](assets/README.md).

---

## What is FlowScope (one line)

Internal tool for **ETL lineage** and **CML job monitoring** during incident response. It does **not** start or schedule CML jobs for you.

**Getting started:** begin with [What is FlowScope](00-what-is-flowscope.md), then [Getting started](01-getting-started.md).

---

## Reading paths

| Role | Read in order |
|------|----------------|
| **Viewer** | [00](00-what-is-flowscope.md) → [01](01-getting-started.md) → [02](02-roles-and-permissions.md) → [03](03-monitor.md) → [04](04-chart-and-table.md) → skim [12](12-standards-and-use-cases.md) → [13](13-faq-and-troubleshooting.md) |
| **Editor** (= job author) | Viewer path + [05](05-editor-draft-and-history.md) → [06](06-editor-node-forms.md) → [07](07-editor-edges-and-lineage.md) → [08](08-editor-schedule-standards.md) → [09](09-editor-job-monitoring.md) → [Incorporate Flow Report](incorporate-flow-report.md) → [10](10-editor-acknowledge.md) → [12](12-standards-and-use-cases.md) → [13](13-faq-and-troubleshooting.md) |
| **Admin / Owner** | Editor path + [11](11-admin.md) |

---

## Quick jumps

- Job failed or late → [Monitor](03-monitor.md)
- Instrument a CML job → [Job monitoring](09-editor-job-monitoring.md) + [Incorporate Flow Report](incorporate-flow-report.md)
- Something looks wrong → [FAQ and troubleshooting](13-faq-and-troubleshooting.md)

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

## Role map

| Role | Can do |
|------|--------|
| **Viewer** | Read Monitor, Chart, Table. No edit/save, History, acknowledge, Admin, or Telegram tab on execution detail. |
| **Editor** (= job author) | Everything Viewer can do, plus draft/save, History restore, acknowledge, Telegram tab; owns lineage nodes and CML Telegram monitoring payloads. |
| **Admin / Owner** | Everything Editor can do, plus `/admin/users` (invite, approve requests, roles). |

There is **no separate “job author” product role** — Editors write and instrument jobs.

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

| Doc | When |
|-----|------|
| [Incorporate Flow Report](incorporate-flow-report.md) | Instrument a CML job with `lib.report_event` (AI-attachable) |
| [FAQ and troubleshooting](13-faq-and-troubleshooting.md) | Common issues and fixes |
| [assets/](assets/README.md) | Screenshot filenames (most still pending) |
