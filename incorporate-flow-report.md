# Incorporate Flow Report — Standalone Standard

**Public guidebook copy.** Canonical coworker docs live in this repo. The FlowScope app repo keeps an internal copy at `docs/INCORPORATE_FLOW_REPORT_STANDARD.md` — update both when the standard changes.

**What this is:** One self-contained file. Share **only this file** (plus the target job `.py`). No repo access required.

**Also see:** [09 — Job monitoring](09-editor-job-monitoring.md) for wiring the Livyatan job node and verifying runs in Monitor.

**Assumes:** The teammate’s environment already has your shared `lib` package with `lib.report_event`, `lib.spark_session`, and (for full migrate) `lib.set_timer` / `lib.get_list_partition` / `lib.write_*`.

**How to use with an AI**

1. Attach this file + the target job `.py` only.
2. Prompt something like:

> Follow INCORPORATE_FLOW_REPORT_STANDARD.md. Add FlowScope reporting to `<path/to/job.py>`. Default = light-touch (keep local helpers). Only full-migrate to `lib.*` if I explicitly say so.

3. One job `.py` per run.
4. After the AI finishes, run the Verification checklist at the bottom.

This file is the full job-instrumentation standard (header, events, snippets, checks). Do not depend on other docs or example jobs.

---

## Scope

### Default: light-touch

**Do**

