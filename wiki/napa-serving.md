---
title: Napa Serving
tags: [concept]
sources: [6h-google-analytics-napa-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Napa Serving

The serving block in [[napa|Napa]] delivers sub-millisecond query results through multiple optimization techniques.

## Techniques

1. **Materialized views** — Uses views to answer queries whenever possible instead of base tables
2. **Predicate pushdown** — Filters pushed to storage layer to minimize network data transfer
3. **Parallel subqueries** — Each LSM-tree segment keeps a local index; queries are partitioned into thousands of subqueries satisfying filters
4. **Two cache layers** — Local RAM on query workers + distributed cache shared between workers
5. **Prefetching** — Reduces disk accesses by anticipating data needs
6. **Small I/O combining** — Batches small reads for efficiency

## References

- [[6h-google-analytics-napa-vutrinh]]
