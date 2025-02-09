---
title: Throughput and latency metrics
headerTitle: Throughput and latency
linkTitle: Throughput+latency metrics
headcontent: Monitor query processing and database IOPS
description: Learn about YugabyteDB's throughput and latency metrics, and how to select and use the metrics.
menu:
  preview:
    identifier: throughput
    parent: metrics-overview
    weight: 100
type: docs
---

## YSQL query processing

YSQL query processing metrics represent the total inclusive time it takes YugabyteDB to process a YSQL statement after the query processing layer begins execution. These metrics include the time taken to parse and execute the SQL statement, replicate over the network, the time spent in the storage layer, and so on. The preceding metrics do not capture the time to deserialize the network bytes and parse the query.

All handler latency metrics include additional attributes. Refer to [Latency metric attributes](#latency-metric-attributes).

The following are key metrics for evaluating YSQL query processing.

##### handler_latency_yb_ysqlserver_SQLProcessor_InsertStmt

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to parse and execute INSERT statement.

##### handler_latency_yb_ysqlserver_SQLProcessor_SelectStmt

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to parse and execute SELECT statement.

##### handler_latency_yb_ysqlserver_SQLProcessor_UpdateStmt

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to parse and execute UPDATE statement.

##### handler_latency_yb_ysqlserver_SQLProcessor_BeginStmt

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to parse and execute transaction BEGIN statement.

##### handler_latency_yb_ysqlserver_SQLProcessor_CommitStmt

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to parse and execute transaction COMMIT statement.

##### handler_latency_yb_ysqlserver_SQLProcessor_RollbackStmt

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to parse and execute transaction ROLLBACK statement.

##### handler_latency_yb_ysqlserver_SQLProcessor_OtherStmts

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to parse and execute all other statements apart from the preceding ones listed in this table. This includes statements like PREPARE, RELEASE SAVEPOINT, and so on.

##### handler_latency_yb_ysqlserver_SQLProcessor_Transactions

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to execute any of the statements in this table.

The YSQL throughput can be viewed as an aggregate across the whole cluster, per table, and per node by applying the appropriate aggregations.

<!-- | Metrics | Unit | Type | Description |
| :------ | :--- | :--- | :---------- |
| `handler_latency_yb_ysqlserver_SQLProcessor_InsertStmt` | microseconds | counter | The time in microseconds to parse and execute INSERT statement |
| `handler_latency_yb_ysqlserver_SQLProcessor_SelectStmt` | microseconds | counter | The time in microseconds to parse and execute SELECT statement |
| `handler_latency_yb_ysqlserver_SQLProcessor_UpdateStmt` | microseconds | counter | The time in microseconds to parse and execute UPDATE statement |
| `handler_latency_yb_ysqlserver_SQLProcessor_BeginStmt` | microseconds | counter | The time in microseconds to parse and execute transaction BEGIN statement |
| `handler_latency_yb_ysqlserver_SQLProcessor_CommitStmt` | microseconds | counter | The time in microseconds to parse and execute transaction COMMIT statement |
| `handler_latency_yb_ysqlserver_SQLProcessor_RollbackStmt` | microseconds | counter | The time in microseconds to parse and execute transaction ROLLBACK statement |
| `handler_latency_yb_ysqlserver_SQLProcessor_OtherStmts` | microseconds | counter | The time in microseconds to parse and execute all other statements apart from the preceding ones listed in this table. This includes statements like PREPARE, RELEASE SAVEPOINT, and so on. |
| `handler_latency_yb_ysqlserver_SQLProcessor_Transactions` | microseconds | counter | The time in microseconds to execute any of the statements in this table.| -->

## Database IOPS (reads and writes)

The [YB-TServer](../../../architecture/concepts/yb-tserver/) is responsible for the actual I/O of client requests in a YugabyteDB cluster. Each node in the cluster has a YB-TServer, and each hosts one or more tablet peers.

All handler latency metrics include additional attributes. Refer to [Latency metric attributes](#latency-metric-attributes).

The following are key metrics for evaluating database IOPS.

##### handler_latency_yb_tserver_TabletServerService_Read

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to perform WRITE operations at a tablet level.

##### handler_latency_yb_tserver_TabletServerService_Write

| Unit | Type | Description |
| :--- | :--- | :--- |
| microseconds | counter | The time in microseconds to perform READ operations at a tablet level.

<!-- | Metrics | Unit | Type | Description |
| :------ | :--- | :--- | :---------- |
| `handler_latency_yb_tserver_TabletServerService_Read` | microseconds | counter | The time in microseconds to perform WRITE operations at a tablet level |
| `handler_latency_yb_tserver_TabletServerService_Write` | microseconds | counter | The time in microseconds to perform READ operations at a tablet level | -->

These metrics can be viewed as an aggregate across the whole cluster, per table, and per node by applying the appropriate aggregations.

## Latency metric attributes

All handler latency metrics include the following additional attributes:

- `total_count/count` - The number of times the value of a metric has been measured.
- `min` - The minimum value of a metric across all measurements.
- `mean` - The average value of the metric across all measurements.
- `Percentile_75` - The 75th percentile value of the metric across all measurements.
- `Percentile_95` - The 95th percentile value of the metric across all measurements.
- `Percentile_99` - The 99th percentile of the metric across all metrics measurements.
- `Percentile_99_9` - The 99.9th percentile of the metric across all metrics measurements.
- `Percentile_99_99` - The 99.99th percentile of the metric across all metrics measurements.
- `max` - The maximum value of the metric across all measurements.
- `total_sum/sum` - The aggregate of all the metric values across the measurements reflected in total_count/count.

For example, if a `select * from table` is executed once and returns 8 rows in 10 milliseconds, then the `total_count=1`, `total_sum=10`, `min=6`, `max=10`, and `mean=8`. If the same query is run again and returns in 6 milliseconds, then the `total_count=2`, `total_sum=16`, `min=6`, `max=10`, and `mean=8`.

Although these attributes are present in all `handler_latency` metrics, they may not be calculated for all the metrics.
