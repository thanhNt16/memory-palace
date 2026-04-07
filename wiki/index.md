---
title: Wiki Index
tags: [meta]
updated: 2026-04-06
---

# Wiki Index

## Concepts
- [[grpo-algorithm]] — Group Relative Policy Optimization: RL method comparing trajectory groups for agent training
- [[multi-agent-architecture]] — Planner/Executor/Verifier loop with tool orchestration
- [[ppo-loss]] — Proximal Policy Optimization: clipped surrogate loss with KL penalty
- [[qlora-peft]] — Parameter-efficient fine-tuning via 4-bit quantization + LoRA adapters
- [[reward-modeling]] — External judge (GPT-4o) assigning binary rewards to agent trajectories
- [[unity-catalog]] — Databricks's unified governance and metadata service for the lakehouse (open-sourced)
- [[databricks-lakehouse-architecture]] — Three-tier lakehouse: storage, DBR/Photon runtime, Unity Catalog service
- [[write-through-cache]] — Simultaneous write to cache and storage; used by UC for mutable metadata
- [[snapshot-isolation-ssi]] — Snapshot isolation and serializable snapshot isolation for concurrent metadata ops
- [[entity-relationship-model]] — UC's extensible ER data model backing diverse asset type management
- [[napa]] — Google's internal analytics warehouse: sub-ms latency, 10GB/s throughput, flexibility model
- [[napa-ingestion]] — Napa's ingestion block: scalable workers, lightweight transformation, cross-DC replication
- [[napa-storage]] — Napa's storage block: LSM-tree, dual file formats (WO/RO), view maintenance
- [[napa-serving]] — Napa's serving block: predicate pushdown, parallel subqueries, two cache layers, prefetching
- [[lsm-tree]] — Log-Structured Merge-Tree: append-only storage paradigm for high write throughput
- [[materialized-views]] — Napa's primary query optimization via pre-computed, sorted, indexed views
- [[parquet-file-format]] — Hybrid columnar format: row groups + column chunks, self-describing footer, dictionary/RLE encoding
- [[pyspark]] — Python wrapper over JVM Spark: Py4j bridge, UDF overhead, evolution (Project Zen, Arrow UDFs, Spark Connect)
- [[spark-connect]] — Spark 3.4 gRPC protocol: client connects to remote Spark server, eliminates Py4j
- [[py4j]] — Python-to-JVM bridge library used by PySpark for IPC communication

## Entities
<!-- Entity pages listed here -->

## Sources
- [[training-multi-agent-grpo]] — Fareed Khan (Feb 2026): tutorial on GRPO for multi-agent planning
- [[5h-unity-catalog-vutrinh]] — Vu Trinh (2025): Unity Catalog architecture deep-dive based on Databricks paper
- [[6h-google-analytics-napa-vutrinh]] — Vu Trinh (Apr 2025): Google Napa architecture for analytics applications
- [[8h-parquet-vutrinh]] — Vu Trinh (Aug 2024): Parquet file format internals, read/write protocols, encoding
- [[6h-pyspark-vutrinh]] — Vu Trinh (Aug 2025): PySpark internals, Py4j, UDF evolution, Spark Connect

## Analyses
<!-- Query-derived pages listed here -->
