# 10 - Acknowledge missed / stuck runs

**Who:** Editor, Admin, Superadmin

---

## Purpose

Acknowledge is an **ops close-out** tool when the timeline shows a missed expected slot, or an execution is stuck in `running` because CML never sent FINISHED/FAILED. Prefer fixing schedule or monitoring instrumentation first ([08](08-editor-schedule-standards.md), [09](09-editor-job-monitoring.md)).

## Who

Editor+. Viewers can see missed/running states but cannot submit acknowledge forms.

## When to use

| Situation | Action |
|-----------|--------|
| Expected slot passed with no run (or no match) | Missed-run acknowledge |
| Execution stays `running` with no terminal event | Stuck-running Failed or Succeeded |
| Job is still legitimately running | **Do not** acknowledge |
| Helper never emits events | Fix helper; acknowledge only if you must close the UI state |

Team escalation rules: TBD in [12](12-standards-and-use-cases.md).

---

## Step by step

### Acknowledge a missed slot

1. On Timeline, click the **missed** (or expected past) bar → schedule-run sidebar.
2. On **Overview**, submit acknowledge (optional reason).
3. Wait for **Saving…** then **Refreshing…**. The panel stays open and syncs.
4. Confirm the bar becomes **acknowledged-missed** (gray dotted) and Overview shows who acknowledged (email).

You can also acknowledge some mapped missed cases from the execution panel where the UI offers it.

<p class="shot-pending">Screenshot pending — assets/monitor-ack-missed.png: Missed-run acknowledge form on schedule-run Overview</p>

### Close a stuck running execution

1. Open the execution (table row or execution bar) → **Overview**.
2. If the schedule-run panel shows a linked execution still `running`, use its **Acknowledge** CTA  -  it switches you to this execution panel (the form lives here only).
3. Choose outcome:
   - **Failed**  -  required reason; finish time = now  
   - **Succeeded**  -  required **WIB** finish time; optional row count  
4. Submit. FlowScope writes a manual FINISHED/FAILED event and stores an acknowledgment audit card (read-only afterward).
5. A second attempt returns **409**  -  do not retry as a “fix”; refresh and read the audit card.

<p class="shot-pending">Screenshot pending — assets/monitor-ack-running.png: Stuck running acknowledge (Failed / Succeeded)</p>

## Form / field standards

| Outcome | Required | Optional |
|---------|----------|----------|
| Missed acknowledge | (submit) | Reason |
| Stuck → Failed | Reason |  -  |
| Stuck → Succeeded | WIB finish time | Row count |

## When **not** to acknowledge

- The job is still running on CML
- You have not checked Events / Livyatan / Open runtime
- The root issue is a bad schedule or missing Telegram FINISHED  -  fix that for the next run
- You are a Viewer (ask an Editor)

## Edge cases

- **409** on second acknowledge  -  already closed out.
- Late Telegram terminal after ack  -  status may move; audit remains.
- Expected slot still in the future  -  not a miss yet.
- No-execution expected/missed  -  Events/Telegram tabs show empty placeholders.

## Related

- [03 - Monitor](03-monitor.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [12  -  Standards / playbooks](12-standards-and-use-cases.md)

