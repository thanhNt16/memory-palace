---
title: Napa Storage
tags: [concept]
sources: [6h-google-analytics-napa-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Napa Storage

The storage block in [[napa|Napa]] is responsible for storing data and maintaining [[materialized-views]].

## Dual File Formats

- **Write-Optimized (WO)** — Serves high-throughput data ingestion. Not immediately available for reading (can impact query performance).
- **Read-Optimized (RO)** — Designed for efficient data reading. Shares characteristics with PAX format (used in Parquet, BigQuery, Snowflake).

WO→RO conversion happens via LSM-tree compaction.

## LSM-Tree Storage Engine

See [[lsm-tree]] for the general concept. In Napa:

- Data ingested into WO format (append-only)
- Periodically flushed to disk as immutable sorted segments (SSTables)
- Background compaction merges segments via merge sort
- Compaction sorts records by key for binary search, aggregates scattered updates
- Applies to both base tables and materialized view updates

If compaction can't keep up, merge can happen at query time (trading performance for freshness).

## References

- [[6h-google-analytics-napa-vutrinh]]
