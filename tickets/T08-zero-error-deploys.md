# T08 — Zero-error deploys
Status: todo · Arc 2

## Setup
Under steady load, a script rolls out ingest 20 times. Then the same for trigger.

## You should see
5xx errors and connection resets at the sim during every rollout. For trigger: duplicate or lag
spikes.

## Done when
- **0 failed requests** across 20 ingest rollouts under load.
- Trigger rollouts stay within a duplicate and lag budget you set beforehand.
- You can draw the race condition on a whiteboard.

## Read after
- Pod termination lifecycle; SIGTERM; preStop hooks
- EndpointSlice propagation and why it lags behind pod deletion
- Readiness vs liveness probes
- Consumer-group rebalancing protocols (eager vs cooperative)
