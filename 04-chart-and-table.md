# 04 - Chart and Table

**Who:** Everyone (Edit, Focus tools, and the Monitor split: Editor+ for edit; Focus and export are available to anyone who can open Chart)

---

## Purpose

**Chart** is the interactive lineage graph. **Table** is the registry for the same nodes, with filters, sort, and export. Both share a read-only detail sidebar. Editors change nodes in a separate form window, then publish with **Save draft**.

## Who

| Role | Can |
|------|-----|
| Viewer | Browse Chart and Table, search and filter, read Detail, export Table rows, open Chart from Table, use Chart Focus and PNG export |
| Editor+ | Everything above, plus create and edit through the form window, bulk scope tags, and the Monitor split on job nodes |

## When to use

| Goal | Prefer |
|------|--------|
| Impact around one node | Chart, then **Focus** or the detail **Data flow** section |
| Find by owner, scope, or notes, or export | Table |
| Read one node’s fields | Either view → detail sidebar |
| Change a node | Detail **Edit** → form window → **Apply** → **Save draft** |
| Open Monitor for a job | Editor+ Monitor split, or Open lineage from Monitor |

A **Scope** in the navbar narrows Chart, Table, and Monitor together. The lens is tagged nodes plus data-lineage upstream. **All** is the full graph. `?scope=<slug>` shares a lens. A node link with `?node=` and no `?scope=` opens **All**.

---

## Step by step

### Explore the Chart

1. Open **Chart** (`/?view=chart`).
2. Pan and zoom. The **MiniMap** sits at the bottom-left. Layout follows data edges (`feeds`, `produces`, `exports_to`), left to right. Schedule **Depends on** edges do not place the nodes.
3. In the left sidebar, search by the node’s own fields: name, schema, table, owner, purpose, notes, a timed schedule, the Livyatan URL, or a pasted CML job id. Dependent jobs do not match on the prerequisite name buried in “After …”. The internal node id is not searchable.
4. Under search, filter the **list** with **All / Table / Job / Sheet / Looker**. The canvas stays complete. **Owner** stays in the **Filters** popover.
5. Click a node. The right sidebar opens with Detail and pushes the canvas narrower without changing zoom. Clicking empty canvas once does nothing. Double-click empty canvas zooms toward the pointer when nothing is selected. With a selection or Focus on, that double-click clears the selection or leaves Focus and does not zoom.
6. Read highlighting. Data upstream and downstream follow `feeds` / `produces` / `exports_to`. Prerequisite and dependent jobs highlight when a `job_dependency` applies. Those schedule edges show in context (selection or hover), not as a permanent overlay of every dependency.
7. **Focus** (toolbar or sidebar) limits the canvas to the anchor node, every data-lineage upstream node, and one hop of data downstream. Further downstream and Depends-on edges stay hidden. The visible piece is laid out on its own. Exit from the toolbar, the sidebar, Esc, or a double-click on empty canvas. Exit keeps the current selection and restores the camera. Choosing another node while Focus is on does not move the cone until you apply Focus again (**Focus this node**).
8. While Focus is on, the address bar gains `?node=` so you can share that cone. An inbound `?node=` link opens Chart already in Focus. Ordinary selection does not write `?node=`. Table **View chart** and Monitor **Open lineage** select and pan, and they leave Focus off.
9. **Export** (camera on the Chart toolbar) downloads a PNG of the fitted visible graph. With Focus on, the PNG is the cone.
10. Resize sidebars with the drag handles. Double-click a handle to reset width. **Fullscreen** hides the top chrome and both sidebars until you exit.

Cards are the same on the canvas and in the list: a type-colored border, then two lines. **Table** shows schema, then table. **Looker** shows menu · submenu, then chart name. **Google Sheet** shows spreadsheet · page, then sheet name. **Job** shows name, then owner. Schedule text stays in the detail sidebar.

<p class="shot-pending">Screenshot pending — assets/chart-canvas.png: Chart canvas with MiniMap and type legend</p>

<p class="shot-pending">Screenshot pending — assets/chart-search-detail.png: Search, type toggles, and the detail sidebar</p>

<p class="shot-pending">Screenshot pending — assets/chart-focus.png: Focus cone for one node</p>

If Editors see **graph warnings**, fix Source, Target, or Depends on in the form before treating the graph as trustworthy ([05](05-editor-draft-and-history.md), [07](07-editor-edges-and-lineage.md)).

### Read the Detail panel

The right sidebar is details only. Editors use **Edit** in the header to open the form window. The sidebar has a **combined header** and three tabs: **Overview**, **Lineage**, and **Notes**.

**Header (always visible)**

- Thin **type-colour** accent on the left; type label on top (Looker and Google Sheet include the chart or sheet name in that line).
- **View only** badge for viewers. Icon buttons for **Edit** (Editor+), **Monitor** (jobs), **Focus** or **Focus this node** / **Exit focus** on Chart, **View chart** on Table, and **Close**.
- Node **name**, then a **location** line: tables show `schema.table` with copy; Looker and Sheet show menu/submenu or spreadsheet/page (no copy). Jobs have no location line.
- **Summary strip:** Owner · Refresh · Sources **N** · Targets **N** (empty Owner or Refresh shows **—**). Click a count to open **Overview** and scroll to **Data flow**. Adhoc and realtime **Refresh** appear here.
- **Graph warnings** (if any) collapse in an amber banner above the tabs.

