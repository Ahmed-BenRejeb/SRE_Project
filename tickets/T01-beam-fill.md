# T01 — Beam fill
Status: todo · Arc 1

## Setup
Add a "fill" pattern to the sim: a quiet baseline, then a burst for 90 s every 10 min. Set the
burst to about **3× what ingest sustains** on your laptop (measure that first).

## You should see
Timeouts or errors at the sim, lag climbing, maybe OOM kills or restarts. Write your prediction first.

## Done when
- Ledger shows **0 lost** across 6 consecutive fills.
- When overloaded, ingest refuses *explicitly and fast* (no timeouts), and the sim recovers
  without making things worse.
- Accepted requests stay within your latency SLO, even during the fill.
- Lag drains back to baseline within a target you set before starting, and you can explain
  that number.
- Consumers scale out on a signal that actually reflects the backlog, and scale back in without
  losing or duplicating anything.

## Constraints
"Make the burst smaller" doesn't count. Neither does "give it more replicas forever".

## Read after
- Amazon Builders' Library: "Timeouts, retries, and backoff with jitter" and "Using load shedding
  to avoid overload"
- Google SRE Book: "Handling Overload" and "Addressing Cascading Failures"
- Little's Law
- KEDA's Kafka scaler, and why autoscaling on CPU lies here
