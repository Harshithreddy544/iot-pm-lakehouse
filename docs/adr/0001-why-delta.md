# ADR 0001: Delta Lake over plain Parquet

Status: accepted, 2026-07-08

## Context
Need ACID upserts for SCD2 MERGE, streaming source/sink semantics,
and time travel to verify backfills.

## Decision
All tables are Delta, managed in Unity Catalog.

## Consequences
Locked to Delta-aware engines; acceptable given Databricks target
roles. Iceberg revisited only if a target JD specifically demands it.
