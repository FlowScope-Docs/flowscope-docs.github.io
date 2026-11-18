# 03 - Monitor

**Who:** Everyone (acknowledge and Telegram tab: Editor+)

---

## Purpose

Monitor shows CML job runs reported through Telegram: what is running, what failed, what was expected on the schedule, and how a run progressed event by event.

## Who

| Role | Can |
|------|-----|
| Viewer | Table, timeline, filters, detail Overview/Events, Open lineage, Open job page, View on Timeline, View on Table, Open runtime |
| Editor+ | Everything above, plus the Telegram tab and acknowledge actions ([10](10-editor-acknowledge.md)) |

The **summary strip** is on the table sub-view only. The timeline uses a compact toolbar. Refresh and the section label live in the workspace controls bar. Both sub-views share the **Last refresh** chip.

## When to use

- Triage a failed, warning, or late job
- Compare **expected** and **actual** bars on the timeline
- Jump from a run to lineage or the Livyatan job page
- Confirm a job is mapped (the row shows a lineage name, not **No lineage link**)

Times on the table and the timeline axis are **WIB (Asia/Jakarta)**. Payload timestamps from jobs are UTC.

A **Scope** lens (navbar) narrows both sub-views to that scope’s tagged nodes plus their data-lineage upstream. Unmapped runs stay hidden while a named scope is selected. **All** shows the full workspace, including unmapped runs.

---

## Step by step

### Use the executions table

1. Open **Monitor** (`/?view=monitor`). The table is the default (`monitorView=table`).
2. Read the **summary strip**: running, Success today, Success (warning) today, Failed today, average runtime today, and webhook health. “Today” is the current WIB calendar day. Success today includes successes that also have a warning.
3. Scan the table. **Job ID** comes first, then **Job** (lineage name, or the CML id with **No lineage link**). When an engine id is present, the Job cell also shows it and an **Open runtime** icon. Then status, step, Start/End (WIB), duration, row count, and an error snippet. Duration stays empty while the run is still going; after finish it is the whole-job time.
4. Filter with the status toggles: **All**, **Running**, **Success**, **Warning**, **Failed**. **All** is on by default. Choosing a status turns All off. Turning off the last status returns to All. Warning means a succeeded run that also reported a WARNING event.
5. Use search, the date control, and **Filters** / **Sort** / **Reset**.
   - Default range is **Last 3 days** (WIB calendar days). The hotel calendar picks a custom range up to 30 days. Future days are disabled.
   - Filters are **Job** (display name), **Job ID**, or **Owner**. Chips apply immediately.
   - **Reset** clears search, status, chips, and sorts, and restores Last 3 days when the range was custom.
6. Set **Auto** to **Off** or **5s / 10s / 30s / 60s**. Only the sub-view you are looking at polls. The Last refresh chip glows **Refreshing…** while a poll or a manual Refresh is in flight. After the first load, a refresh merges the latest day into the rows already on screen.

<p class="shot-pending">Screenshot pending — assets/monitor-table-overview.png: Monitor table with the summary strip</p>

<p class="shot-pending">Screenshot pending — assets/monitor-filters.png: Status toggles, search, and the date calendar</p>

Clicking a **row** opens the detail sidebar only. It does not write `?execution=` in the URL. A shared `?execution=` link still opens that run, including when it sits outside the loaded window.

Unmapped runs show the CML job id and **No lineage link**. An Editor maps them by saving a Livyatan `link` on the job node ([09](09-editor-job-monitoring.md)).

### Inspect an execution

1. Click a table row or an execution bar on the timeline.
2. The header shows the job name. The subtitle is the **engine id**, with **Open runtime** when that id was sent. Header actions:
   - **Open lineage** — Chart with the mapped job selected (when the run is mapped)
   - **Open job page** — Livyatan `/jobs/{id}` in a new tab, including unmapped runs that still have a CML id
   - **View on Timeline** (from the table) — timeline, with that job’s name highlighted
   - **View on Table** (from the timeline) — table, and the execution panel stays open
3. **Overview** loads first: status, timing, rows, and each WARNING and FAILED line for the run (newest first). You do not need the Events tab to read those lines.
4. **Events** is a row list with a type toggle (**All**, Start, Step, Info, Warning, Finished, Failed) and a search box. An empty selection means All. Timestamps are WIB. A duration on a STEP Load row is that step’s loop time, separate from the whole-job duration on Overview.
5. **Editor+:** open **Telegram** for the raw log context. Events and Telegram load when you open the tab.
6. While the panel is open, Overview stays current with the Monitor poll. Events and Telegram refresh when that tab is the one on screen.
7. Close with Esc or the close control.

<p class="shot-pending">Screenshot pending — assets/monitor-execution-overview.png: Execution Overview with engine id, Open runtime, and Open job page</p>

<p class="shot-pending">Screenshot pending — assets/monitor-execution-events.png: Events list with the type toggle</p>

Editor+ acknowledge for a stuck `running` execution lives on Overview — see [10 - Acknowledge](10-editor-acknowledge.md).

