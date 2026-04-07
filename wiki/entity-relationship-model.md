---
title: Entity-Relationship Model (Unity Catalog)
tags: [concept]
sources: [5h-unity-catalog-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Entity-Relationship Model in Unity Catalog

The ER data model is the foundation of [[unity-catalog|Unity Catalog]]'s ability to manage diverse asset types.

## What It Provides

- ID- or name-based lookup
- Parent-child relationship mapping
- Privilege grant management
- State management for resource provisioning and cleanup

The model is materialized in a relational database and is extensible — developers can add new asset types.

## Asset Type Registration

Developers register a new asset type by adding a manifest to UC's asset types registry. The manifest specifies:
- Location in the model hierarchy
- Supported operations and privileges
- Rules associated with each operation
- Lifecycle management details

## Layered Architecture

1. **ER Model** (bottom) — Relational database
2. **Adapter Layer** — Integration points for asset types with cloud providers
3. **Core Features Layer** — Namespace, lifecycle, access control, audit logging, APIs
4. **Background Functions** (top) — Discovery, search, lineage

## References

- [[5h-unity-catalog-vutrinh]]
