# 08 - Schedule standards

**Who:** Editor, Admin, Superadmin

---

## Purpose

Refresh frequency and run times tell Monitor when a **timed** job was expected to run. Set them in the job form so missed slots and late detection stay meaningful.

## Who

Editors (especially job authors), Admins, Superadmins.

## When to use

- Configuring when a job is expected to run
- Modeling a job that runs only after another job
- Explaining why the timeline shows expected bars, or only real runs

Schedule times in the form and on the timeline are **WIB (Asia/Jakarta)**.

---

## Frequency modes

| Frequency | What the timeline shows | Form UI |
|-----------|-------------------------|---------|
| hourly, daily, weekly, monthly, quarterly, yearly | Expected, missed, and actual bars from the generated cron | Run-time fields, a human preview, and a muted cron line |
| dependent | **Real runs only.** No calculated expected bar. A Depends-on chain can show a color stroke in the timeline name column. | **Depends on** only |
| adhoc | Real runs only | Short mode hint |
| realtime | Real runs only | Short mode hint |

Expected slots are filled for the current WIB day through the timeline’s near horizon (end of today plus 12 hours on the default range). Changing cron does not back-fill expected bars onto earlier days in the picker.

---

## Step by step

### Configure a timed job

1. Open the job form → **Schedule** ([06](06-editor-node-forms.md)).
2. Choose a timed frequency (hourly / daily / weekly / monthly / quarterly / yearly).
3. Fill the frequency-specific fields:
   - **Hourly** — minute past the hour only
   - **Daily and longer** — one or more `HH:mm` run times (WIB)
   - **Weekly** — day of week required
   - **Monthly / quarterly** — day of month required
   - **Yearly** — month and date required
4. For several times in a day, keep **at least 60 minutes** between them.
5. Read the human preview. Cron is generated when the draft is saved (hour-list, or pipe-joined when the minutes differ).
6. **Apply**, then **Save draft** and confirm the review.
7. Open Monitor → Timeline and look for expected bars on that job. The job appears on the Gantt after it has reported at least once.

<p class="shot-pending">Screenshot pending — assets/node-form-schedule-multi.png: Several WIB run times on a timed job</p>

### Configure a dependent job

1. Set frequency to **dependent**.
2. In Schedule, pick **Depends on** (the prerequisite job).
3. This mode has no Schedule or Cron cards.
4. **Apply**, then **Save draft**. Save persists the `job_dependency` edge.
5. On the timeline, read that job’s **real runs**. Do not look for a purple or calculated expected bar that follows the master’s slots. Row order still follows Depends on, upstream first.

<p class="shot-pending">Screenshot pending — assets/node-form-depends-on.png: Depends on picker for a dependent job</p>

### Verify on the timeline

1. Open Timeline ([03](03-monitor.md)). Default range is the last 3 WIB days through the end of today plus 12 hours.
2. For a **timed** job, compare expected bars with actual runs. A slot with no matching run becomes **missed** after the grace window.
3. Expected and missed bars use a **20-minute** minimum width so a slot stays visible. Running and finished bars use actual duration. The miss grace itself is longer than that visual width.
4. For a **dependent** job, confirm the prerequisite and the real runs. A chain of two or more visible jobs can show one color stroke beside the names.
5. Bars stay visible while data reloads. The Last refresh chip shows **Refreshing…**.

<p class="shot-pending">Screenshot pending — assets/monitor-timeline.png: Expected and actual bars for a timed job</p>

## Form / field standards

| Rule | Detail |
|------|--------|
| Timezone | WIB for every schedule time in the form |
| Multi run-times | At least 60 minutes apart |
| Dependent | Pick a prerequisite so the Depends-on edge is saved |
| Leaving dependent | Save clears `job_dependency` |
| Cron | Built from the form. Day-to-day editing does not hand-edit cron. |

## Edge cases

- Treating form times as UTC shifts the expected bars.
- A dependent job with no bars in the window may still be on the timeline if it has reported before. Click the name for job detail.
- No expected bars on a timed job: blank or invalid run times, the job has never reported (so it is not on the Gantt yet), or you are looking at a past day that was never materialised after a cron edit.
- Adhoc and realtime jobs do not get expected bars.

## Related

- [06 - Node forms](06-editor-node-forms.md)
- [07 - Edges and lineage](07-editor-edges-and-lineage.md)
- [03 - Monitor](03-monitor.md)
- [10 - Acknowledge](10-editor-acknowledge.md)