### Use the timeline

1. Switch to **Timeline** (`monitorView=timeline`). The timeline is one calendar.
2. The default window is the last **3 WIB calendar days**, drawn through the **end of today WIB plus 12 hours**. The right edge stays put until the WIB day rolls. **Now** is a vertical line when the current time sits inside the window, and the view opens centered on it. Default zoom is **24h**.
3. Pick a custom range with the same style of hotel calendar (30-day cap). A custom range stays fixed and is not shared with the table’s calendar.
4. Zoom with the **Zoom** dropdown (**1h / 3h / 6h / 12h / 24h / Reset / Fit**) or **Alt+scroll** (**Zoom: Custom**). Reset, and changing the date range, return to **24h**.
5. Zoomed out, bars **aggregate**. Hover a block for the WIB range, run count, and status breakdown. Zoom in to see individual runs.
6. Read the **legend**: expected, running, Success, Success (warning), failed, missed, and acknowledged-missed. Expected and missed slots for timed jobs use a short minimum width so a point-in-time slot stays visible. Finished and running bars use the real duration.
7. Job rows are ordered by explicit **Depends on** (`job_dependency`), upstream first. A chain of two or more visible jobs gets one color stroke in the name column. A job with no stored execution is absent from the Gantt, even if it exists on the Chart.
8. **Find job**: type a name or id, then click or press Enter on a match. The name cell blinks amber. Typing alone does not scroll. Escape closes the list without moving.
9. **Dependent** jobs show their real runs only. The timeline does not draw a calculated expected bar for them.
10. Bars stay on screen during a refresh. The Last refresh chip is the refresh signal. **Scroll to NOW** appears when now is inside the loaded window.

<p class="shot-pending">Screenshot pending — assets/monitor-timeline.png: Default timeline with the now line and 24h zoom</p>

<p class="shot-pending">Screenshot pending — assets/monitor-zoom-legend.png: Zoom control and bar-state legend</p>

<p class="shot-pending">Screenshot pending — assets/monitor-depends-on-stroke.png: Depends-on color stroke in the job-name column</p>

### Open schedule-run or job detail from the timeline

1. Click an **expected**, **missed**, or **acknowledged-missed** bar. The schedule-run sidebar opens (Overview / Events / Telegram). The subtitle is the expected WIB time. A slot with no linked execution shows an empty state on Events and Telegram.
2. Click an **execution** bar. The execution sidebar opens, same as a table row.
3. Click the **job name** in the left column. A job-level panel opens even when that row has no bars in the window. Overview can show **Instrumented** and **Last event**.
4. Use **Open lineage** and **Open job page** from the panel header when those links exist.

<p class="shot-pending">Screenshot pending — assets/monitor-schedule-run-panel.png: Schedule-run detail sidebar</p>

Missed-run acknowledge, and the bridge from a still-running linked slot into the execution panel: [10 - Acknowledge](10-editor-acknowledge.md).

### Jump from Chart/Table or share a link

From a job on Chart or Table, Editor+ use the Monitor split to open Timeline or Table with that job focused ([04](04-chart-and-table.md)).

| Param | Effect |
|-------|--------|
| `?view=monitor` | Monitor |
| `?monitorView=table\|timeline` | Sub-view |
| `?job=<node_external_id>` | Seeds the table Job chip and focuses the timeline row when that job has reported at least once. A job that has never reported does not gain a Gantt row. Legacy links without `monitorView` open the timeline, then the URL normalizes. |
| `?execution=<id>` | Opens that execution’s detail panel |
| `?scope=<slug>` | Applies a scope lens on Chart, Table, and Monitor |

Switching table and timeline **keeps** `job` and **clears** `execution` when leaving the table.

## Form / field standards

| Control | Standard |
|---------|----------|
| Auto-refresh | Off / 5s / 10s / 30s / 60s. Polls the active sub-view. An open execution detail soft-refreshes with that poll. |
| Date range | Last 3 WIB days, or a confirmed custom range of at most 30 days |
| Status | All, or one or more of Running, Success, Warning, Failed |
| Times | Read WIB on Start/End and the axis. Payload timestamps are UTC. |

Acknowledge fields are in [10](10-editor-acknowledge.md).

## Edge cases

- **No lineage link** — map the Livyatan URL on the node and save.
- **Empty Events** — no events stored yet, or an expected/missed slot with no linked execution.
- **Aggregated blocks** — zoom in before deciding a single run is missing.
- **Job missing on the timeline** — it has no stored execution yet. Check the table for an unmapped CML id.
- **Rows vanished after picking a scope** — the lens hides anything outside that scope’s upstream cone, including unmapped runs. Switch the picker back to **All**.
- A separate live event feed beside the workspace is not part of Monitor. The Events tab is the event list, and it refreshes with the poll while it is open.

## Related

- [04 - Chart and Table](04-chart-and-table.md)
- [08 - Schedule standards](08-editor-schedule-standards.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [10 - Acknowledge](10-editor-acknowledge.md)
- [13 - FAQ](13-faq-and-troubleshooting.md)
