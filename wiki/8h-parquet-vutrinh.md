---
title: "8h Parquet — Vu Trinh"
tags: [source]
sources: []
created: 2026-04-06
updated: 2026-04-06
---

# 8h Parquet

**Author:** Vu Trinh (Dimensions newsletter on Substack)
**Date:** 2024-08-24
**Type:** Deep-dive into Apache Parquet file format internals

## Summary

Vu Trinh's comprehensive walkthrough of Apache Parquet — its hybrid columnar structure, metadata model, write and read protocols, encoding strategies, and OLAP benefits. Includes practical exploration using fastparquet to write/read a Pandas dataframe.

## Key Takeaways

- Parquet is hybrid (not purely columnar): row groups (horizontal) + column chunks (vertical)
- Self-describing via footer metadata — schema, stats, encoding all embedded
- Write: schema → PAR1 → row groups → column chunks → pages → footer → PAR1
- Read: validate PAR1 → footer → prune row groups via min/max → select columns → read pages
- Dictionary encoding + RLE leverages columnar homogeneity
- Three levels of parallelism: multi-file, row-group, column
- Nested data via definition/repetition levels (Dremel-inspired)

## Concepts Covered

- [[parquet-file-format]]
- [[napa-storage]] — Napa's RO format shares PAX characteristics with Parquet
- [[databricks-lakehouse-architecture]] — Delta Lake provides table abstraction over Parquet

## Source Location

`raw/vutrinh/I spent 8 hours learning Parquet. Here's what I discovered.md`
