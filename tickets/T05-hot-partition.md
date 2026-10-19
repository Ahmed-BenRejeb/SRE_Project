# T05 — Hot partition
Status: todo · Arc 1

## Setup
The sim sends 80% of events with the same key (one run, or one detector region).

## You should see
One consumer pegged and the others idle, lag on a single partition, and adding replicas does
nothing.

## Done when
- A dashboard shows lag **per partition**.
- You can explain why scaling out didn't help.
- A fixed key design. State your ordering requirement explicitly and show it's still preserved
  where it matters.
- A written decision on partition count, including what changing it later would cost.

## Read after
- Kafka partitioning and ordering guarantees
- DDIA, ch. 6 (Partitioning)
- Amazon Builders' Library: "Workload isolation using shuffle-sharding"
