---
title: "5h Unity Catalog — Vu Trinh"
tags: [source]
sources: []
created: 2026-04-06
updated: 2026-04-06
---

# 5h Unity Catalog

**Author:** Vu Trinh (Dimensions newsletter on Substack)
**Date:** 2025
**Type:** Article summarizing the Databricks Unity Catalog paper

## Summary

Vu Trinh's walkthrough of the Databricks Unity Catalog paper ("Unity Catalog: Open and Universal Governance for the Lakehouse and Beyond", ACM 2025). Covers the problems UC was designed to solve, how it fits into the Databricks lakehouse architecture, the SQL query lifecycle, and the system design decisions.

## Key Takeaways

- UC serves as an independent, disaggregated catalog service — not tied to any specific query engine
- Supports diverse asset types through an ER model + adapter layer with manifest-based registration
- Two-level access control: metadata (REST APIs) + data (temporary credentials, no direct storage access)
- Event-driven architecture for background functions (discovery, lineage) — independently scalable
- Performance via write-through cache, sharding, LRU eviction with timeout caps, and SSI isolation

## Concepts Covered

- [[unity-catalog]]
- [[databricks-lakehouse-architecture]]
- [[write-through-cache]]
- [[snapshot-isolation-ssi]]
- [[entity-relationship-model]]

## Source Location

`raw/vutrinh/5h Unity Catalog.md`
