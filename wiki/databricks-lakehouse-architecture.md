---
title: Databricks Lakehouse Architecture
tags: [concept]
sources: [5h-unity-catalog-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Databricks Lakehouse Architecture

The Databricks lakehouse consists of three key components:

## 1. Storage

Separates storage and compute for flexibility and interoperability. Uses object storage services (AWS S3, Google Cloud Storage). A metadata layer on top of physical data (Parquet) provides table abstraction via [[delta-lake]] and [[apache-iceberg]].

## 2. Databricks Runtimes (DBR)

Databricks's core execution engine — a fork of Apache Spark with reliability and performance enhancements. Includes the Photon engine, a library integrating closely with DBR to enhance analytical workloads. Photon provides new physical operators inside DBR; Spark query plans use them transparently. Customers run existing workloads unchanged while benefiting from Photon.

## 3. Unity Catalog Service

See [[unity-catalog]] — the multi-tenant service providing all UC functionality and APIs. Clients (mostly query engines) use this service to deliver user needs.

## References

- [[5h-unity-catalog-vutrinh]]
