# 05  -  Draft workflow and History

**Who:** Editor, Admin, Superadmin

---

## Purpose

Edits in Chart/Table stay in a **local draft** until you save. **History** lists saved revisions so you can restore an earlier snapshot without deleting the audit trail.

## Who

Editors, Admins, and Superadmins. Viewers do not see History or Save/Revert controls.

## When to use

| Goal | Action |
|------|--------|
| Keep work in progress locally | Edit freely; watch dirty badge |
| Publish to the shared workspace | **Save draft** |
| Throw away unsaved edits | **Revert** |
| Pull latest saved graph | **Refresh** (confirm if dirty) |
| Undo a bad saved change | **History → Restore** |

---

## Step by step

### Save, revert, refresh

1. Open Chart or Table and edit a node or edge (Edit tab).
2. Watch the save status badge: **dirty** means unsaved local changes; **saving** while a save runs; **clean** with last-saved time when synced.
3. Click **Save draft** to persist. Success and error toasts confirm the result.
4. To discard unsaved work, **Revert**. Confirm if prompted.
5. To reload the server’s current graph, **Refresh**. If the draft is dirty, confirm that you will lose local edits.
6. If the editor panel shows **graph warnings** (orphan or invalid edges), fix them before treating the graph as trustworthy  -  warnings often mean a broken reference after delete or a bad edge.

Saves use incremental graph sync under the hood. You do not choose sync vs full replace in normal UI use.

### Browse and restore a revision

1. Open **History** (`/?view=history`).
2. Review the list: revision number, timestamp, actor, action, summary. The **current** revision is highlighted.
3. Click **Reload** on the list if you need a fresh fetch.
4. Choose an older revision and **Restore**.
5. Confirm the dialog  -  especially if you have an unsaved draft (restore would overwrite it by loading the restored snapshot as the new current state).
6. Restore **creates a new revision**; history is append-only (nothing is deleted from the list).

<p class="shot-pending">Screenshot pending — assets/history-restore.png: History list with restore confirmation</p>

## Form / field standards

N/A  -  use Save/Revert/Refresh deliberately; do not leave dirty drafts overnight if others depend on the shared graph.

## Edge cases

- **Viewer**  -  History and edit controls are hidden.
- **Failed save**  -  toast error; draft stays dirty; fix the issue and save again.
- **Restore + dirty draft**  -  confirmation warns you; cancel if you still need the local edits (save first, or copy values out).
- Concurrent editing  -  last successful save wins; refresh if the graph looks stale.

## Related

- [06  -  Node forms](06-editor-node-forms.md)
- [04  -  Chart and Table](04-chart-and-table.md)
- [13 - FAQ](13-faq-and-troubleshooting.md)

