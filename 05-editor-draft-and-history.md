# 05 - Draft workflow and History

**Who:** Editor, Admin, Superadmin

---

## Purpose

Edits stay in a **local draft** until you confirm **Save draft**. **Apply** in a form only stages that draft. **History** lists saved revisions so you can restore an earlier graph. Restoring does not roll scope tags back.

## Who

Editors, Admins, and Superadmins. Viewers do not see History, Save, or Revert.

## When to use

| Goal | Action |
|------|--------|
| Keep work in this browser | Edit and **Apply**. Watch the dirty badge. |
| Show the change to everyone | **Save draft**, read the review, **Confirm** |
| Throw away unapplied form fields | **Close** the form tab (confirm if it asks) |
| Throw away the staged draft | **Revert** |
| Pull the latest saved graph | **Refresh** (confirm if the draft is dirty) |
| Undo a saved graph change | **History → Restore** |

Save is on Chart, Table, and History. It is not on Monitor.

---

## Step by step

### Apply, save, revert, refresh

1. Open a node from Chart or Table with **Edit**, or use **+** to create one ([06](06-editor-node-forms.md)).
2. Change fields. **Apply** writes them into the local draft and closes that form tab. **Ctrl+S** applies and leaves the tab open. **Ctrl+Enter** applies and closes. Nothing is published yet.
3. Watch the save status: **dirty** means the draft differs from the server; **saving** while a save runs; **clean** with a last-saved time when it matches.
4. Click **Save draft**. The **Review draft** modal lists added, updated, and deleted nodes and schemas, with old → new on each changed field. Scope tags show as **+** / **−** chips on the node row. Connection and Depends-on edits show as Source tables, Target tables, Schedule, or Frequency on the node. A save that only restamps “last updated” does not open a real change.
5. If form tabs still have unapplied edits, the review says they are **not included**. Apply those tabs first when those edits should ship.
6. **Confirm** writes the whole draft: graph changes, then scope assignments. **Cancel**, Esc, or the backdrop write nothing.
7. **Revert** discards the unsaved draft, including staged scope tags.
8. **Refresh** reloads the server graph. If the draft is dirty, confirm that local edits will be dropped.
9. Graph warnings in the detail header mean a broken reference. Fix the form before treating the graph as trustworthy.

Leaving the page with unapplied form tabs raises the browser’s leave warning.

<p class="shot-pending">Screenshot pending — assets/draft-review.png: Review draft modal before Confirm</p>

### Browse and restore a revision

1. Open **History** (`/?view=history`).
2. Read the timeline: revision number, actor, time, Save or Restore, and a short summary. The **current** revision is marked.
3. Expand a save that stored a review. The body matches the Save draft review (nodes, edges, schemas, scope chips). Older revisions without a stored review stay as a one-line summary.
4. **Reload** fetches the list again.
5. **Restore** an older revision and confirm. The note says scope tags are not restored. A dirty draft is called out because restore loads that snapshot as the new current graph.
6. Restore **creates a new revision**. The list is append-only.

<p class="shot-pending">Screenshot pending — assets/history-review.png: History row expanded to the field diff, with Restore</p>

## Form / field standards

Apply, then Save, when other people need the change. Revert when the draft should never publish. Confirm a restore only when you mean to replace the current graph and leave today’s scope tags in place.

## Edge cases

- **Viewer** — History and edit controls are hidden.
- **Failed save** — an error toast; the draft stays dirty.
- **Graph saved, tags failed** — the review stays open and says the graph is already saved. Retry the tag write from there.
- **Tags only** — Save still writes a History row so the tag change has a revision.
- **Restore plus a dirty draft** — the confirmation warns you. Cancel, save, or copy values out first.
- Concurrent editing — the last successful save wins. Refresh if the graph looks stale.

## Related

- [06 - Node forms](06-editor-node-forms.md)
- [04 - Chart and Table](04-chart-and-table.md)
- [13 - FAQ](13-faq-and-troubleshooting.md)
