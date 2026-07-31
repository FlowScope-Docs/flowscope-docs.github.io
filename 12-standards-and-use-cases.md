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
| Node types | Use the type that matches the asset: table, etl_job, typescript_script, csv_export, telegram, google_sheet, looker_studio ([06](06-editor-node-forms.md)) |
| Data vs schedule | `feeds` / `produces` / … for lineage; `job_dependency` only via frequency **dependent** + Depends on ([07](07-editor-edges-and-lineage.md)) |
| Job Monitor mapping | Livyatan `link` must end with `/jobs/{id}`; save the graph ([09](09-editor-job-monitoring.md)) |
| Schedule times | WIB; multi run-times ≥ 60 minutes apart ([08](08-editor-schedule-standards.md)) |
| Acknowledge | Last resort after checking Events / Livyatan / helper; prefer fixing instrumentation ([10](10-editor-acknowledge.md)) |
| Drafts | Save before leaving; restore via History if a bad save shipped ([05](05-editor-draft-and-history.md)) |

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

1. Open Monitor table; filter status **failed** or search the job name.
2. Open the execution → Overview (error snippet, times) and **Events** (which step failed).
3. If Engine ID is present, **Open runtime** on Livyatan.
4. **Open lineage** to see downstream impact on Chart.
5. Fix the job / data; ensure the next run sends FINISHED or FAILED ([09](09-editor-job-monitoring.md)).
6. Escalate per team TBD contact if needed.

### Playbook: Missed run

1. Open Timeline Live/History; find the **missed** bar.
2. Confirm the job schedule in Edit is correct (frequency, WIB times, Depends on).
3. Check whether CML ran outside the match window or never ran.
4. If the miss is accepted (skip / known outage), Editor **acknowledges** with an optional reason ([10](10-editor-acknowledge.md)).
5. If the schedule was wrong, fix and Save; do not only acknowledge forever.

### Playbook: Unmapped run (“No lineage link”)

1. Note the CML job id on the Monitor row.
2. Find or create the job node; set **link** to the Livyatan URL ending in `/jobs/{that id}`.
3. **Save draft**.
4. Confirm the next (or current) run shows the lineage name. Past rows may still show the old label until refreshed/remapped in practice  -  verify with a new event if unsure.

### Playbook: Stuck running

1. Open execution Overview + Events  -  is FINISHED/FAILED missing?
2. Check Livyatan / Open runtime whether the session is still alive.
3. If still running → wait; do not acknowledge.
4. If CML finished but helper failed → fix helper ([09](09-editor-job-monitoring.md)); Editor may close out with Failed or Succeeded acknowledge ([10](10-editor-acknowledge.md)).
5. Second acknowledge returns 409  -  stop retrying.

### Playbook: Wrong schedule bars

1. Compare node Schedule (frequency, run times, dependent prerequisite) to Timeline expected bars.
2. Remember: dependent expecteds follow **master schedule slots**, not late adhoc master runs ([08](08-editor-schedule-standards.md)).
3. Zoom in if bars are aggregated ([03](03-monitor.md)).
4. Fix form + Save; reload Timeline.

### Playbook: Impact / lineage

1. Open Chart; select the failed or critical node.
2. Read upstream (what feeds it) and downstream (what it feeds).
3. Use Table export if you need a filtered list for stakeholders ([04](04-chart-and-table.md)).
4. Notify owners per team TBD ownership map.

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

