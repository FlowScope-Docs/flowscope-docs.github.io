# 03 - Monitor

**Who:** Everyone (acknowledge and Telegram tab: Editor+)

---

## Purpose

Monitor shows **live and historical CML job runs** reported through Telegram: what is running, what failed, what was expected on the schedule, and how a run progressed event by event.

## Who

| Role | Can |
|------|-----|
| Viewer | Table, timeline, filters, detail Overview/Events, Open lineage, View on Timeline, Open runtime |
| Editor+ | Everything above, plus Telegram tab and acknowledge actions ([10](10-editor-acknowledge.md)) |

**Layout note:** The **summary strip** appears on the table sub-view only. Timeline uses a compact toolbar (toggle, Live/History, date picker); refresh/section label live in the workspace controls bar.

## When to use

- Triage failed or late jobs
- Compare **expected** vs **actual** on the timeline
- Jump from a run to lineage or the Livyatan engine runtime
- Confirm a job is mapped (not “No lineage link”)

All times in the table and timeline axis are **WIB (Asia/Jakarta)** unless noted.

---

## Step by step

### Use the executions table

1. Open **Monitor** (`/?view=monitor`). Table sub-view is the default (`monitorView=table`).
2. Read the **summary strip** (table only): running count, successes today, failures today, average runtime today, webhook health.
3. Scan the executions table: job name (or CML id), status, step, Start/End (WIB), duration, row count, error snippet.
4. Filter with **status**, **text search**, **job node id**, and/or **execution id**.
5. Toggle **auto-refresh** with the interval picker: **Off** (manual only) or **5s / 10s / 30s / 60s**. Only the active Monitor sub-view polls (table executions vs timeline schedule). While an execution detail sidebar is open, Overview / Events / Telegram soft-refresh on the same cadence (no full-panel loading flash).

<p class="shot-pending">Screenshot pending — assets/monitor-table-overview.png: Monitor table with summary strip</p>

<p class="shot-pending">Screenshot pending — assets/monitor-filters.png: Monitor filters (status, search, job, execution)</p>

**Important:** Clicking a **row** opens the detail sidebar only. It does **not** fill the Execution filter or write `?execution=` in the URL. To pin an execution in the filter, type it (or use a deep link).

Unmapped runs show the CML job id and **No lineage link**  -  an Editor must fix the Livyatan `link` on the node and save ([09](09-editor-job-monitoring.md)).

### Inspect an execution

1. Click a table row (or an execution bar on the timeline).
2. On **Overview**, check status, timing, rows, errors, and **Engine ID**. If Engine ID is present, use **Open runtime** to open the Livyatan CML engines URL.
3. Open **Events** for the numbered vertical event timeline (START, STEP/INFO, WARNING, FINISHED, FAILED). Use the search box on message/step/type. Timestamps are WIB.
4. **Editor+:** open the **Telegram** tab for raw/related Telegram log context.
5. Use header actions:
   - **Open lineage**  -  Chart focused on the mapped job (if mapped)
   - **View on Timeline**  -  Timeline sub-view with that job row focused
6. Close with Esc, the X control, or click outside.

<p class="shot-pending">Screenshot pending — assets/monitor-execution-overview.png: Execution detail Overview tab</p>

<p class="shot-pending">Screenshot pending — assets/monitor-execution-events.png: Execution Events timeline</p>

<p class="shot-pending">Screenshot pending — assets/monitor-open-runtime.png: Engine ID and Open runtime link</p>

Editor+ acknowledge for stuck `running` lives on Overview  -  see [10 - Acknowledge](10-editor-acknowledge.md).

### Use the timeline

1. Switch to **Timeline** in the Monitor toolbar (`monitorView=timeline`).
2. Choose **Live** or **History**:
   - **Live**  -  about 24 hours centered on NOW (loads a wider window). Use **Scroll to NOW** if you pan away.
   - **History**  -  starts with the past few days in a coarser zoom; set From/To (max 30 days) and **Apply**. Near the left edge, **Load earlier** adds more past days.
3. Zoom with the **Zoom** dropdown (1h / 3h / 6h / 12h / 24h / Reset / Fit) or **Alt+scroll** (shows `Zoom: Custom` when not on a preset).
4. When zoomed out, bars **aggregate** into blocks. Hover a block for time range, run count, and status breakdown. Zoom in to see individual runs.
5. Read the **legend** for expected, running, succeeded, failed, missed, and acknowledged-missed.
6. Job rows are ordered by explicit schedule **Depends on** (`job_dependency`), upstream first  -  not by data-lineage edges.
7. While data refreshes, bars usually stay visible (soft refresh); the controls area may show a background refresh state.

<p class="shot-pending">Screenshot pending — assets/monitor-timeline-live.png: Timeline Live mode with NOW line</p>

<p class="shot-pending">Screenshot pending — assets/monitor-timeline-history.png: Timeline History mode with date picker</p>

<p class="shot-pending">Screenshot pending — assets/monitor-zoom-legend.png: Zoom control and bar-state legend</p>

### Open schedule-run or job detail from the timeline

1. Click an **expected / missed / dependency / acknowledged-missed** bar → schedule-run sidebar (Overview / Events / Telegram). Slots without a linked execution show empty states on Events/Telegram.
2. Click an **execution** bar → execution sidebar (same as table row click).
3. Click the **job name** in the left label column → job-level panel even if the row has no bars in the window.
4. Use **Open lineage** from the panel header when the job is mapped.

<p class="shot-pending">Screenshot pending — assets/monitor-schedule-run-panel.png: Schedule-run detail sidebar</p>

Missed-run acknowledge and the “running bridge” to the execution panel: [10 - Acknowledge](10-editor-acknowledge.md).

### Jump from Chart/Table or share a link

- From a job on Chart/Table (Editor+ Monitor split), open Timeline or Table with job focus ([04](04-chart-and-table.md)).
- Deep links (also in [README](README.md#deep-link-cheat-sheet)):

| Param | Effect |
|-------|--------|
| `?view=monitor` | Monitor |
| `?monitorView=table\|timeline` | Sub-view |
| `?job=<node_external_id>` | Focus/filter job (legacy without `monitorView` opens timeline then normalizes) |
| `?execution=<id>` | Execution filter only if typed or deep-linked |

Keep/clear: switching table ↔ timeline **keeps** `job`, **clears** `execution` when leaving table.

## Form / field standards

| Control | Standard |
|---------|----------|
| Auto-refresh | Off / 5s / 10s / 30s / 60s; polls only the active sub-view; open execution detail soft-refreshes with the poll |
| Filters | Use job/execution ids when you need a pinned filter; row click alone is not enough for `execution` |
| Times | Read WIB on Start/End and axis; payload timestamps from jobs are UTC |

Acknowledge form fields are documented in [10](10-editor-acknowledge.md).

## Edge cases

- **No lineage link** → map the Livyatan URL on the node and save.
- **Empty Events** → no events stored yet, or expected/missed slot with no linked execution.
- **Aggregated blocks** → zoom in before concluding a single run is missing.
- **Not available yet:** live event feed sidebar (separate from the execution Events tab).

## Related

- [04  -  Chart and Table](04-chart-and-table.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [10 - Acknowledge](10-editor-acknowledge.md)
- [13 - FAQ](13-faq-and-troubleshooting.md)

