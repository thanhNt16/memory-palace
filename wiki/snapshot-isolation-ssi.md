---
title: Snapshot Isolation and SSI
tags: [concept]
sources: [5h-unity-catalog-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Snapshot Isolation and Serializable Snapshot Isolation (SSI)

## Snapshot Isolation (SI)

Each read transaction reads a consistent snapshot of the database. The transaction only sees changes committed before it starts.

## Serializable Isolation

The strongest isolation level: parallel transactions produce effects equivalent to serial execution.

## Serializable Snapshot Isolation (SSI)

Based on SI — reads still use consistent snapshots. SSI adds mechanisms to detect write conflicts. A common approach checks whether a transaction's reading snapshot is stale. If committed changes were ignored during the transaction, the database aborts non-read-only transactions.

## Application in [[unity-catalog|Unity Catalog]]

UC uses database versions to provide both snapshot isolation and serializable isolation for metadata operations.

## References

- [[5h-unity-catalog-vutrinh]]
