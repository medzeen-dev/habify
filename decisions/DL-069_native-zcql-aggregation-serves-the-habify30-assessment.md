---
dl: 69
title: "Native ZCQL aggregation serves the habify30 assessment/reflection dashboard at the expected data volumes. No Catalyst Function aggregation layer is needed for this project."
status: active
supersedes: []
superseded_by: []
---
# DL-069

## Decision

Native ZCQL aggregation serves the habify30 assessment/reflection dashboard at the expected data volumes. No Catalyst Function aggregation layer is needed for this project.

## Context

An open question existed whether ZCQL's native `GROUP BY`/`AVG`/`SUM`/`COUNT` could carry an assessment or reflection dashboard, or whether a Catalyst Function would need to perform aggregation server-side. The question was answered empirically in a 2026-07-15 Cowork probe session (Development only, synthetic data). Probe tables `zz_probe_assessment` (42 rows, 7 user columns) and `zz_probe_cohort` (3 rows, 6 user columns) were used.

## Decision

Native ZCQL aggregation is the query layer for the habify30 dashboard. Empirically confirmed:

- **Supported:** `COUNT`, `AVG`, `SUM`, `MIN`, `MAX`, `GROUP BY` (single-axis and multi-axis), `HAVING`, `ORDER BY`, `LIMIT … OFFSET …`, correlated subqueries (`WHERE col IN (SELECT …)`).
- **`COUNT(DISTINCT col)` silently ignores DISTINCT** — returns a plain COUNT. Distinct-participant counts must use a subquery or `GROUP BY`-then-count. This is a correctness trap, not a missing function.
- **No free JOINs:** ZCQL joins only tables connected by a predefined foreign-key relationship. FK relationships cannot be created via MCP. Workaround: correlated subqueries (confirmed working). Sufficient for habify30's query patterns.
- **OLAP mode unavailable:** `Execute_Query` with `OLAP: true` returns "OLAP System is not available" on the current plan. Normal mode handles all dashboard queries at habify30's volumes.
- **`Insert_Rows` caps at 200 rows per call.** Bulk load requires batched calls. Not a constraint for habify30's small cohort writes.
- **Schema provisioning:** User columns cannot be created via MCP (`Create_Table` creates only system columns; no add-column tool exists; ZCQL DDL not supported). Columns must be created once via the Catalyst console.
- **Cohort creation via MCP:** Confirmed working end-to-end — cohort master rows and participant rows written in one flow. Pre-existing schema required.

At habify30's expected volumes (small cohorts, a few hundred to low thousands of rows total), native ZCQL aggregation is accurate and sufficient.

## Rationale

Habify30's data volumes are small. Catalyst is designed for this class of workload. The `COUNT(DISTINCT)` pitfall is documented and avoidable via standard SQL workarounds. The absence of free JOINs is addressed by correlated subqueries, which are verified working. The scale-test (latency curve vs. row count) was deliberately dropped: real volumes are small and high scale is Catalyst's core product, not worth re-measuring for this project.

## Consequences

- `Catalyst_Platform_Capabilities.md` Cluster B (ZCQL / Data Store) records empirical measurements.
- Schema setup (user columns) must be performed once in the Catalyst console before any data pipeline work — a known manual step, not a blocker.
- The `COUNT(DISTINCT)` pitfall must be verified in any query counting distinct participants.
