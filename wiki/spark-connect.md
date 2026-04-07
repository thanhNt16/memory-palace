---
title: Spark Connect
tags: [concept]
sources: [6h-pyspark-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Spark Connect

Introduced in Spark 3.4, Spark Connect is a protocol enabling client applications to connect to a remote Spark server via gRPC. Similar to connecting to a database via JDBC.

## How It Works

1. gRPC connection established between client and Spark Connect Server
2. Client converts DataFrame query to an **unresolved logical plan** describing the operation intent
3. Plan encoded using **protocol buffers** (language-agnostic) and sent to server
4. Server analyzes, optimizes, and converts to physical plan, then executes
5. Results sent back as **Apache Arrow record batches** via gRPC

## Benefits

- **No Py4j needed** — Client doesn't need to spawn a local JVM driver
- **Isolation** — Each application runs in its own process; OOM or client issues only affect that application
- **Independent upgrades** — Spark driver can be upgraded without updating all client applications (as long as protocol remains compatible)
- **Better debugging** — Interactive step-through debugging from IDEs
- **Language agnostic** — Protocol buffers enable any language to connect

## Architecture

```
Client (Python/any language)  ←→  gRPC (protobuf)  ←→  Spark Connect Server
                                                              ↓
                                                     Spark Driver (JVM)
                                                              ↓
                                                     Spark Executors
```

## References

- [[6h-pyspark-vutrinh]]
