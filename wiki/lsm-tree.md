---
title: LSM-Tree (Log-Structured Merge-Tree)
tags: [concept]
sources: [6h-google-analytics-napa-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# LSM-Tree

A storage engine paradigm optimized for high write throughput. Used by [[napa|Napa]], [[vortex-bigquery|BigQuery Vortex]], ClickHouse, Apache Paimon, and RisingWave (Hummock).

## How It Works

1. Writes are buffered in an **in-memory component** (memtable)
2. Periodically flushed to disk as **immutable, sorted segments** (SSTables)
3. Background **compaction** merges segments, reconciles updates/deletes, maintains sorted order across levels

This converts random writes into sequential disk I/O.

## Trade-offs

- **Write-optimized** — Append-only strategy avoids random I/O
- **Read amplification** — Reads may need to check multiple segments
- **Write amplification** — Compaction rewrites data multiple times

## vs B-Trees

B-trees are mutable, page-oriented structures that update in-place. Writes navigate the tree to find the relevant page and modify it directly, incurring random I/O and potential page splits/merges. B-trees avoid the fragmentation problem that LSM-trees face (updates scattered across files).

## Applications

- [[napa-storage]] — WO/RO file management and compaction
- ClickHouse MergeTree engine — LSM-tree-like mechanism
- Apache Paimon — LSM tree for primary-key table storage
- RisingWave Hummock — LSM storage for stream processing

## References

- [[6h-google-analytics-napa-vutrinh]]
