# 04  -  Chart and Table

**Who:** Everyone (Edit tab and Monitor split: Editor+)

---

## Purpose

**Chart** is the interactive lineage graph. **Table** is the mode-driven registry for the same nodes, with filters, sort, and export. Both share a detail sidebar; Editors also get Edit and a Monitor jump for jobs.

## Who

| Role | Can |
|------|-----|
| Viewer | Browse Chart/Table, search/filter, read Detail, export Table rows, open Chart from Table |
| Editor+ | Detail + **Edit**, create/update/delete via draft save ([05](05-editor-draft-and-history.md)), Monitor split on job nodes |

## When to use

| Goal | Prefer |
|------|--------|
| Impact / upstream-downstream | Chart |
| Find by owner, schedule, or notes; export | Table |
| Inspect one node’s fields | Either → Detail sidebar |
| Open Monitor for a job | Editor+ Monitor split, or Open lineage from Monitor the other way |

---

## Step by step

### Explore the Chart

1. Open **Chart** (`/?view=chart`).
2. Pan and zoom the canvas; use the **MiniMap** for large graphs. Layout is left-to-right by data flow (not schema swimlanes).
3. In the **left sidebar**, search by name, filter by **owner** and **type**, then click a result to select and focus.
4. Click a node on the canvas. The **right sidebar** shows Detail (and Edit for Editor+).
5. Read highlighting:
   - Data upstream / downstream along `feeds` / `produces` (and related data edges)
   - Schedule prerequisite / dependent jobs when `job_dependency` applies
   - Schedule dependency edges appear in context (selection/hover), not always as a full permanent overlay
6. Resize sidebars with the flush drag handles; double-click a handle to reset width.
7. Use **fullscreen** for a clean canvas (hides chrome and both sidebars until exit).

If Editors see **graph warnings** (orphan/invalid edges), fix them in Edit before relying on the graph ([05](05-editor-draft-and-history.md), [07](07-editor-edges-and-lineage.md)).

<p class="shot-pending">Screenshot pending — assets/chart-canvas.png: Chart canvas with MiniMap</p>

<p class="shot-pending">Screenshot pending — assets/chart-search-detail.png: Search sidebar and detail panel</p>

### Read the Detail panel

The right sidebar has a **combined header** and three tabs: **Overview**, **Lineage**, and **Notes**.

**Header (always visible)**

- Thin **type-colour** accent on the left; type label on top (Looker/Google Sheet include the chart or sheet name in that line).
- **View only** badge for viewers; icon buttons for **Edit** (Editor+), **Monitor** (jobs), **Focus** / **View chart**, and **Close**.
- Node **name**, then a **location** line: tables show `schema.table` with copy; Looker/Sheet show menu/submenu or spreadsheet/page (no copy).
- **Summary strip:** Owner · Refresh · Upstream **N** · Downstream **N** (empty Owner/Refresh show **—**). Click a count to open the **Lineage** tab. Adhoc/realtime **Refresh** appears here.
- **Graph warnings** (if any) collapse in an amber banner above the tabs.

**Tabs**

- **Overview** — Identity (created/updated by, schema/table or Looker/Sheet fields, last updated, scope tags), Links (**Job ID** with copy for jobs), Resources and Schedule when they apply. Owner and Refresh are only in the summary strip, not repeated here.
- **Lineage** — Grouped lists (upstream, downstream, jobs, source/target tables, etc.). Click a row to select and focus that node. Unresolved table refs show as “not in chart” and are not clickable.
- **Notes** — Business purpose, failure impact, and notes (tab label dims when all empty).

**Empty fields:** Link, Scopes, Owner, Refresh, and Depends on (for dependent jobs) always show **—** when blank; most other empty fields are hidden. Empty Lineage/Notes tabs show short placeholder messages.

**Schedule display rules** (in Overview unless noted):

- Timed → human schedule + cron
- Dependent → **Depends on** only (no redundant Schedule/Cron rows)
- Adhoc / realtime → frequency in the **Refresh** summary strip only

### Use the Table registry

1. Open **Table** (`/?view=table`).
2. Pick a **mode** (one per node type):

   - Tables  
   - Jobs  
   - Google Sheets  
   - Looker Studio  

3. Use mode-aware **search**, then add **filters** (operators such as `is`, `contains`, `is_empty`, …) and **sorts** (header shortcuts or sort builder).
4. Click a row to open the right sidebar (Detail / Edit on wide screens).
5. From the sidebar, jump to **Chart** focused on that node when you need the graph.
6. **Export** CSV or XLSX  -  downloads the **current filtered/sorted** rows for the active mode only.

Empty mode and “no results for filters” messages are separate; clear filters if the list looks wrong.

<p class="shot-pending">Screenshot pending — assets/table-mode-filters.png: Table mode picker with filters</p>

<p class="shot-pending">Screenshot pending — assets/table-export.png: CSV / XLSX export control</p>

### Open Monitor for a job (Editor+)

On job-type nodes in the Chart/Table sidebar:

1. Use the **Monitor** split control.
2. Primary action → Monitor **Timeline** with that job focused (row scroll/highlight).
3. Chevron menu → Monitor **Table** with the job filter set.

This clears Chart `node` selection params in favor of Monitor `job` + `monitorView` ([README deep links](README.md#deep-link-cheat-sheet)).

<p class="shot-pending">Screenshot pending — assets/editor-monitor-split.png: Monitor split control on a job node (Timeline / Table)</p>

Viewers do not edit; they still use Detail, Chart navigation, and Table export.

## Form / field standards

| Area | Standard |
|------|----------|
| Table filters | Prefer precise `is` for enums (owner, frequency); `contains` for free text |
| Export | Always check filters before download  -  export matches what you see |
| Detail vs Edit | Viewers: Detail only. Editors: change fields in Edit, then **Save draft** ([05](05-editor-draft-and-history.md)) |
| Full form field rules | [06  -  Node forms](06-editor-node-forms.md), [08  -  Schedule](08-editor-schedule-standards.md) |

## Edge cases

- **No results** after filters → clear filters or switch mode.
- **Graph warnings** after bad edges → Editor must repair links/edges.
- Deep link `?node=<id>` selects that node on Chart/Table.
- Chart node type labels are Title Case; silhouettes use per-type chrome (storage / courier / destination) with a four-type legend on the canvas toolbar.

## Related

- [03 - Monitor](03-monitor.md)
- [05  -  Draft and History](05-editor-draft-and-history.md)
- [06  -  Node forms](06-editor-node-forms.md)
- [07  -  Edges and lineage](07-editor-edges-and-lineage.md)
- [12  -  Standards](12-standards-and-use-cases.md)

