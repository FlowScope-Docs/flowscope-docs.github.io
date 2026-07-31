# 00  -  What is FlowScope

**Who:** Everyone

---

## Purpose

FlowScope is internal tool for **ETL pipeline lineage** and **CML job monitoring** during incident response. Use it to answer: what feeds what, who owns a job, when a run was expected, and what happened on the latest execution.

## Who

Everyone who uses the app. How deep you go depends on role  -  see [02 - Roles and permissions](02-roles-and-permissions.md) and the [reading paths](README.md#reading-paths).

## When to use


| Situation                              | Use                            |
| -------------------------------------- | ------------------------------ |
| A job failed or looks late             | [Monitor](03-monitor.md)       |
| You need impact / upstream-downstream  | [Chart](04-chart-and-table.md) |
| You need a registry list or export     | [Table](04-chart-and-table.md) |
| You maintain lineage or CML monitoring | Editor chapters 05 - 10          |
| You manage who can log in              | [Admin](11-admin.md)           |




## What FlowScope is

- A **lineage graph** of tables, jobs, scripts, exports, Telegram bots, Sheets, and Looker assets
- A **Monitor** of CML executions reported over Telegram (expected vs actual on a timeline)
- A **shared workspace** (one workspace for the team; slug `default`)



## What FlowScope is not

- It does **not** start, stop, or schedule jobs on Livyatan/CML for you
- It is **not** a general ticketing or chat system
- It is **not** multi-workspace in the UI (one shared graph)



## How Monitor connects to lineage

1. An Editor puts the Livyatan job URL in the node’s **link** field (`…/jobs/{id}`).
2. On save, FlowScope maps that id to the node (`cml_job_map`).
3. The CML job sends Telegram JSON events with the same `cml_job_id`.
4. Monitor shows the run under the lineage job name (or “No lineage link” if the map is missing).

Editors are also **job authors**: they own both the graph and the monitoring payloads. Details: [09 - Job monitoring](09-editor-job-monitoring.md).

## Step by step

Go straight to [Getting started](01-getting-started.md).



## Form / field standards

N/A

## Screenshot

<p class="shot-pending">Screenshot pending — assets/workspace-chrome.png: Workspace after login with Monitor as the default view</p>

## Edge cases

- Signed in but no membership → you see an access request panel, not an empty graph ([01](01-getting-started.md)).
- Unmapped Monitor rows mean the Livyatan link is missing or wrong on the job node ([13](13-faq-and-troubleshooting.md)).



## Related

- [01 - Getting started](01-getting-started.md)
- [02 - Roles and permissions](02-roles-and-permissions.md)
- [09 - Job monitoring](09-editor-job-monitoring.md) (Editors instrumenting CML jobs)

<div class="chapter-nav">
<a href="#/">← Home</a>
<a href="#/01-getting-started">Next: Getting started →</a>
</div>

