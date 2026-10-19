# T03 — Poison event
Status: todo · Arc 1

## Setup
The sim injects 1 malformed event in 100k: NaN pt, missing fields, a 10 MB payload, an unknown
schema version. Later, roll out a **v2 event schema** while v1 producers are still running.

## You should see
The trigger crashlooping, one partition's lag growing forever, and everything behind the bad
event stuck.

## Done when
- Bad events are quarantined with a *reason*, and the pipeline keeps running.
- The ledger counts quarantined events as their own bucket, so the totals still balance.
- An alert fires on the quarantine rate.
- A replay tool sends quarantined events back through after a fix, without duplicates.
- v1 and v2 events coexist with no downtime during the schema rollout.

## Read after
- Dead-letter queue patterns
- Schema registries (Redpanda has one built in) and compatibility modes
- Exactly how CrashLoopBackOff backs off, and why that hides the problem
