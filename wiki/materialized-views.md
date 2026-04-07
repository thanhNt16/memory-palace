---
title: Materialized Views (Napa)
tags: [concept]
sources: [6h-google-analytics-napa-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Materialized Views in Napa

[[napa|Napa]] uses materialized views as its primary query performance technique — a different approach from traditional data warehouses like Snowflake or Databricks, which rely on data pruning (e.g., min-max indexes in Parquet).

## Properties

- Sorted, indexed, and range-partitioned by primary key(s)
- Updated via the same LSM-tree compaction process used for base tables
- Napa uses views to answer queries whenever possible instead of querying base tables

## Role in Flexibility

The number and maintenance frequency of views is a key lever in Napa's three-way tradeoff:
- **More views** → Better query performance but higher cost (more workers, more maintenance)
- **Fewer views** → Lower cost but degraded performance (fewer pre-computed results available)

## References

- [[6h-google-analytics-napa-vutrinh]]