**Tabs**

- **Overview** — Identity (created and updated by, schema/table or Looker/Sheet fields, last updated, scope tags), then **Data flow** (direct **Source tables** and **Target tables**). For **jobs**, this lists the configured source and target tables. For **tables**, **Google Sheet**, and **Looker Studio**, it shows tables one data job away, with a **via job** line when that applies. Unresolved table refs are muted, say “not in chart”, and are not clickable. Then Links (**Job ID** with copy for jobs), Resources, and Schedule when they apply. Owner and Refresh stay in the summary strip.
- **Lineage** — **Job relationships only** (prerequisite and dependent jobs for jobs; produced-by and feeds jobs for tables, sheets, and Looker). Click a row to select that node. There is no full transitive upstream/downstream list on this tab.
- **Notes** — Business purpose, failure impact, and notes. The tab label dims when all of those are empty.

**Empty fields:** Link, Scopes, Owner, Refresh, and Depends on (for dependent jobs) show **—** when blank. Most other empty fields are hidden. Empty Lineage and Notes tabs show a short placeholder.

**Schedule display rules** (in Overview unless noted):

- Timed → human schedule and cron
- Dependent → **Depends on** only
- Adhoc / realtime → frequency in the **Refresh** summary strip only

### Use the Table registry

1. Open **Table** (`/?view=table`).
2. Pick a **mode** (one per node type): Tables, Jobs, Google Sheets, Looker Studio.
3. Search the row’s own fields (name, owner, purpose, schema and table labels, links, CML job id, notes, frequency). Reference columns such as source tables are for filters, not for the search box.
4. Add **filters** and **sorts**. Click a sortable header to cycle ascending, descending, and off. A number on the header is the multi-sort order.
5. **Scope** sits after Name: membership chips for tags saved on that node. An empty cell can still appear in a named lens because the node is upstream of a tagged seed. The filter matches if any selected scope is on the row.
6. Click a row to open the detail sidebar. Click the same row again to close it.
7. **Export** downloads the current filtered and sorted rows for the active mode. The main button is CSV. The chevron also offers XLSX.
8. From the sidebar, **View chart** selects that node on Chart and pans to it. It does not turn Focus on.

When the workspace has at least one scope, Editors see row checkboxes on **All** and on a named lens. Select rows, then use the chip bar: a solid chip means every selected row has that scope, an empty chip means none, a mixed chip means some. Click an off chip to add that scope. Click a solid or mixed chip to remove only that scope. Other tags stay. This stages a draft until **Save draft**. Viewers see the Scope column and no checkboxes. Changing mode clears the selection.

<p class="shot-pending">Screenshot pending — assets/table-mode-filters.png: Table mode picker, search, and filters</p>

<p class="shot-pending">Screenshot pending — assets/table-scope-bulk.png: Scope column and bulk scope chips</p>

<p class="shot-pending">Screenshot pending — assets/table-export.png: Export menu with CSV and XLSX</p>

Empty mode and “no results for filters” are different messages. Clear filters if the list looks wrong. A scope with no tagged seeds shows an empty state and a way back to **All**.

### Open Monitor for a job (Editor+)

On job nodes in the Chart or Table sidebar:

1. Use the **Monitor** split.
2. The primary action opens Monitor **Timeline** with that job’s name highlighted, when the job has reported at least once.
3. The chevron opens Monitor **Table** with the job filter set.

This uses Monitor `job` and `monitorView`. It does not leave a Chart `?node=` Focus link behind.

<p class="shot-pending">Screenshot pending — assets/editor-monitor-split.png: Monitor split on a job node (Timeline and Table)</p>

## Form / field standards

| Area | Standard |
|------|----------|
| Table filters | Use `is` for a single owner or frequency. Use `contains` for free text. |
| Export | Check filters first. Export matches the rows on screen. |
| Detail vs form | Viewers stay on Detail. Editors change fields in the form window, **Apply**, then **Save draft** ([05](05-editor-draft-and-history.md)). |
| Scope | Tags are a lens, not a permission. Bulk chips and the form **Scopes** field publish on Save. |

Field rules: [06 - Node forms](06-editor-node-forms.md) and [08 - Schedule](08-editor-schedule-standards.md).

## Edge cases

- **No results** after filters — clear filters or switch mode.
- **Graph warnings** — an Editor repairs Source, Target, or Depends on.
- `?node=<id>` opens Chart **Focus** on that node. It does not mean “select this row on Table”.
- Focus plus a named scope shows the overlap of the two cones. A jump to a node outside the scope switches that jump to **All**.
- Chart type colors are Table, Job, Google Sheet, and Looker. The legend is a color swatch plus the type name.

## Related

- [03 - Monitor](03-monitor.md)
- [05 - Draft and History](05-editor-draft-and-history.md)
- [06 - Node forms](06-editor-node-forms.md)
- [07 - Edges and lineage](07-editor-edges-and-lineage.md)
- [12 - Standards](12-standards-and-use-cases.md)
