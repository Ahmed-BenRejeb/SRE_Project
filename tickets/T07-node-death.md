# T07 — Node death
Status: todo · Arc 2

## Setup
`docker stop` the k3d agent that runs an ingest pod and a stateful pod (a Redpanda broker or a
CNPG replica). Time everything.

## You should see
Stateless pods take minutes to move. A stateful pod gets stuck and never moves on its own.

## Done when
- A timeline of exactly what the control plane did and when: node conditions, taints, evictions,
  events. Explain each step.
- Recovery time for stateless workloads reduced, with a justified setting, *and what that
  setting costs you*.
- Topology spread constraints and PDBs in place.
- A safe, documented procedure for recovering a stateful pod on a dead node, and an explanation
  of **why Kubernetes refuses to do it automatically**.
- Bonus: network-partition the node (iptables or tc) instead of stopping it, and explain why the
  behaviour differs.

## Read after
- The node lifecycle controller and taint-based evictions
- StatefulSet "at most one" semantics
- The KEP on non-graceful node shutdown
