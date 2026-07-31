# FlowScope Coworker Guidebook

**Audience:** BRI coworkers using FlowScope (Viewer, Editor, Admin)  
**Language:** English (Indonesian translation later)  
**Status:** Pass 4 complete (English prose for chapters 00?13; screenshots still placeholders; team TBD boxes open in ch. 12)  
**Last updated:** 2026-07-31 (public repo + incorporate-flow-report)

**Site:** [https://phoenixz33.github.io/FlowScope-Guidebook/](https://phoenixz33.github.io/FlowScope-Guidebook/)  
**GitHub:** [PhoenixZ33/FlowScope-Guidebook](https://github.com/PhoenixZ33/FlowScope-Guidebook)

English is the source of truth. Screenshots are placeholders until PNGs are dropped into [`assets/`](assets/README.md).

**All writing passes complete:** chapters [00](00-what-is-flowscope.md)–[13](13-faq-and-troubleshooting.md). Fill team TBD boxes in [12](12-standards-and-use-cases.md); add PNGs per [assets/README.md](assets/README.md); Indonesian translation later.

Job authors: see [Incorporate Flow Report](incorporate-flow-report.md) (also linked from [09](09-editor-job-monitoring.md)).

---

## What is FlowScope (one line)

Internal tool for **ETL lineage** and **CML job monitoring** during incident response. It does **not** start or schedule CML jobs for you.

Start with [00 ? What is FlowScope](00-what-is-flowscope.md).

---

## Reading paths

| Role | Read in order |
|------|----------------|
| **Viewer** | [00](00-what-is-flowscope.md) ? [01](01-getting-started.md) ? [02](02-roles-and-permissions.md) ? [03](03-monitor.md) ? [04](04-chart-and-table.md) ? skim [12](12-standards-and-use-cases.md) ? [13](13-faq-and-troubleshooting.md) |
| **Editor** (= job author) | Viewer path + [05](05-editor-draft-and-history.md) ? [06](06-editor-node-forms.md) ? [07](07-editor-edges-and-lineage.md) ? [08](08-editor-schedule-standards.md) ? [09](09-editor-job-monitoring.md) ? [10](10-editor-acknowledge.md) ? [12](12-standards-and-use-cases.md) ? [13](13-faq-and-troubleshooting.md) |
| **Admin / Owner** | Editor path + [11](11-admin.md) |

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
| 10 | [Acknowledge missed / stuck runs](10-editor-acknowledge.md) | Editor+ |
| 11 | [Admin](11-admin.md) | Admin / Owner |
| 12 | [Standards and use cases](12-standards-and-use-cases.md) | Everyone |
| 13 | [FAQ and troubleshooting](13-faq-and-troubleshooting.md) | Everyone |
| — | [Incorporate Flow Report](incorporate-flow-report.md) | Editor+ (job instrumentation) |

---

## Role map

| Role | Can do |
|------|--------|
| **Viewer** | Read Monitor, Chart, Table. No edit/save, History, acknowledge, Admin, or Telegram tab on execution detail. |
| **Editor** (= job author) | Everything Viewer can do, plus draft/save, History restore, acknowledge, Telegram tab; owns lineage nodes **and** CML Telegram monitoring payloads. |
| **Admin / Owner** | Everything Editor can do, plus `/admin/users` (invite, approve requests, roles). |

There is **no separate ?job author? product role** ? Editors write and instrument jobs.

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
| `?execution=<execution_id>` | Monitor execution filter (typed or deep link only ? row click does **not** set this) |
| `?node=<id>` | Select/focus a Chart/Table node |

**Keep / clear rules (summary):**

- Switching Monitor table ? timeline **keeps** `job`, **clears** `execution` when leaving table
- Chart/Table ? Monitor (Monitor split) sets `job` + `monitorView`, clears `node` / `execution`

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
| **scheduled_run** | Expected run slot materialised from the job?s cron for the timeline |
| **job_dependency** | Explicit ?Depends on? schedule edge (not inferred from data lineage) |
| **WIB** | Asia/Jakarta wall-clock timezone |
| **Acknowledge** | Editor+ marks a missed slot or closes a stuck `running` execution |

---

## Related docs

| Doc | When |
|-----|------|
| [Incorporate Flow Report](incorporate-flow-report.md) | Instrument a CML job with `lib.report_event` (AI-attachable) |
| [09 — Job monitoring](09-editor-job-monitoring.md) | Wire Livyatan link + verify in Monitor |
| FlowScope maintainers / ops | Bot token, webhook registration, deploy (not in this public repo) |

---

## Screenshots

See [assets/README.md](assets/README.md).

- **When the PNG exists:** put `![Caption](./assets/filename.png)` on its own line (not inside a `>` blockquote).
- **When missing:** italic pending note naming the expected file.

### Viewing images in Cursor

Cursor's **in-tab Preview / Markdown toggle** often **does not render images** (known IDE bug). To see screenshots:

1. Open the `.md` file
2. Command Palette (`Ctrl+Shift+P`) ? **Markdown: Open Preview to the Side**

On GitHub after push, images render normally.

---

## Pass status

| Pass | Scope | Status |
|------|-------|--------|
| 1 | Skeletons + topics + placeholders | **Done** |
| 2 | Chapters 00-04 prose | **Done** |
| 3 | Chapters 05-10 prose | **Done** |
| 4 | Chapters 11-13 + team TBD answers | **Done** (TBD boxes left for team fill-in) |
| Later | Real screenshots; Indonesian | Pending (user-owned) |
