---
title: Unity Catalog
tags: [concept, entity, source]
sources: [training-multi-agent-grpo]
created: 2026-04-06
updated: 2026-04-06
---

# Unity Catalog

Unity Catalog (UC) is Databricks's unified governance and metadata service for the lakehouse. Originally proprietary, it was open-sourced after serving 9000+ customers since 2021.

## Scale

- 100 million tables under management
- 400,000 ML models
- 60,000 API calls per second

## Problems UC Solves

- **Uniform access control** — Secure access to assets via catalog or direct cloud storage path
- **Diverse asset types** — Tables, ML models, and extensible via manifest registration
- **External access** — Data sharing without copying (avoids storage cost and sync complexity)
- **Discovery support** — Assets are discoverable with transparent lineage and lifecycle
- **Performance** — Metadata operations must be fast despite high throughput demands

## Architecture

See [[databricks-lakehouse-architecture]] for how UC fits in the three-tier system (storage, runtime, catalog).

UC itself is a layered architecture:
1. **Entity-Relationship Model** — Backed by a relational database; exposes lookup, relationship mapping, privilege, and state management
2. **Adapter Layer** — Integration points for different asset types with various cloud providers
3. **Core Features Layer** — Namespace, lifecycle management, access control, audit logging, APIs
4. **Background Functions Layer** — Discovery, search, lineage (event-driven, independently scalable)

## Access Control

Two-level security model:
- **Metadata level** — UC REST APIs validate user permissions per operation
- **Data level** — Temporary credentials issued by UC; no direct storage access. Admins grant storage permissions only to UC.

For fine-grained control (row/column security), UC partners with the query engine for enforcement.

## Performance Optimizations

- **Batching** — Multiple asset requests batched together
- **Caching** — Immutable metadata (e.g., temporary credentials) cached at UC service and query engines
- **Write-through cache** — Mutable metadata cached with sharded relational database backend
- **Cache eviction** — LRU for unpopular entities; timeout caps for popular entity versions
- **Isolation** — Snapshot isolation and serializable isolation (SSI) via database versions

## SQL Query Journey

1. DBR receives SQL query, parses it for data asset references
2. DBR sends REST request to UC for permission check and metadata retrieval
3. DBR uses metadata for query planning
4. DBR requests temporary credentials from UC for object storage access
5. DBR accesses data using credentials
6. Query processed (scan, filter, join, aggregate); results returned to user

## References

- Databricks, "Unity Catalog: Open and Universal Governance for the Lakehouse and Beyond," 2025
- [[5h-unity-catalog-vutrinh]] — Vu Trinh's article summarizing the paper
