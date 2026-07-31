# 13 - FAQ and troubleshooting

**Who:** Everyone

---

## Purpose

Fast answers for common confusion. For longer procedures, use the playbooks in [12](12-standards-and-use-cases.md) or the feature chapters.

## Who

Everyone.

## When to use

Something looks wrong or a control is missing.

---

## Access

**I signed in but I don’t see Monitor/Chart.**  
You are not a workspace member. Submit an access request and wait for Admin approval ([01](01-getting-started.md), [11](11-admin.md)).

**Who can approve me?**  
Admin or Owner on `/admin/users`.

---

## Monitor

**What does “No lineage link” mean?**  
The CML id is not mapped to a node. An Editor must set the Livyatan job `link` (`…/jobs/{id}`) and Save ([09](09-editor-job-monitoring.md)).

**Times look wrong.**  
Table Start/End and the timeline axis use **WIB (Asia/Jakarta)**. Job payload timestamps are UTC ISO 8601.

**I can’t change poll interval to 5s/10s.**  
Only auto-refresh **on/off** at **30 seconds** exists today. Interval picker is **not available yet**.

**I clicked a row but the Execution filter didn’t fill.**  
Expected. Row click opens the detail panel only. Type the execution id in filters or use a `?execution=` deep link ([03](03-monitor.md)).

**Timeline shows big blocks, not individual runs.**  
You are zoomed out (aggregation). Use Zoom presets or Alt+scroll to zoom in; hover blocks for status breakdown.

**Bars didn’t disappear while refreshing.**  
Soft refresh  -  expected. Controls may show a background refresh state.

---

## Schedule / timeline

**No expected bars for my job.**  
Check frequency: adhoc/realtime have none; dependent needs a scheduled prerequisite; timed jobs need valid run times saved ([08](08-editor-schedule-standards.md)).

**Dependent bars didn’t move when the master ran late/adhoc.**  
By design. Dependent expecteds follow the **master schedule slots**, not orphan/adhoc master executions.

**I acknowledged a miss  -  what should I see?**  
Acknowledged-missed styling (gray dotted) and Overview audit with actor email ([10](10-editor-acknowledge.md)).

---

## Editing

**My edits vanished after Refresh.**  
Unsaved draft was discarded. Save before refresh, or restore from History if a bad save was already committed ([05](05-editor-draft-and-history.md)).

**I don’t see History or Edit.**  
Your role is Viewer ([02](02-roles-and-permissions.md)). Ask an Admin for Editor.

**Graph warnings appear.**  
Orphan or invalid edges  -  fix Source/Target/Depends-on or delete bad edges, then Save ([07](07-editor-edges-and-lineage.md)).

---

## Monitoring instrumentation

**Events never show up.**  
Check: Telegram `text` is JSON-only; same `execution_id` for the run; webhook/ops health ([09](09-editor-job-monitoring.md)). If the webhook itself is down, ask a FlowScope maintainer / ops.

**No Open runtime link.**  
Send `engine_id` on START (not `host` in new helpers).

**Stuck on running.**  
Missing FINISHED/FAILED from the helper. Fix the helper; Editor may acknowledge Failed/Succeeded as close-out ([10](10-editor-acknowledge.md)). Second try → 409.

---

## Not available yet (do not expect)

- Configurable poll intervals (5s / 10s / 60s)
- Hide-on-scroll navbar / stable icon-only chrome polish
- Title Case node type labels / chart family visuals
- Live event feed sidebar (detail Events tab is the current event UI)

---

## Form / field standards

N/A

## Screenshot

N/A

## Related

- [01 - Getting started](01-getting-started.md)
- [03 - Monitor](03-monitor.md)
- [09 - Job monitoring](09-editor-job-monitoring.md)
- [10 - Acknowledge](10-editor-acknowledge.md)
- [11 - Admin](11-admin.md)
- [12  -  Standards and playbooks](12-standards-and-use-cases.md)

