# Loki Configuration Guide

## 1. Increase Node Computing Power

Ensure your nodes have sufficient computing resources to handle the load efficiently.

## 2. Use TSDB

### Recommended Index Stores
- **Single Store TSDB**: Stores TSDB index files in the object store. Recommended for Loki 2.8 and newer. [Learn more](https://grafana.com/docs/loki/latest/configure/storage/#supported-storage-backends).
- **Single Store BoltDB (boltdb-shipper)**: Stores boltdb index files in the object store. Recommended for Loki 2.0 through 2.7.x. [Learn more](https://grafana.com/docs/loki/latest/configure/storage/#supported-storage-backends).

### Performance Comparison

#### TSDB
- Request 1: 0m4.313s
- Request 2: 0m4.188s
- Request 3: 0m4.211s
- Request 4: 0m4.274s
- Request 5: 0m4.206s

#### BoltDB-Shipper
- Request 1: 0m15.728s
- Request 2: 0m15.961s
- Request 3: 0m15.705s
- Request 4: 0m15.445s
- Request 5: 0m15.536s

## 3. Over-Parallelizing

Limit the maximum number of queries scheduled in parallel by the frontend for TSDB schemas. Refer to [this guide](https://grafana.com/blog/2023/12/28/the-concise-guide-to-loki-how-to-get-the-most-out-of-your-query-performance/) for more details.

- **Maximum Query Parallelism**: Adjust using `tsdb_max_query_parallelism` (default = 128). For small datasets, ~32 is suggested.
- **Query Splitting**: Control splitting by setting `split_queries_by_interval` (default = 1h). This impacts parallelization; adjust based on data period to avoid overhead.

### Parallelization Settings
- **max_concurrent**: Typically set to 8. Adjust based on resource constraints. Reduce if experiencing Out Of Memory (OOM) crashes. For single binary setups, align with core count (2x number of cores or less).

## 4. Label Best Practices

Ensure optimal label usage to reduce label cardinality. For more details, refer to [this guide](https://grafana.com/blog/2023/12/20/the-concise-guide-to-grafana-loki-everything-you-need-to-know-about-labels/).

## 5. Upgrade Loki

Ensure your Loki installation is up-to-date to leverage the latest features and improvements.

## 6. Enable Automatic Stream Sharding

Configure Loki to automatically shard streams for better performance.

## 7. Force Chunks to Flush

Adjust chunk flushing to ensure efficient memory usage:
- **Chunk Idle Period**: Set using `chunk_idle_period` (default = 30m). This determines how long chunks sit in-memory without updates before being flushed, ensuring even half-empty chunks are flushed after a period of inactivity.
