---
name: habify30-catalyst-probing
description: "Structure recurring, empirical probes of Zoho Catalyst capabilities for Habify30 via the Catalyst MCP. Use before running any Catalyst measurement (ZCQL aggregation limits, Data Store write reliability, MCP-driven cohort creation). Enforces Development-only access, dummy-data conventions, and a never-guess-IDs discipline, and defines the finding format for capability reports. Survives beyond a single demo — it is the harness for the later real dashboard build, not just for one probe."
---

# Habify30 Catalyst Probing

Purpose: turn "we assume Catalyst can do X" into "we measured that Catalyst does
X up to limit Y". *Reality beats elegance.* Nothing that is measurable is assumed.

## 1. Verified environment facts (re-read, never hardcode blindly)

These were read from the MCP, not guessed. Re-verify with `List_All_Organizations`
and `List_All_Projects` at the start of every session — IDs can change if the
project is re-cloned.

| Fact | Value (verified 2026-07-15) | How to re-read |
|---|---|---|
| Org name / id | `Matthias` / `20116360871`, EU DC (`catalyst.zoho.eu`) | `List_All_Organizations` |
| Project | `Habify30` / `22671000000014048` | `List_All_Projects` |
| Development env id | `22671000000014065` (env_type 3, **is_default: false**) | `List_All_Projects` → `env_details` |
| Production env id | `22671000000016011` (**is_default: true** ← danger) | `List_All_Projects` → `env_details` |
| DB / timezone | `SINGLE_DB` / `Europe/Berlin` | `Get_Project_By_Id` |
| Real dev tables — DO NOT TOUCH | `UserRecovery`, `AccessControl`, `FormSubmissions` | `List_All_Tables` |

## 2. MCP access convention (non-negotiable)

- **Environment is a per-call header.** Every Catalyst MCP tool takes a
  `headers.Environment` field. Production is `is_default: true`, so **omitting or
  mis-setting it targets Production.** Set `Environment: "Development"` on
  *every* call — reads and writes alike.
- **Production is never written.** Read Production only on explicit instruction;
  never read-and-write. Before any write, confirm the call carries
  `Environment: "Development"`.
- **Never guess an ID, count, table name, or number — always read it via MCP.**
  In prior sessions two guessed values (an RI number, a row count) slipped
  through and were caught only by attention. Every id / row count / table name
  that appears in a report is read from a tool response and cited with its
  source call.
- **State uncertainty openly.** The report writes the promise that is true, not
  the one that sounds good. "Tested to 5,000 rows, not beyond" beats "scales well".

## 3. Dummy-data convention

- All probe tables are throwaway and prefixed **`zz_probe_`** so they are
  obviously disposable and easy to find/clean up.
- Only synthetic data — no real cohort, participant, or customer data.
- At the end of every probing session, list each created/filled table
  individually: name, table_id, purpose, row count. Matthias cleans up.
- Prefer `Truncate_Table` between load stages (keeps schema, drops rows);
  `Delete_Table` only when the table itself is no longer needed.

## 4. Loading data at scale

- `Insert_Rows` accepts an array of row objects per call — good for hundreds of
  rows per call, but many calls for tens of thousands. Batch and track cumulative count.
- For large loads (20k+), prefer `Create_Bulk_Write_Job` from an uploaded CSV
  (`file_id`) when insert-call volume becomes the bottleneck. Poll with
  `Get_Bulk_Write_Job_Status`.
- Measure and record the actual row count after loading (`SELECT COUNT(ROWID)`),
  never the intended count.

## 5. ZCQL test patterns (reusable checklist)

Run each class at each load stage; record wall-clock latency per query.

1. **GROUP BY + COUNT/AVG/SUM** — e.g. average value per dimension per cohort
   (`GROUP BY pid, dimension`). The core of any assessment dashboard.
2. **Multi-axis GROUP BY** — e.g. dimension × wave (baseline→end change per
   dimension). Tests whether multi-dimensional aggregation is native.
3. **JOIN + aggregate** — assessment answers joined to a cohort master table,
   then aggregated. Tests whether enriched analysis is native or needs post-processing.
4. **Latency curve** — for each class at each stage, log response time. The goal
   is the *curve* (where does it slow down, at what row count / complexity), not a
   single number.

Known pitfalls the report must address explicitly:

- **ZCQL feature gaps** — are all needed aggregate functions, nested/multi-axis
  GROUP BY, and JOIN types supported? A missing function is a core finding
  ("native aggregation is not enough because X is missing"), not something to gloss over.
- **Result-set / row limits** — does ZCQL cap returned rows per query (forcing
  pagination)? If so, from what number, and does it force function-side post-processing?
- **Timeout behaviour** — are there query timeouts? At what load?
- ZCQL notes: use `ROWID` as the row key; `COUNT(ROWID)` for counts.
  `Execute_Query` has an `OLAP` flag — test analytical queries both with and
  without it and record any difference.

## 6. Finding format (per measurement point)

For each measurement point, produce exactly:

- **One clear Yes/No answer** to the single question that drives the architecture.
- **The measured number** that backs it (with the source call).
- **The limit at which it tips** ("native up to X rows, then Function-side").

