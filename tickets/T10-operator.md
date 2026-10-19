# T10 — The DataRun operator
Status: todo · Arc 2

**The biggest ticket of the project. Give it about 3 weeks.**

## Read before starting
The Kubebuilder book, including its CronJob tutorial (book.kubebuilder.io).

## Setup
A `DataRun` CRD with fields for: run number, rate, fill pattern, trigger selection, retention.
Reconciling it creates the topic, the bucket prefix, the catalog row, and the sim and trigger
workloads. It reports meaningful status conditions. A finalizer cleans up according to retention.

## Sabotage it
- Kill the operator in the middle of a reconcile (add a sleep to make the window wide).
- Delete child objects behind its back.
- Edit child objects by hand (drift).
- Run 2 replicas without leader election, then with it.
- Make the Redpanda admin API fail or time out.

## Done when
- It converges every time, for every sabotage above.
- envtest tests cover the reconcile paths.
- You can explain level-triggered vs edge-triggered reconciliation, informer cache staleness, why
  reconcile must be idempotent, owner references and garbage collection, and finalizers, all
  with examples from *your* code.

## Read after
- *Programming Kubernetes* (Hausenblas and Schimanski)
- controller-runtime's workqueue and rate-limiter source
- Kubernetes API conventions, especially the section on conditions
