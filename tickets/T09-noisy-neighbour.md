# T09 — Noisy neighbour
Status: todo · Arc 2

## Setup
Launch reprocessing Jobs (read Parquet from MinIO and recompute, CPU- and memory-heavy) while a
fill is running.

## You should see
Ingest p99 exploding, maybe evictions or OOM kills, nodes under pressure.

## Done when
- The ingest SLO holds while batch work runs at the highest throughput that still allows it.
- Batch work goes through Kueue with quotas, with PriorityClasses in place.
- You can explain, with evidence from the node itself (cgroup files, `dmesg`), the difference
  between **eviction** and an **OOM kill** and which one you hit.
- You measured CPU throttling, decided whether to set CPU limits at all, and defended the choice.
- You can explain the QoS class of every pod you run.

## Read after
- Node-pressure eviction
- cgroup v2: memory.max vs memory.high; CFS bandwidth throttling
- Kueue concepts: ClusterQueue, LocalQueue, cohorts
