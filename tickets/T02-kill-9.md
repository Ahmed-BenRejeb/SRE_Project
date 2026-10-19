# T02 — kill -9
Status: todo · Arc 1

## Setup
Under steady load (and once during a fill), a script `kill -9`s a random trigger pod every
20–60 s, 50 times. Then the same for ingest pods. Then for the Redpanda broker.

## Done when
- Ledger: **0 lost**.
- Duplicates are either 0 at the output, or provably harmless: the histogram still matches the
  offline truth bin for bin, and the catalog has no double-counted files.
- `design/delivery-guarantees.md` states the guarantee **at every hop** (sim → ingest → Redpanda →
  trigger → MinIO/catalog/histogram), and *where* each guarantee is enforced in your code.

## Read after
- Kafka/Redpanda consumer offset commit semantics: commit before vs after processing
- Idempotent producer and transactions, and what "exactly-once" actually covers
- Amazon Builders' Library: "Making retries safe with idempotent APIs"
- DDIA, ch. 11 (Stream Processing)
