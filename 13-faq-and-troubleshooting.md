# 13 - FAQ and troubleshooting

**Who:** Everyone

---

## Purpose

Fast answers when something looks wrong or a control is missing. Longer procedures are in [12](12-standards-and-use-cases.md) and the feature chapters.

## Who

Everyone.

## When to use

A button is missing, a job is absent, or a time looks off.

---

## Access

**I signed in but I don’t see Monitor or Chart.**  
You are not a workspace member. Submit an access request and wait for Admin approval ([01](01-getting-started.md), [11](11-admin.md)).

**Who can approve me?**  
Admin or Superadmin on `/admin/users`.

---

## Monitor

**What does “No lineage link” mean?**  
The CML id is not mapped to a node. An Editor sets the Livyatan job `link` (`…/jobs/{id}`) and saves ([09](09-editor-job-monitoring.md)). Under a named **Scope**, unmapped runs are hidden. Switch to **All** to see them.

**Times look wrong.**  
Table Start/End and the timeline axis use **WIB (Asia/Jakarta)**. Job payload timestamps are UTC.

**How do I poll every 5 or 10 seconds?**  
On Monitor, set **Auto** to **Off**, **5s**, **10s**, **30s**, or **60s**. Chart and Table do not auto-poll the graph. The Last refresh chip shows **Refreshing…** while a poll is in flight ([03](03-monitor.md)).

**I clicked a row and the address bar did not pin that execution.**  
Expected. A row or bar opens the detail panel only. Share a run with `?execution=` ([03](03-monitor.md)).

**The timeline shows big blocks, not individual runs.**  
You are zoomed out. Use a Zoom preset or Alt+scroll. Hover a block for the status breakdown.

**Bars stayed on screen while the chip said Refreshing.**  
Expected. The timeline keeps the current bars during a reload.

**A Chart job is missing from the timeline.**  
The Gantt lists a job after FlowScope has stored at least one run for it. Until then, look on the table (including **No lineage link** rows on **All**).

**Where did Live and History go?**  
The timeline is one calendar. Default is the last 3 WIB days through the end of today plus 12 hours. Custom ranges use the hotel calendar ([03](03-monitor.md)).

---

## Schedule / timeline

**No expected bars for my job.**  
Adhoc, realtime, and **dependent** jobs do not get expected bars. Dependent jobs show real runs only. A timed job needs saved run times, and it appears on the Gantt only after it has reported once ([08](08-editor-schedule-standards.md)).

**My dependent job did not draw a bar when the master ran late.**  
The timeline does not invent an expected slot for a dependent job. Read the dependent’s own runs, and the Depends-on stroke when several jobs in the chain are on screen.

**I acknowledged a miss. What should I see?**  
An acknowledged-missed bar (gray dotted) and an Overview audit with the actor’s email ([10](10-editor-acknowledge.md)).

---

## Scope, Chart, and editing

**Nodes disappeared after I used the navbar.**  
The **Scope** picker is a lens. A named scope shows tagged nodes plus data-lineage upstream. Choose **All** for the full graph ([01](01-getting-started.md)).

**I edited a field and nobody else can see it.**  
**Apply** only stages your browser draft. **Save draft**, then **Confirm** the review, publishes it ([05](05-editor-draft-and-history.md)).

**My edits vanished after Refresh.**  
An unsaved draft was discarded. Save before refresh. If a bad save already landed, restore a graph revision from History. Restore does not roll scope tags back ([05](05-editor-draft-and-history.md)).

**I don’t see History, Edit, or the form window.**  
Your role is Viewer ([02](02-roles-and-permissions.md)). Ask an Admin for Editor.

**A shared `?node=` link opened Focus.**  
That is what `?node=` does: Chart Focus on that node. Selecting a node yourself does not write `?node=` ([04](04-chart-and-table.md)).

**Graph warnings appear.**  
An edge points at a missing node. Fix Source, Target, or Depends on in the node form, then Save ([07](07-editor-edges-and-lineage.md)).

---

## Monitoring instrumentation

**Events never show up.**  
Check that Telegram `text` is JSON only, that every event in the run uses the same `execution_id`, and that webhook health on the Monitor summary is up ([09](09-editor-job-monitoring.md)). If the webhook itself is down, ask a FlowScope maintainer.

**No Open runtime link.**  
Send `engine_id` on START (`host` is only a legacy fallback).

**Stuck on running.**  
FINISHED or FAILED never arrived. Fix the helper. An Editor can close the row with Failed or Succeeded acknowledge ([10](10-editor-acknowledge.md)). A second acknowledge returns 409.

**The execution id grew a `~a2` suffix.**  
CML sent START again after a terminal event while the same Spark application was still alive. FlowScope stored a new attempt. The earlier row stays finished.

---

## Not part of the product

- A live event feed sidebar beside Monitor. Use the execution **Events** tab. It refreshes with auto-refresh while that tab is open.
- Several workspaces in the UI. Everyone shares **Data DRB**. Scope is a lens inside that workspace.

---

## Related

- [01 - Getting started](01-getting-started.md)
- [03 - Monitor](03-monitor.md)
- [04 - Chart and Table](04-chart-and-table.md)
- [05 - Draft and History](05-editor-draft-and-history.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [10 - Acknowledge](10-editor-acknowledge.md)
- [11 - Admin](11-admin.md)
- [12 - Standards and playbooks](12-standards-and-use-cases.md)
