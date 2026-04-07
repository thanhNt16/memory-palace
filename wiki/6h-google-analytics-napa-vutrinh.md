---
title: "6h Google Serves Analytics Applications — Vu Trinh"
tags: [source]
sources: []
created: 2026-04-06
updated: 2026-04-06
---

# 6h Google Serves Analytics Applications

**Author:** Vu Trinh (Dimensions newsletter on Substack)
**Date:** 2025-04-30
**Type:** Article summarizing Google's Napa paper

## Summary

Vu Trinh's walkthrough of Google's Napa system — the internal warehouse powering analytics for billion-user services. Covers Napa's unique workload profile (sub-ms latency, high throughput), its three-block architecture (ingestion, storage, serving), and the technical choices (LSM-trees, dual file formats, materialized views) that enable its flexibility model.

## Key Takeaways

- Napa is closer to real-time OLAP (Pinot/Druid) than traditional DW (Snowflake/Databricks)
- Three-block decoupled architecture (ingestion, storage, serving) enables independent scaling and the flexibility model
- Materialized views are the primary query optimization, not data pruning
- LSM-trees with dual file formats (WO/RO) handle the write-heavy workload
- Clients trade off freshness, performance, and cost in a three-way triangle

## Concepts Covered

- [[napa]] — Main system overview
- [[napa-ingestion]] — Data ingestion block
- [[napa-storage]] — Storage, LSM-trees, dual file formats
- [[napa-serving]] — Query serving optimizations
- [[lsm-tree]] — Log-Structured Merge-Tree paradigm
- [[materialized-views]] — Materialized views as primary query optimization

## Source Location

`raw/vutrinh/6h Google serves analytics applications.md`
