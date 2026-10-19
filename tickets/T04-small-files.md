# T04 — Small files
Status: todo · Arc 1

## Setup
Run for many hours, or speed up the sim, with the trigger flushing small batches often. Then
query with DuckDB: "all selected events from run X with mass in [80, 100] GeV".

## Done when
- Before and after numbers for: object count, query latency, and **$/month if this were S3**
  (count the requests, not just the storage).
- A compaction job runs as a CronJob. It's safe to run while writers are writing, and safe if
  killed halfway: the catalog and ledger stay consistent, with no lost or doubled files.
- The storage layout (partitioning, file size, row-group size) is justified in writing.

## Read after
- Parquet row groups and column statistics; partition pruning
- S3 request pricing
- Why table formats (Iceberg, Delta) exist, and which of their problems you just met
