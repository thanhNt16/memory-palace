---
title: Napa
tags: [concept, entity]
sources: [6h-google-analytics-napa-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Napa

Napa is Google's internal data warehouse system designed to serve analytics applications at scale — both internal analytics for Google services and user-facing analytics with sub-millisecond latency requirements.

## Workload Profile

Unlike traditional cloud data warehouses (Snowflake, Databricks), Napa serves:
- Simpler analytical queries (fewer joins)
- High-throughput data ingestion from billion-user services
- Sub-millisecond latency requirements
- High data freshness demands

Comparable to real-time OLAP systems like [[apache-pinot]] or [[apache-druid]].

## Scale

- 10 GB/s throughput
- Sub-millisecond query latency
- Serves analytics for services with 1B+ users

## Architecture

Three decoupled blocks: [[napa-ingestion]], [[napa-storage]], [[napa-serving]].

Built on Google infrastructure:
- **Colossus** — Data storage
- **Spanner** — Strict transaction semantics (metadata management)
- **F1 Query** — Data serving
- **Control plane** — Coordinates work among sub-services

## Primary Optimization: Materialized Views

Napa aggressively uses [[materialized-views]] rather than data pruning (unlike Snowflake/Databricks). Views are sorted, indexed, and range-partitioned by primary key(s).

## Flexibility Model

Napa allows clients to trade off three dimensions:
- **Freshness** — How quickly ingested data becomes queryable
- **Performance** — Query latency
- **Cost** — Resource usage (compaction, view maintenance, ingestion workers)

The decoupled architecture enables tuning by adjusting worker allocation and view maintenance strategies.

## Related Systems

- [[vortex-bigquery]] — BigQuery's storage engine, also uses dual file formats + LSM-trees
- ClickHouse — LSM-tree-like mechanism (MergeTree engine)
- Apache Hudi — Dual file formats
- Apache Paimon — LSM tree for file storage
- RisingWave — Hummock LSM storage engine

## References

- Google, "Napa: Powering Scalable Data Warehousing with Robust Query Performance at Google" (2021)
- [[6h-google-analytics-napa-vutrinh]]
