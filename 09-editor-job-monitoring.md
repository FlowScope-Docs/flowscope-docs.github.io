# 09 - Job monitoring (CML + Telegram)

**Who:** Editor, Admin, Owner (= job authors)

---

## Purpose

Editors own both the lineage job node and the **CML → Telegram → Monitor** instrumentation. This chapter is the coworker checklist. For day-to-day job code changes, use the standalone [Incorporate Flow Report](incorporate-flow-report.md) standard (attach that file + one job `.py` to an AI).

## Who

Anyone who authors or maintains monitored jobs (Editor+). Bot token / webhook registration is **ops** — ask a FlowScope maintainer if the webhook is down or a new bot needs registering. That is not day-to-day editing.

## When to use

- Onboarding a new CML job into FlowScope
- Fixing “No lineage link” or missing events
- Adding Engine ID / Open runtime support
- Instrumenting (or fixing) `lib.report_event` in a job `.py`

---

## Mental model

```text
Livyatan job URL on node.link  →  cml_job_map on Save
        ↑
CML helper sends Telegram JSON with same cml_job_id
        ↓
FlowScope webhook  →  Monitor executions / events / timeline link
```

- Jobs **never** send FlowScope node ids  -  only `cml_job_id`.
- Jobs **do not** send schedule in the payload; FlowScope attaches `scheduled_run_id` when a run matches an expected slot (±30 minutes) at ingest.

---

## Step by step

### Wire a job node to Livyatan

1. Create/edit the job node ([06](06-editor-node-forms.md)).
2. Set **link** to the Livyatan job page, e.g.  
   `https://livyatan.apps.slithereen.data.bri.co.id/Digital-Innovation/playground-dgv/jobs/14143`
3. Confirm the form can parse the CML job id (URL must end with `/jobs/{id}`).
4. Set schedule correctly ([08](08-editor-schedule-standards.md)).
5. **Save draft**.

### Instrument with Flow Report

Prefer the shared `lib` helper rather than hand-writing Telegram `sendMessage` calls.

1. Open [Incorporate Flow Report](incorporate-flow-report.md) (self-contained).
2. Default mode is **light-touch**: keep local helpers; add `lib.report_event` / `lib.spark_session` only. Use **full migrate** to `lib.*` write helpers only if you explicitly ask for it.
3. With an AI: attach **only** `incorporate-flow-report.md` + the target job `.py`, then prompt e.g.  
   > Follow incorporate-flow-report.md. Add FlowScope reporting to `<path/to/job.py>`. Default = light-touch.
4. One job `.py` per run.
5. After the AI finishes, run the **Verification** checklist at the bottom of that standard.
6. For adhoc/backdate runs, set `cml_job_id` and/or `BACKDATE_*` as noted in the standard.

### Emit lifecycle events from CML (payload shape)

If you are inspecting raw Telegram messages (or writing a helper from scratch):

1. From job code, send one Telegram `sendMessage` per event (or use `lib.report_event`, which does this).
2. `text` must be **JSON only** (no Markdown, no code fences, no prose wrapper).
3. Keep the same **`execution_id`** for every event in that run.
4. Recommended lifecycle: **START** → **STEP** / **INFO** / **WARNING** → **FINISHED** or **FAILED**.
5. On **START**, send **`engine_id`** (from `CDSW_ENGINE_ID` / `ENGINE_ID` / hostname) so Monitor can show **Open runtime**.
6. On success terminal: `FINISHED` + `status: succeeded` (+ `duration_ms`, `row_count` as applicable).
7. On failure terminal: `FAILED` + `status: failed` + `error_message`.

Do **not** send `host` in new helpers (`engine_id` only; `host` is a short-lived fallback).

Example START:

```json
{
  "cml_job_id": "14143",
  "execution_id": "14143_20260618140000",
  "event_type": "START",
  "status": "running",
  "step": "Initialize",
  "message": "Job started",
  "engine_id": "hp2ilolblx1d9jly",
  "timestamp": "2026-06-18T14:00:00Z"
}
```

### Verify in Monitor

1. Open Monitor table  -  the run should show the **lineage job name**, not only the CML id.
2. Open Overview  -  status, times (WIB), Engine ID / Open runtime when provided.
3. Open Events  -  ordered lifecycle cards.
4. Open Timeline  -  expected slot should link when schedule matching succeeds.
5. If unmapped → fix `link`, save again, wait for the next run (or check filters).

---

## Form / field standards (payload checklist)

| Field | Required | Notes |
|-------|----------|-------|
| `cml_job_id` | Always | Same id as Livyatan `/jobs/{id}` |
| `execution_id` | Always | Stable for the whole run |
| `event_type` | Always | `START`, `STEP`, `INFO`, `WARNING`, `FINISHED`, `FAILED` |
| `status` | Always | `running`, `succeeded`, `failed` |
| `timestamp` | Always | ISO 8601 **UTC** |
| `step` / `message` | Recommended | Human triage |
| `engine_id` | Recommended on START | Enables Open runtime |
| `duration_ms` | Terminal | Duration |
| `row_count` | Success terminal | Rows processed |
| `error_message` | Failure | Before re-raise |

Snippets, anti-patterns, and the full verification list: [Incorporate Flow Report](incorporate-flow-report.md).

## Edge cases

- Missing FINISHED/FAILED → stuck `running`  -  fix the helper; Editors may [acknowledge](10-editor-acknowledge.md) as a last resort.
- Late terminal webhook after manual ack  -  live status may update; acknowledgment audit remains.
- Ops-only failures (webhook down)  -  check webhook health on Monitor summary; escalate to a FlowScope maintainer / ops.

## Related

- [Incorporate Flow Report](incorporate-flow-report.md)
- [03 - Monitor](03-monitor.md)
- [06 - Node forms](06-editor-node-forms.md)
- [10 - Acknowledge](10-editor-acknowledge.md)
- [13 - FAQ](13-faq-and-troubleshooting.md)

