---
title: PySpark
tags: [concept, entity]
sources: [6h-pyspark-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# PySpark

PySpark is the Python interface to Apache Spark. It is a wrapper — when a PySpark script runs, two processes are spawned: a Python process (driver) and a JVM process (actual Spark driver), communicating via [[py4j]].

## Architecture

```
Python Process (Driver)  ←→  Py4J (IPC)  ←→  JVM Process (Spark Driver)
                                                       ↓
                                              Cluster Manager
                                                       ↓
                                              JVM Executors
```

- Python process listens on port 25334
- JVM process listens on port 25333
- Communication uses Inter-Process Communication (IPC) with serialization/deserialization
- Physical data processing occurs in JVM executors, same as Scala Spark

## Spark Application Components

- **Driver** — JVM process managing the application: user input, task distribution, execution planning (logical → physical plan)
- **Cluster Manager** — Manages machines/processes (YARN, Mesos, standalone). Allocates resources for driver and executors.
- **Executors** — Run tasks assigned by driver, report status/results. Each Spark application has its own set.

## Python UDF Overhead

When a PySpark application uses Python UDFs:

1. Additional Python processes spawned alongside JVM executors
2. JVM serializes data → sends to Python process via IPC
3. Python deserializes, executes function, serializes result
4. Result sent back to JVM

Overhead: serialization/deserialization cost + UDFs don't benefit from Catalyst optimizer or Tungsten (off-heap memory optimization). Row-by-row processing.

## Improvements Over Time

### Project Zen (Spark 3.0)
Community initiative making PySpark more Pythonic:
- Updated documentation (examples, Getting Started guide)
- Python type hints for IDE autocompletion and static analysis
- Concise Python tracebacks instead of mixed Python/Scala stack traces

### Pandas UDFs (Spark 2.3)
Vectorized UDFs using Arrow for data exchange + Pandas for manipulation. Execute on batches instead of row-by-row. Serialization bypassed via Arrow columnar format.

### Arrow-Optimized UDFs (Spark 3.5)
Both JVM and Python processes handle data in Arrow format, bypassing costly serialization/deserialization. Columnar format improves processing time vs. Pickle (row-wise).

### Python Data Source API (Spark 4.0)
Define custom data sources/sinks in Python. Previously required Scala or Java.

### Spark Connect (Spark 3.4)
See [[spark-connect]] — gRPC protocol connecting client to remote Spark server. Eliminates Py4j dependency.

## Adoption

- 2013: 92% Scala, 5% Python, 3% SQL (Databricks users)
- 2020: 47% Python, 41% SQL, 12% Scala (Databricks users)

## References

- [[6h-pyspark-vutrinh]]
- Related: [[databricks-lakehouse-architecture]] — DBR is a fork of Apache Spark
