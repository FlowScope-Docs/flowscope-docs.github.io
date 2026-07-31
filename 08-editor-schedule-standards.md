# 08  -  Schedule standards

**Who:** Editor, Admin, Owner

---

## Purpose

Refresh frequency and run times drive **timeline expected bars** on Monitor. Set them correctly so missed/late detection and dependent jobs stay meaningful.

## Who

Editors (especially job authors), Admins, Owners.

## When to use

- Configuring when a job is expected to run
- Modeling a job that runs only after another job’s schedule slots
- Explaining why the timeline shows (or does not show) expected bars

All schedule UI times are **WIB (Asia/Jakarta)**.

---

## Frequency modes

| Frequency | Expected bars on timeline? | Form UI |
|-----------|----------------------------|---------|
| hourly, daily, weekly, monthly, quarterly, yearly | Yes (from generated cron) | Run-time fields + human preview + muted cron |
| dependent | Yes  -  one expected per **upstream schedule slot** | **Depends on** only (no Schedule/Cron cards) |
| adhoc | No | Short mode hint |
| realtime | No | Short mode hint |

---

## Step by step

### Configure a timed job

1. Open the job → Edit → **Schedule**.
2. Choose a timed frequency (hourly / daily / weekly / monthly / quarterly / yearly).
3. Fill frequency-specific fields:
   - **Hourly**  -  minute past the hour only  
   - **Daily+**  -  one or more `HH:mm` run times (WIB badge)  
   - **Weekly**  -  day of week required  
   - **Monthly / quarterly**  -  day of month required  
   - **Yearly**  -  month + date required  
4. For multiple daily (or similar) times: add/remove run times with **at least 60 minutes** between times.
5. Confirm the human preview; cron is generated on save (hour-list or pipe-joined when minutes differ).
6. **Save draft**.
7. Open Monitor → Timeline and verify expected bars appear for that job.

<p class="shot-pending">Screenshot pending — assets/node-form-schedule-multi.png: Multi run-times for a daily job</p>

### Configure a dependent job

1. Set frequency to **dependent**.
2. In Schedule, pick **Depends on** (prerequisite job)  -  searchable picker.
3. Do not expect Schedule/Cron cards in this mode.
4. **Save draft** (persists `job_dependency`).
5. On Timeline: expect one bar per **master schedule slot**, not per late adhoc/orphan master execution.

### Verify on the timeline

1. Switch to Timeline Live or History ([03](03-monitor.md)).
2. Confirm expected vs actual; missed slots turn missed when the window has passed with no matching run.
3. Remember: expected/missed bars use a **20-minute minimum** visual width; dependents on unmet upstream slots start ~20m after the upstream expected time (not after the 30m miss window); real runs use actual duration.
4. Soft refresh keeps bars visible while data reloads.

<p class="shot-pending">Screenshot pending — assets/monitor-timeline-live.png: Expected vs actual bars for scheduled jobs</p>

## Form / field standards

| Rule | Detail |
|------|--------|
| Timezone | WIB for all schedule times |
| Multi run-times | ≥ 60 minutes apart |
| Dependent | Prerequisite required for a correct dependency edge |
| Leaving dependent | Clears `job_dependency` on save |
| Cron | Auto-built  -  do not hand-edit in normal workflow |

## Edge cases

- Assuming UTC in the form → wrong expected bars.
- Dependent bars not following a late master adhoc run → **by design** (master schedule slots only).
- No expected bars → adhoc/realtime, blank/invalid schedule, or dependent without a scheduled master.

## Related

- [06  -  Node forms](06-editor-node-forms.md)
- [07  -  Edges and lineage](07-editor-edges-and-lineage.md)
- [03 - Monitor](03-monitor.md)
- [10 - Acknowledge](10-editor-acknowledge.md)

<div class="chapter-nav">
<a href="#/">← Home</a>
<a href="#/09-editor-job-monitoring">Next: Job monitoring →</a>
</div>
