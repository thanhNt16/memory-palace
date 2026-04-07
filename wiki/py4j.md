---
title: Py4J
tags: [concept]
sources: [6h-pyspark-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Py4J

Py4J is a library that enables Python programs to access Java objects in a JVM. It is the bridge between the Python process and the JVM process in [[pyspark|PySpark]].

## How It Works in PySpark

- Python process listens on port 25334 (default)
- JVM process listens on port 25333 (default)
- Communication via Inter-Process Communication (IPC)
- Data transfer requires serialization from sender, deserialization from receiver
- Python process can send commands to JVM and view results

## Limitations

- Overhead from serialization/deserialization, especially for large objects (e.g., Pandas DataFrames)
- Replaced by [[spark-connect|Spark Connect]] (gRPC + Arrow) in newer Spark versions for improved performance

## References

- [[6h-pyspark-vutrinh]]
