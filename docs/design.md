# IoT Predictive Maintenance Lakehouse — Design

## Problem
A fleet of 40 industrial machines emits sensor telemetry (temperature,
vibration, pressure, RPM). Maintenance is currently reactive: failures
are discovered when machines stop. This platform enables (a) governed
queryable history, (b) near-real-time visibility into machine health,
(c) a foundation for anomaly-based maintenance alerts.

## Data source
Simulated. Real fleet telemetry is not publicly available at scale;
simulation with documented, parameterized failure modes (duplicate,
late-arrival, malformed payload rates) is a more honest test of the
pipeline than a scrubbed public dataset. Generator lives in
src/generator/produce.py.

## SLOs
| SLO                  | Target                                        | Verified by                |
|----------------------|-----------------------------------------------|----------------------------|
| Streaming freshness  | Event visible in gold within 5 min (Azure)    | Timestamp diff, Week 5     |
| Batch freshness      | Hourly triggered run under 10 min             | Lakeflow Jobs history      |
| Data quality         | ≥99% well-formed pass; 100% malformed to quar | ops.run_metrics            |
| Recoverability       | Any day rebuildable from bronze               | Backfill runbook, Week 3   |
| Cost                 | Azure sprint under $40                        | Cost Management screenshot |

## Scope
In: simulated telemetry with chaos, batch reference data, medallion
lakehouse on Delta + Unity Catalog, streaming + batch ingestion, SCD2
dimension, quality gates with quarantine, orchestration, CI/CD via
Asset Bundles, one BI dashboard, monitoring, Terraform for Azure sprint.

Out: real device connectivity, Kubernetes, multi-region, streaming ML
inference, always-on infrastructure after demo.

## Event contract (schema v1)
```json
{
  "event_id": "uuid",
  "machine_id": "M-017",
  "event_ts": "2026-07-06T14:03:22",
  "temp_c": 66.41,
  "vibration_mm_s": 2.113,
  "pressure_kpa": 402.7,
  "rpm": 1447,
  "schema_version": 1
}
```

## Deviations from the build guide
- Schemas named `bronze`/`silver`/`gold`/`ops` (no numeric prefix) to
  avoid backtick-quoting in every Spark SQL statement.

## Connectivity spike result
TBD — Session 2 Step G.
