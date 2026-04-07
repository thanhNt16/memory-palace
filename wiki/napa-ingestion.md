---
title: Napa Ingestion
tags: [concept]
sources: [6h-google-analytics-napa-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Napa Ingestion

The ingestion block in [[napa|Napa]] handles inserting large volumes of data into storage.

## Process

1. Accepts data from Google services
2. Performs lightweight transformation
3. Writes data (ensures disk persistence and cross-datacenter replication)

## Scalability

Users can increase or decrease the number of ingestion workers to match throughput needs. Worker count is one of the levers in Napa's [[napa|flexibility model]] — more workers → higher ingestion throughput → higher cost.

## References

- [[6h-google-analytics-napa-vutrinh]]
