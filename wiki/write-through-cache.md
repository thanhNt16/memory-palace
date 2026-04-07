---
title: Write-Through Cache
tags: [concept]
sources: [5h-unity-catalog-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Write-Through Cache

Data is written simultaneously to the cache and the primary storage system. This ensures the cache always contains the most recent data.

## Application in [[unity-catalog|Unity Catalog]]

UC uses a write-through cache for the relational database backing mutable metadata (table names, columns, permissions). The database is sharded across nodes; each node owns a subset of data in both storage and cache.

## Cache Eviction

Two strategies in UC:
1. **LRU** — Discards unpopular cached entities
2. **Timeout caps** — Limits cached versions of popular entities. When a new version enters the cache, existing versions serve requests for at most the timeout period.

## References

- [[5h-unity-catalog-vutrinh]]
