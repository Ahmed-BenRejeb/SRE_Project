# T12 — etcd quorum loss
Status: todo · Arc 2

Do **Kubernetes the Hard Way** first if the control plane still feels like magic.

## Setup
A k3d cluster with 3 servers (k3s embedded etcd). Take a snapshot, create some DataRuns, then
stop 2 servers.

## Done when
- You can explain what still works during quorum loss (reads? writes? running pods? traffic?) and
  *why*: control plane vs data plane.
- Restored from the snapshot. You can explain what happened to objects created after the
  snapshot was taken, and how your operator and ArgoCD reacted.
- A dashboard with etcd metrics: DB size, fsync latency, leader changes.

## Read after
- The Raft paper ("In Search of an Understandable Consensus Algorithm") and raft.github.io
- The etcd operations guide
- k3s etcd snapshot docs
