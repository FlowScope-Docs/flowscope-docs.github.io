# FlowScope Guidebook

<p class="brand-kicker">Coworker docs</p>

<p class="pitch"><strong>FlowScope</strong> is the team tool for <strong>ETL lineage</strong> and <strong>CML job monitoring</strong> during incident response. It does <strong>not</strong> start or schedule Livyatan jobs for you.</p>

<p class="muted-note">All Monitor times use <strong>WIB (Asia/Jakarta)</strong> unless a field says UTC.</p>

## I want to…

<div class="task-list">

<a href="#/01-getting-started"><strong>Sign in / request access</strong>First login and workspace membership</a>

<a href="#/03-monitor"><strong>Triage a failed or late job</strong>Monitor table, timeline, and execution detail</a>

<a href="#/04-chart-and-table"><strong>See impact upstream / downstream</strong>Chart lineage and Table registry</a>

<a href="#/09-editor-job-monitoring"><strong>Instrument a CML job</strong>Wire Livyatan link + Flow Report standard</a>

<a href="#/05-editor-draft-and-history"><strong>Edit lineage (Editor+)</strong>Draft, save, and History restore</a>

<a href="#/11-admin"><strong>Manage users (Admin)</strong>Invite, approve requests, change roles</a>

<a href="#/13-faq-and-troubleshooting"><strong>Fix something that looks wrong</strong>FAQ and troubleshooting</a>

</div>

## Start by role

<div class="role-grid">

<a class="role-card" href="#/00-what-is-flowscope">
<h3>Viewer</h3>
<p>What FlowScope is → Getting started → Roles → Monitor → Chart/Table → FAQ</p>
</a>

<a class="role-card" href="#/05-editor-draft-and-history">
<h3>Editor</h3>
<p>Viewer path, then draft/nodes/edges/schedule → job monitoring → acknowledge</p>
</a>

<a class="role-card" href="#/11-admin">
<h3>Admin / Owner</h3>
<p>Editor path, plus user admin at <code>/admin/users</code></p>
</a>

</div>

**Job authors:** start with [Job monitoring](09-editor-job-monitoring.md), then the standalone [Incorporate Flow Report](incorporate-flow-report.md) (attach that file + one job `.py` to an AI).

---

## Role map

| Role | Can do |
|------|--------|
| **Viewer** | Read Monitor, Chart, Table. No edit/save, History, acknowledge, Admin, or Telegram tab on execution detail. |
| **Editor** | Everything Viewer can do, plus draft/save, History restore, acknowledge, Telegram tab; owns lineage nodes and CML monitoring payloads. |
| **Admin / Owner** | Everything Editor can do, plus `/admin/users` (invite, approve requests, roles). |

There is no separate “job author” product role — Editors write and instrument jobs.

## Deep-link cheat sheet

| URL param | Effect |
|-----------|--------|
| `?view=monitor\|chart\|table\|history` | Workspace view (default: Monitor) |
| `?monitorView=table\|timeline` | Monitor sub-view (default: table) |
| `?job=<node_external_id>` | Focus/filter job on Monitor |
| `?execution=<execution_id>` | Monitor execution filter (typed or deep link only) |
| `?node=<id>` | Select/focus a Chart/Table node |

## Glossary

| Term | Meaning |
|------|---------|
| **Lineage** | Graph of tables, jobs, and delivery nodes |
| **cml_job_id** | Livyatan job id from `/jobs/{id}` on the node link |
| **execution_id** | Id for one run; same across all events in that run |
| **Acknowledge** | Editor+ marks a missed slot or closes a stuck `running` execution |
| **WIB** | Asia/Jakarta wall-clock timezone |

<p class="muted-note">Site: <a href="https://flowscope.github.io/">flowscope.github.io</a> · Screenshots: see <a href="#/assets/README">assets</a> (most still pending).</p>
