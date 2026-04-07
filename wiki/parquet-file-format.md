---
title: Apache Parquet File Format
tags: [concept, entity]
sources: [8h-parquet-vutrinh]
created: 2026-04-06
updated: 2026-04-06
---

# Apache Parquet File Format

Apache Parquet is a hybrid columnar file format — not purely columnar. It combines row-wise and column-wise storage by grouping data into row groups (horizontal partition) with column chunks (vertical partition) within each.

## Data Layout

```
┌─────────────────────────────────────┐
│  Magic Number (PAR1)                │
├─────────────────────────────────────┤
│  Row Group 1                        │
│    Column Chunk A                   │
│      Dictionary Page (optional)     │
│      Data Page 1                    │
│      Data Page 2                    │
│    Column Chunk B                   │
│      ...                            │
├─────────────────────────────────────┤
│  Row Group 2                        │
│    ...                              │
├─────────────────────────────────────┤
│  Footer (FileMetadata)              │
│  Magic Number (PAR1)                │
└─────────────────────────────────────┘
```

## Structure

| Unit | Description |
|------|-------------|
| **Row Group** | Horizontal partition — subset of rows |
| **Column Chunk** | Data for one column within a row group; stored contiguously on disk |
| **Page** | Smallest data unit. Types: data page, dictionary page, index page |

## Metadata Model (Self-Describing)

- **Magic number** — `PAR1` at beginning and end; validates file format
- **FileMetadata** (footer) — Number of rows, schema, row group metadata. Each row group metadata contains ColumnMetadata: encoding, compression, sizes, page offsets, value counts, min/max statistics
- **PageHeader** — Stored with page data: value encoding, definition/repetition level encoding

## Write Process

1. Application issues write request (data, compression, encoding, file scheme)
2. Writer collects schema, null patterns, encoding, column types → FileMetadata
3. Write `PAR1` magic number
4. Calculate row groups from max row group size and data size
5. For each row group, iterate columns → write column chunks (pages sequentially)
6. Each page: header (rows, encoding for data/definition/repetition) + data
7. Dictionary page (if used) stored before data page
8. Column chunk metadata: min/max, sizes, page offsets
9. After all row groups → write FileMetadata to footer → write `PAR1`

## Read Process

1. Application issues read request (file, filters, desired columns)
2. Optionally validate `PAR1` at start and end
3. Read FileMetadata from footer → extract schema and row group metadata
4. **Row group pruning** — Check filters against column chunk min/max statistics; skip non-matching row groups
5. **Column selection** — Read only requested columns
6. For each row group: read column chunks using ColumnMetadata, locate pages via offsets, decode using page headers
7. Track rows read vs. total to know when chunk is complete

## Encoding

- **Dictionary encoding** — Replaces repeated values with shorter keys (applied by default when distinct values meet threshold)
- **Run-Length Encoding (RLE)** — Compresses consecutive identical values as value + count; applied after dictionary encoding

Columnar data is naturally more homogeneous → encoding is more effective than in row-wise formats.

## Nested Data

Parquet stores definition levels (for nested fields) and repetition levels (for array-like fields), inspired by Google Dremel's approach to semi-structured data.

## Parallelism

Three levels:
- **Multi-file** — Multiple Parquet files read simultaneously with multi-threading
- **Row group level** — Parallel reads across row groups within a file
- **Column level** — Parallel reads across column chunks

## OLAP Benefits

- Column pruning — read only needed columns
- Row group filtering — skip row groups using min/max statistics without scanning data
- Efficient encoding — columnar homogeneity improves compression ratios

Example: Query filtering on columns A and B only reads row groups whose A/B statistics match the filter, and only reads columns A and B within those groups.

## Connections

- Used by [[napa-storage|Napa's RO format]] (PAX-style)
- Referenced in [[databricks-lakehouse-architecture]] as the underlying file format with Delta Lake metadata layer
- Related to [[lsm-tree]] systems — many LSM-based systems convert to Parquet-like formats for read-optimized storage

## References

- [[8h-parquet-vutrinh]]
- Apache Parquet Official Docs
- Ailamaki et al., "Weaving Relations for Cache Performance" (VLDB 2001) — PAX model
