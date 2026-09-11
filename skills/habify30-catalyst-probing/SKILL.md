---
name: habify30-catalyst-probing
description: "Product-specific layer for empirical Catalyst probes on the Habify30 project — the verified project/environment/table IDs, the real tables that must never be touched, and the product decisions about what is worth measuring. The general discipline (environment per call, zz_probe_ dummy data, never guess an ID, three-line finding) lives in kado: KONV-catalyst, VF-catalyst-messen and its skill catalyst-messen; the measured platform behaviour in DOK-catalyst-mcp and, product-near, Catalyst_Platform_Capabilities.md. Load this together with catalyst-messen, before any Catalyst measurement for Habify30."
---

# Habify30 Catalyst Probing — product layer

Since 2026-09-11 the tool-general substance of this skill lives in `kado`:

| What | Where |
|---|---|
| Standing rules — `--dc eu`, environment on every call, `zz_probe_` dummy data, nothing guessed, records only aggregated, schema in the console | `kado/konventionen/catalyst-konventionen.md` (`KONV-catalyst` §1, §2, §7, §8, §9, §10) |
| The measurement procedure and its handgrips | `kado/verfahren/catalyst-messen/` (`VF-catalyst-messen`, skill `catalyst-messen`) |
| Measured platform behaviour — schema gap, 200-row insert cap, Stratus gate, ZCQL feature support, `COUNT(DISTINCT)`, JOIN, OLAP | `kado/dokumentation/referenz_catalyst-mcp.md` (`DOK-catalyst-mcp`) |
| Product-near measurements incl. Production gating (E5), concurrency (B8), cache headers (A5) | `Catalyst_Platform_Capabilities.md` in this repository |

This file keeps only what is specific to Habify30. It does not restate the rules; where it
seems to, the `kado` text wins (`KONV-kanon-dokumente` §5).

## 1. Verified environment facts (re-read, never hardcode blindly — `KONV-catalyst` §1, §8)

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
| Real dev tables — DO NOT TOUCH | six (verified 2026-09-10): `AccessControl` `22671000000014463`, `FormSubmissions` `22671000000014073`, `UserRecovery` `22671000000014832`, `PeerGroups` `22671000000051023`, `PeerSignups` `22671000000052005`, `CohortConfig` `22671000000053005` | `List_All_Tables` |

## 2. Production on this project

Production configuration cannot be written by any tool available to this project; the only
path is `Deploy to Production`, which was gated on 2026-09-10 (see
`Catalyst_Platform_Capabilities.md` E5). Every call names its environment
(`KONV-catalyst` §2); a write to Production, where it is possible at all, is an egress act
on explicit instruction (`VTR-catalyst` Dimension 5, DL-2026-018 rule 4). Production records
are never read as single rows (DL-2026-018 rule 3).

## 3. What is worth measuring here

**Decision (2026-07-15):** the latency/scale curve was deliberately dropped for Habify30 —
real data volumes are small and high scale is Catalyst's core product. The load-limit findings
stand as facts about the MCP (`DOK-catalyst-mcp`); they do not block the architecture. Any
future probe starts from the "not measured" list in `DOK-catalyst-mcp` and in
`Catalyst_Platform_Capabilities.md` (A7, B9), not from zero.

## Field notes (record of the measurements; platform-general findings are mirrored in `DOK-catalyst-mcp`)

### 2026-07-15 — first empirical touch (session "Catalyst-Fähigkeiten messen")

> **Correction note (2026-09-10, Capabilities B5):** the blocking finding below is
> **no longer true.** An add-column tool *does* exist in the MCP — `Create_Column`,
> addressed by table id — and it works. Two traps: omit the advertised `description`
> field (sending it fails the whole call with `PATTERN_NOT_MATCHED`), and send one
> column per call rather than batching. Evidence: `confirm_token` and
> `confirm_token_expiry` added to `PeerSignups` in Development, 2026-09-09 (DL-090).
> Provisioning columns is no longer a manual console step. The rest of the note —
> `Create_Table` yielding only the 4 system columns, ZCQL having no DDL — still holds.
> Authoritative source is `Catalyst_Platform_Capabilities.md` B5, not this note.

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

**Probe tables left behind (Development) — cleaned up; `List_All_Tables` on 2026-09-10
returned neither. Kept as a record of what the measurement cost:**
- `zz_probe_assessment` — table_id `22671000000016076` — **42 rows** (7 user cols).
- `zz_probe_cohort` — table_id `22671000000019217` — **3 rows** (6 user cols).