- `import lib` then `lib.report_event(...)` — never `from lib.flow_report import …`
- `lib.spark_session(...)` — replace `bribrain.sparkSession` / raw `SparkSession.builder`
- Dual START comments + early `BACKDATE_*`
- Materialize `list_partition_process`; apply BACKDATE override at ds boundary
- Reuse `hive_schema` / `hive_table` in business write/read/validate/SHOW/Load messages
- Wrap main process in `try` / `except` (`spark.stop()` after FINISHED only — no `finally`)
- Load STEP: `row_count` + `progress_pct` after successful write
- DQ / write hard-stops **raise** → outer except emits `FAILED` (not WARNING + continue)
- Mirror important process-tracking `print`s with `INFO` / `WARNING` / `FAILED` (see [Print → report](#print--report-process-tracking)); keep the prints

**Do not change** (unless user asks full migrate)

- Local helpers (`set_timer`, `write_periodic`, `get_list_partition`, …)
- Transform / filter / join / agg / dedupe
- Validate + telegram / other notifications
- Write fallback structure (keep it; final failure must still `raise`)

### Full migrate (only if asked: “full migrate” / “migrate to `lib.*`”)

- `lib.set_timer` / `lib.get_timer`
- `lib.get_list_partition`
- `lib.write_*` (e.g. `lib.write_temp_periodic`)
- Early Extract `FAILED` when the job already hard-stops on empty source

### Do not invent or copy unrelated partition policies

- Do not add `[:7]` / reverse-sort caps unless that job already uses them
- Do not put decorative `~##` strings inside `report_event` messages

Ask once if light-touch vs full migrate is unclear, then stick to that mode.

---

## Required header

```python
import lib

spark = lib.spark_session("Job Name", "high")

lib.report_event(spark=spark, event_type="START")  # comment if adhoc/backdate
# lib.report_event(spark=spark, cml_job_id="...", event_type="START")  # adhoc/backdate

#  Backdate Parameters (optional) enable by setting both BACKDATE_START and BACKDATE_END
#=========================================
BACKDATE_START = None  # e.g. "2026-04-01"
BACKDATE_END = None    # e.g. "2026-04-01"

##             Parameters
#=========================================
hive_schema = "project_sv"
hive_table = "your_table_name"
print(f"{hive_schema}.{hive_table}")
```

Full migrate: insert `START_TIME = lib.set_timer()` immediately before START.

Always pass `spark=` on the first `report_event`. Later events can omit `spark=` / `cml_job_id=`.

Declare `BACKDATE_*` here. At ds boundary, only apply the override — do not redeclare the constants.

---

## Event types (closed set)

Use only: `START` | `STEP` | `INFO` | `WARNING` | `FINISHED` | `FAILED`.

| Rule | Detail |
|------|--------|
| `STEP` needs `step=` | PascalCase: Extract, Process, Load, Transform, Validate, … |
| Never `step="INFO"` / `step="WARNING"` | Use matching `event_type` |
| Load | Always pass `row_count=` and `progress_pct=` |
| FINISHED | `row_count=total_row_count` (sum of successful Loads) |
| Hard-stops | raise → FAILED; not WARNING + continue for DQ / write failures |
| Messages | Plain text; no Markdown, emojis, or `##~` wrappers |

Typical spine for a daily `ds` loop:

`START` → Extract (boundary) → INFO (partition list) → per-ds Process → per-ds Load → FINISHED → `spark.stop()`  
On error: FAILED + re-raise. Do **not** use `finally: spark.stop()` (CML treats clean Spark shutdown after raise as success).

---

## Print → report (process tracking)

Jobs already use `print` for session/log tracking. When incorporating Flow Report, **keep those prints** and **also** emit a matching `lib.report_event` so FlowScope sees the same progress/debug signals.

Use prints (and their report mirrors) especially for:

- Marking progress at major stage boundaries (so a long run shows where it is)
- Making failures easier to locate (which stage stopped)
- Surfacing important parameter values early (`ds`, `ds_month`, table FQN, partition list) before heavy work
- Other need-to-know facts (row counts, DQ uniqueness outcomes, skip reasons)

**Rule:** Keep the `print`. Add a matching `report_event` immediately after (or next to) it.

| Print kind | Event | Notes |
|------------|-------|-------|
| Stage / progress marker (extract boundary, start of heavy block, post-write) | `INFO` | Plain message mirroring the print text |
| Important variable values (`ds`, `ds_month`, FQN, partition list, counts) | `INFO` | Prefer `f"..."`; truncate huge lists |
| Recoverable anomaly already printed | `WARNING` | Only if the job continues |
| Hard-stop reason already printed | `FAILED` + `error_message=` | Then raise / exit as today |

**Do not** mirror:

- Commented-out prints
- Pure decorative separators / timer-only noise (`get_timer()` alone, `~## ... ##~`) when a STEP already covers that phase
- Prints that duplicate an event already emitted for the same fact (e.g. bare `print(ds)` right after Process STEP `Processing ds={ds}`)

Do **not** remove existing prints; do **not** replace them with report-only.

### Examples

Partition list (canonical):

```python
print("list_partition_process:", list_partition_process)
lib.report_event(
  event_type="INFO",
  message=f"list_partition_process={list_partition_process}",
)
```

Important vars / counts:

```python
print(last_ds_fitur, "-", last_ds_source)
lib.report_event(
  event_type="INFO",
  message=f"ds boundary: last_ds_fitur={last_ds_fitur} last_ds_source={last_ds_source}",
)

print("df_list_fitur cnt *:", df_list_fitur_cnt)
lib.report_event(
  event_type="INFO",
  message=f"df_list_fitur cnt *={df_list_fitur_cnt}",
)

print("service_id UNIQUE")
lib.report_event(event_type="INFO", message="service_id UNIQUE")
```

Hard-stop print paired with FAILED:

```python
print("No source partitions found.")
lib.report_event(
  event_type="FAILED",
  step="Extract",
  error_message="No source partitions found",
)
raise RuntimeError("No source partitions found")
```

---

## Daily partition checklist

1. `import lib`
2. `lib.spark_session` + dual START + early `BACKDATE_*`
3. `hive_schema` / `hive_table`
4. Extract STEP (`Setting ds boundary`, `progress_pct=5`)
5. Build `list_partition_process` from **existing** date logic (do not change which dates run)
6. BACKDATE override + INFO + `total_row_count = 0`
7. Scan live `print`s; add missing INFO/WARNING mirrors for important ones (see [Print → report](#print--report-process-tracking))
8. `try`: loop Process/Load; success diagnostics; FINISHED; `spark.stop()`
9. `except`: FAILED + `raise` (no `finally`)

### Extract + BACKDATE override

```python
lib.report_event(
  event_type="STEP",
  step="Extract",
  message="Setting ds boundary",
  progress_pct=5,
)

# Build from the job's existing date logic
list_partition_process = [
  partition.strftime("%Y%m%d")
  for partition in pd.date_range(start=last_ds_fitur, end=last_ds_source, freq="D")
]

if BACKDATE_START and BACKDATE_END:
  list_partition_process = [
    partition.strftime("%Y%m%d")
    for partition in pd.date_range(start=BACKDATE_START, end=BACKDATE_END, freq="D")
  ]

print("list_partition_process:", list_partition_process)
lib.report_event(
  event_type="INFO",
  message=f"list_partition_process={list_partition_process}",
)

total_row_count = 0
```

### Early empty-source hard stop (before try)

```python
if len(source_partitions) == 0:
  print("No source partitions found.")
  lib.report_event(
    event_type="FAILED",
    step="Extract",
    error_message="No source partitions found",
  )
  raise RuntimeError("No source partitions found")
```

### Light-touch loop (write + optional fallback → raise)

```python
try:
  for i, today in enumerate(list_partition_process):
    ds = today
    lib.report_event(
      event_type="STEP",
      step="Process",
      message=f"Processing ds={ds}",
      progress_pct=int(10 + 80 * i / max(len(list_partition_process), 1)),
    )

    # ... existing transform ...

    # DQ example:
    # raise ValueError("service_id NOT UNIQUE")

    write_succeeded = False
    try:
      write_periodic(df, hive_schema, hive_table, ds)
      write_succeeded = True
    except Exception:
      try:
        # existing fallback write ...
        write_succeeded = True
      except Exception:
        print(f"ds {ds} unavailable")
        raise ValueError(f"write unavailable for ds={ds}")

    if write_succeeded:
      df_count = (
        spark.read.table(f"{hive_schema}.{hive_table}")
        .filter(F.col("ds") == ds)
        .count()
      )
      total_row_count += df_count
      lib.report_event(
        event_type="STEP",
        step="Load",
        message=f"Loaded {hive_schema}.{hive_table} for ds={ds}",
        row_count=df_count,
        progress_pct=int(10 + 80 * (i + 1) / max(len(list_partition_process), 1)),
      )

  lib.report_event(
    event_type="FINISHED",
    message=f"Job completed — {len(list_partition_process)} partition(s)",
    row_count=total_row_count,
  )
  spark.stop()
except Exception as e:
  lib.report_event(
    event_type="FAILED",
    step="Process",
    error_message=str(e),
  )
  raise
```

### Full-migrate loop (one write path)

```python
total_row_count = 0
try:
  for i, ds in enumerate(list_partition_process):
    lib.report_event(
      event_type="STEP",
      step="Process",
      message=f"Processing ds={ds}",
      progress_pct=int(10 + 80 * i / max(len(list_partition_process), 1)),
    )
    # transform ...
    lib.write_temp_periodic(spark, df, hive_schema, hive_table, ds)
    df_count = (
      spark.read.table(f"{hive_schema}.{hive_table}")
      .filter(F.col("ds") == ds)
      .count()
    )
    total_row_count += df_count
    lib.report_event(
      event_type="STEP",
      step="Load",
      message=f"Loaded {hive_schema}.{hive_table} for ds={ds}",
      row_count=df_count,
      progress_pct=int(10 + 80 * (i + 1) / max(len(list_partition_process), 1)),
    )

  lib.report_event(
    event_type="FINISHED",
    message=f"Job completed — {len(list_partition_process)} partition(s)",
    row_count=total_row_count,
  )
  spark.stop()
except Exception as e:
  lib.report_event(event_type="FAILED", step="Process", error_message=str(e))
  raise
```

### Other shapes

| Shape | Approach |
|-------|----------|
| Single-shot / non-loop | START → phase STEPs → FINISHED/FAILED; skip per-ds Process/backdate unless useful |
| Already partially instrumented | Fill gaps only; never double START/FINISHED |

---

## Anti-patterns

- New `event_type` values outside the closed set
- Leaving `bribrain` / raw Spark builder after incorporating
- Rewriting local write/partition helpers without an explicit full-migrate ask
- Counts only in `message` without `row_count=`
- Load event when write failed
- WARNING + continue for DQ / write hard-stops
- Swallowing exceptions after FAILED
- Huge INFO payloads
- Transform/Validate STEP spam for every small block
- Copying unrelated jobs’ `[:7]` / reverse-sort caps into simple `date_range` jobs
- Leaving important progress / count / DQ prints without a matching report event
- Dumping timer-only or decorative prints as INFO spam
- Removing or replacing prints with report-only (keep both)
- `finally: spark.stop()` after FAILED + raise (masks CML failure — stop only after FINISHED)

---

## Verification

1. `python -m py_compile` on the target file
2. One START after session; FINISHED on success; FAILED in except; early FAILED only for hard-stops
3. Load has `row_count` + `progress_pct`; FINISHED uses `total_row_count`
4. Dual START comments + early `BACKDATE_*`; `lib.spark_session` in use
5. Transform logic unchanged aside from indentation / FQN → `hive_*`
6. `spark.stop()` after FINISHED only (no `finally`)
7. No accidental partition-cap changes / helper migration (unless full migrate) / telegram deletion
8. DQ / write hard-stops raise (no WARNING-continue on those paths)
9. Important live prints have a matching report event (or are intentionally skipped per the do-not-mirror list)

Remind the owner: set `cml_job_id` and/or `BACKDATE_*` for adhoc/backdate runs.

