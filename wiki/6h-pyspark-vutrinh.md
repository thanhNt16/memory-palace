---
title: "6h PySpark — Vu Trinh"
tags: [source]
sources: []
created: 2026-04-06
updated: 2026-04-06
---

# 6h PySpark

**Author:** Vu Trinh (Dimensions newsletter on Substack)
**Date:** 2025-08-26
**Type:** Deep-dive into PySpark internals, architecture, and evolution

## Summary

Vu Trinh's exploration of how PySpark works behind the scenes — the Python wrapper over JVM Spark, Py4j communication, Python UDF overhead, and the key improvements that made Python the dominant Spark language (Project Zen, Arrow UDFs, Pandas UDFs, Spark Connect, Python Data Source API).

## Key Takeaways

- PySpark is a wrapper: Python driver communicates with JVM driver via Py4j (IPC)
- Python UDFs require separate Python processes — significant serialization overhead
- Project Zen (Spark 3.0): Pythonic APIs, type hints, better errors
- Pandas UDFs (Spark 2.3): vectorized execution via Arrow
- Arrow-optimized UDFs (Spark 3.5): bypass serialization entirely
- Python Data Source API (Spark 4.0): custom sources in Python
- Spark Connect (Spark 3.4): gRPC protocol eliminates Py4j dependency

## Concepts Covered

- [[pyspark]] — Main architecture and evolution
- [[py4j]] — Python-JVM bridge library
- [[spark-connect]] — Remote Spark server protocol
- [[databricks-lakehouse-architecture]] — DBR as fork of Spark

## Source Location

`raw/vutrinh/I spent 6 hours learning PySpark.md`
