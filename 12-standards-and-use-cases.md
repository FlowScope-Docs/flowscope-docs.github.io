# 12  -  Standards and use cases

**Who:** Everyone (Editors own most conventions)

---

## Purpose

Shared rules for how the team models lineage and handles incidents. **Product rules** (what the app allows) are fixed below. **Team standards** marked TBD should be filled by your team and kept current.

## Who

Viewers follow playbooks for triage. Editors enforce form and monitoring standards. Admins set access and may own escalation contacts.

## When to use

- Before creating nodes or wiring Monitor
- During an incident (playbooks)
- When onboarding a new Editor

---

## Product standards (always apply)

| Topic | Standard |
|-------|----------|
| Node types | Use the type that matches the asset: table, etl_job, google_sheet, looker_studio ([06](06-editor-node-forms.md)). Apps Script lives with Google Sheet — not a separate node type. |
| Data vs schedule | `feeds` / `produces` / … for lineage; `job_dependency` only via frequency **dependent** + Depends on ([07](07-editor-edges-and-lineage.md)) |
| Job Monitor mapping | Livyatan `link` must end with `/jobs/{id}`; save the graph ([09](09-editor-job-monitoring.md)) |
| Schedule times | WIB; multi run-times ≥ 60 minutes apart ([08](08-editor-schedule-standards.md)) |
| Acknowledge | Last resort after checking Events / Livyatan / helper; prefer fixing instrumentation ([10](10-editor-acknowledge.md)) |
| Drafts | Apply stages locally. Save draft and Confirm publish. History restore rolls the graph back and leaves scope tags as they are ([05](05-editor-draft-and-history.md)) |

If a team habit conflicts with the UI (e.g. “infer run-after from data edges”), **the product behaviour wins**.

---

## Team standards (TBD  -  fill these in)

Replace the italics with your team’s answers. Leave the **Team standard (TBD)** label until filled so gaps stay visible.

> **Team standard (TBD):** Preferred node naming pattern  -  _e.g. `Domain - NN. Description`_

> **Team standard (TBD):** Owner field format  -  _email? team name?_

> **Team standard (TBD):** Who owns which domains / pipelines

> **Team standard (TBD):** When to set failure severity / summary

> **Team standard (TBD):** When to acknowledge vs escalate / fix the job

> **Team standard (TBD):** Escalation contact for Monitor incidents

---

## Incident playbooks

### Playbook: Job failed

1. Open the Monitor table. Turn on status **Failed**, or search the job name. Use **Warning** when the run succeeded and still reported a warning.
2. Open the execution. Overview lists warning and failed lines. **Events** shows which step failed.
3. When the header shows an engine id, use **Open runtime**. **Open job page** opens the Livyatan job.
4. **Open lineage** to read impact on Chart. **Focus** limits the canvas to upstream plus one hop downstream ([04](04-chart-and-table.md)).
5. Fix the job / data; ensure the next run sends FINISHED or FAILED ([09](09-editor-job-monitoring.md)).
6. Escalate per team TBD contact if needed.

### Playbook: Missed run

1. Open Timeline and find the **missed** bar on a timed job. Dependent jobs do not draw expected slots ([08](08-editor-schedule-standards.md)).
2. Open the job form and check frequency, WIB run times, and Depends on.
3. Check whether CML ran outside the match window or never ran.
4. If the miss is accepted (skip or a known outage), an Editor **acknowledges** it with an optional reason ([10](10-editor-acknowledge.md)).
5. If the schedule was wrong, fix it and Save. Acknowledging the same bad schedule on every cycle leaves the next day wrong too.

### Playbook: Unmapped run (“No lineage link”)

1. On Monitor, set Scope to **All** so unmapped rows are visible. Note the CML job id.
2. Find or create the job node. Set **link** to the Livyatan URL ending in `/jobs/{that id}`.
3. **Apply**, then **Save draft** and confirm.
4. Confirm the next run shows the lineage name. Verify with a new event if an older row still shows the CML id.

### Playbook: Stuck running

1. Open execution Overview + Events  -  is FINISHED/FAILED missing?
2. Check Livyatan / Open runtime whether the session is still alive.
3. If still running → wait; do not acknowledge.
4. If CML finished but helper failed → fix helper ([09](09-editor-job-monitoring.md)); Editor may close out with Failed or Succeeded acknowledge ([10](10-editor-acknowledge.md)).
5. Second acknowledge returns 409  -  stop retrying.

### Playbook: Wrong schedule bars

1. Compare the job’s Schedule (frequency, WIB run times, Depends on) with the timeline.
2. Timed jobs show expected and missed bars. Dependent, adhoc, and realtime jobs show real runs only ([08](08-editor-schedule-standards.md)).
3. Zoom in when bars are aggregated ([03](03-monitor.md)).
4. Fix the form, Apply, and Save. Reload the timeline. New expected slots fill from today forward, not backward over older days in the picker.

### Playbook: Impact / lineage

1. Open Chart and select the failed or critical node.
2. Read **Data flow** on Overview (direct source and target tables) and **Lineage** (job relationships). **Focus** keeps upstream and one hop of data downstream on the canvas.
3. Export the filtered Table when stakeholders need a list ([04](04-chart-and-table.md)).
4. Notify owners per the team TBD ownership map.

---

## Form / field standards

Follow chapters 06 - 09 for field-level rules. Apply team TBD naming/owner formats once filled above.

## Screenshot

N/A  -  use screenshots in feature chapters.

## Edge cases

- Product vs team conflict → product wins for what can be stored.
- Viewer cannot acknowledge or edit  -  pull an Editor into the playbook at the action step.

## Related

- [06  -  Node forms](06-editor-node-forms.md)
- [08  -  Schedule standards](08-editor-schedule-standards.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [10 - Acknowledge](10-editor-acknowledge.md)
- [13 - FAQ](13-faq-and-troubleshooting.md)

