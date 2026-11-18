# 00 - What is FlowScope

**Who:** Everyone

---

## Purpose

FlowScope is an internal tool for **ETL pipeline lineage** and **CML job monitoring** during incident response. Use it to answer what feeds what, who owns a job, when a timed run was expected, and what happened on the latest execution.

## Who

Everyone who uses the app. How far you can go depends on role. See [02 - Roles and permissions](02-roles-and-permissions.md) and the [reading paths](README.md#start-here-by-role).

## When to use

| Situation | Use |
|-----------|-----|
| A job failed, warned, or looks late | [Monitor](03-monitor.md) |
| You need impact around one node | [Chart](04-chart-and-table.md) |
| You need a registry list or an export | [Table](04-chart-and-table.md) |
| You maintain lineage or CML monitoring | Editor chapters 05–10 |
| You manage who can log in, or the scope list | [Admin](11-admin.md) |

## What FlowScope is

- A **lineage graph** of four node types: table, job, Google Sheet, and Looker Studio chart
- A **Monitor** of CML executions reported over Telegram, with expected versus actual bars for timed jobs
- One shared workspace, slug `data-drb`, name **Data DRB**
- Optional **scopes** inside that workspace (for example Brimo): a view lens of tagged nodes plus their data upstream, not a second workspace and not a permission

## What FlowScope is not

- It does not start, stop, or schedule jobs on Livyatan/CML
- It is not a ticketing or chat system
- It does not offer several workspaces in the UI

## How Monitor connects to lineage

1. An Editor puts the job URL in the node’s **link** field (`…/jobs/{id}`).
2. On **Save draft**, FlowScope maps that id to the node.
3. The CML job sends Telegram JSON events with the same `cml_job_id`.
4. Monitor shows the run under the lineage job name, or **No lineage link** when the map is missing.

Editors are also job authors: they own the graph and the monitoring payloads. Details: [09 - Job monitoring](09-editor-job-monitoring.md).

## Step by step

Go to [Getting started](01-getting-started.md).

## Screenshot

<p class="shot-pending">Screenshot pending — assets/workspace-chrome.png: Workspace after login, Monitor default, scope picker and view pills</p>

## Edge cases

- Signed in but no membership → an access request panel, not an empty graph ([01](01-getting-started.md)).
- An unmapped Monitor row means the job link on the job node is missing or wrong ([13](13-faq-and-troubleshooting.md)).

## Related

- [01 - Getting started](01-getting-started.md)
- [02 - Roles and permissions](02-roles-and-permissions.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
