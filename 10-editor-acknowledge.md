# 10 - Acknowledge missed / stuck runs

**Who:** Editor, Admin, Superadmin

---

## Purpose

Acknowledge closes an ops record when a **timed** job shows a missed expected slot, or an execution stays `running` because CML never sent FINISHED or FAILED. Fix the schedule or the job helper first when you can ([08](08-editor-schedule-standards.md), [09](09-editor-job-monitoring.md)).

## Who

Editor+. Viewers can see missed and running states. They cannot submit acknowledge.

## When to use

| Situation | Action |
|-----------|--------|
| A timed job’s expected slot passed with no matching run | Missed-run acknowledge on that schedule slot |
| An execution stays `running` with no terminal event | Stuck-running Failed or Succeeded on the execution |
| The job is still running on CML | Leave it |
| The helper never emits events | Fix the helper. Acknowledge only to close the Monitor row. |

Dependent jobs do not have calculated expected slots, so there is no missed-slot acknowledge for a bar the timeline never drew. Team escalation rules stay TBD in [12](12-standards-and-use-cases.md).

---

## Step by step

### Acknowledge a missed slot

1. On Timeline, click the **missed** bar (or an expected bar whose window has passed). The schedule-run sidebar opens.
2. On **Overview**, submit acknowledge. A reason is optional.
3. Wait through **Saving…**, then **Refreshing…**. The panel stays open and updates in place.
4. The bar becomes **acknowledged-missed** (gray dotted). Overview shows the actor’s email.

<p class="shot-pending">Screenshot pending — assets/monitor-ack-missed.png: Missed-run acknowledge on schedule-run Overview</p>

### Close a stuck running execution

1. Open the execution from a table row or an execution bar → **Overview**.
2. If you started from a schedule slot whose linked execution is still `running`, use that panel’s acknowledge action. It switches to the execution panel. The form lives on the execution only.
3. Choose an outcome. Both use a WIB finish time that defaults to now:
   - **Failed** — required reason and required WIB finish time
   - **Succeeded** — required WIB finish time; row count optional (the stored status remains succeeded)
4. Submit. FlowScope writes a manual FINISHED or FAILED event and an acknowledgment audit card. The card is read-only afterward.
5. A second attempt returns **409**. Refresh and read the audit card.

<p class="shot-pending">Screenshot pending — assets/monitor-ack-running.png: Stuck running acknowledge with Failed and Succeeded</p>

## Form / field standards

| Outcome | Required | Optional |
|---------|----------|----------|
| Missed acknowledge | Submit | Reason |
| Stuck → Failed | Reason, WIB finish time | — |
| Stuck → Succeeded | WIB finish time | Row count |

## When to leave the run alone

- The job is still running on CML
- Events, the Livyatan job page, and Open runtime have not been checked
- The next run will stay wrong until the schedule or the missing FINISHED event is fixed
- You are a Viewer — ask an Editor

## Edge cases

- **409** means this execution was already acknowledged.
- A late Telegram terminal event after acknowledge can still move the live status. The audit card remains.
- An expected slot still in the future is not a miss yet.
- Expected or missed slots with no execution show empty placeholders on Events and Telegram.

## Related

- [03 - Monitor](03-monitor.md)
- [08 - Schedule standards](08-editor-schedule-standards.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [12 - Standards and playbooks](12-standards-and-use-cases.md)