Example: "Native ZCQL aggregation carries an assessment dashboard — YES, up to
~N rows / complexity C; beyond that <observed failure mode>."

## 7. This skill is sharpened during measurement

Whatever shows up during probing as a useful pattern or a stumbling block gets
written back here. The skill is better at the end of a session than at the start.
Append findings under a dated "Field notes" section rather than overwriting the
patterns above.

## Field notes

### 2026-07-15 — first empirical touch (session "Catalyst-Fähigkeiten messen")

**Blocking discovery — MCP cannot provision Data Store schema.**
- `Create_Table` creates only an empty shell with the 4 system columns
  (`ROWID`, `CREATORID`, `CREATEDTIME`, `MODIFIEDTIME`). Evidence: `List_All_Columns`
  on a freshly created table.
- There is **no create/add-column tool** in the MCP. Only `Update_Column`
  (needs an existing `column_id`), `Delete_Column`, `Get_Column_By_Id`,
  `List_All_Columns`.
- ZCQL supports **no DDL**: `CREATE TABLE (cols…)` → "Syntax error";
  `ALTER TABLE … ADD COLUMN` → "Syntax error".
- `Insert_Rows` with unknown columns → `INVALID_INPUT` ("Invalid input value for
  column name"). Empty-object rows → `INVALID_OPERATION` ("Empty row cannot be
  updated"). So you cannot insert *any* row until a user column exists.
- **Consequence:** user columns must be created via the Catalyst console UI (or the
  raw Admin REST API, which this MCP does not wrap). Any data-volume / latency /
  cohort-write measurement is blocked until columns exist.

**ZCQL feature support (tested on system columns of empty `zz_probe_` tables — 0 rows,
so this proves parse/execute acceptance, NOT scaling).**
- Supported: `COUNT`, `AVG`, `SUM`, `MIN`, `MAX`, `COUNT(DISTINCT …)`,
  `GROUP BY` (single and multi-axis), `HAVING`, `ORDER BY`, `LIMIT … OFFSET …`,
  subqueries (`WHERE col IN (SELECT …)`).
- **JOIN limited:** arbitrary `INNER JOIN … ON a.x = b.y` → "No relationship
  between tables". ZCQL only joins tables connected by a **predefined foreign-key
  relationship**. Since FK columns can't be created via MCP either, joins can't be
  set up via MCP. Workaround for enriched analysis: correlated **subqueries**
  (confirmed working) instead of JOIN.
- **OLAP mode unavailable:** `Execute_Query` with `OLAP: true` → "OLAP System is
  not available" (likely plan/provisioning-gated). Analytical queries must run in
  normal mode.

**Not measurable this session (blocked by the schema gap):** latency-vs-rowcount
curve, empirical result-set/row caps, query timeout thresholds, real cohort-write
reliability. All require pre-existing user columns.

**Update (same session, after Matthias created the columns via the Catalyst console):**

Once user columns existed, writing + aggregating via MCP worked well. New measured facts:

- **`Insert_Rows` caps at 200 rows per call** ("Only 200 rows can be updated at
  once"). Empty rows and unknown columns still rejected. So bulk load = many calls.
- **Stratus (object store) is gated:** `Create_Bucket` → "User needs to be in
  session when accessing Stratus for the first time". Bulk-write-from-object needs
  a one-time interactive Stratus init in the console. No File Store upload tool is
  exposed either. ⇒ **Loading 20k–100k rows via MCP is impractical in this
  environment** (200-row insert cap + token cap on inline payloads + no bulk path).
  The latency-vs-rowcount curve therefore remains UNMEASURED.
- **Aggregation is correct on real data** (verified with fixed values c001=4, c002=2):
  `GROUP BY pid` → AVG 4.0000/2.0000, COUNT 18/18; two-axis `dimension, wave` → 18
  groups AVG 3.0000; total COUNT 36→42. `AVG` returns a 4-dp string.
- **`COUNT(DISTINCT col)` silently ignores DISTINCT** — returns plain COUNT
  (c001 with 1 distinct uid but 18 rows returned 18). Real correctness pitfall:
  distinct-participant counts must use a subquery/GROUP-BY-then-count, or a Function.
- **Subquery enrichment works and is correct** (`WHERE pid IN (SELECT pid FROM
  cohort WHERE seats<10)` → only c900, AVG 4.0000). This is the viable replacement
  for the unsupported free JOIN.
- **Cohort creation via MCP (Messpunkt 2) works end-to-end, first try, no rework:**
  wrote cohort master rows (c001/c002/c900) + a live-created cohort c900 with its
  own participants (u9001/u9002) in one flow. Caveat: schema (columns) must
  pre-exist; uid↔pid link is a shared text key, not an FK.

**Decision (2026-07-15):** the latency/scale test was deliberately dropped for
Habify30 — real data volumes are small and high scale is Catalyst's core product,
not worth re-measuring. The load-limit findings above still stand as facts about
the MCP; they just don't block the architecture here. The ZCQL latency-curve
patterns in section 5 remain valid for any future project where volume matters.

**Probe tables left behind (Development):**
- `zz_probe_assessment` — table_id `22671000000016076` — **42 rows** (7 user cols).
- `zz_probe_cohort` — table_id `22671000000019217` — **3 rows** (6 user cols).