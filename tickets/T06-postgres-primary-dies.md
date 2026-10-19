# T06 — Postgres primary dies
Status: todo · Arc 2

## Setup
Run a CNPG cluster with 3 instances. During a fill, kill the primary pod. Then, separately,
destroy the primary's volume.

## You should see
A new primary within seconds, but the apps keep erroring for much longer. Why?

## Done when
- RTO measured: how long writes fail, as seen *by the app*, not by CNPG.
- RPO measured: catalog rows lost, checked against the ledger.
- Tried with **both** async and sync replication, with RTO/RPO numbers for each and a written
  choice.
- Apps recover automatically within a target you set.
- A decision on connection pooling (CNPG Pooler / PgBouncer, or not), with the reason.
- Backups go to MinIO, and **an actual point-in-time restore** into a new cluster has been tested
  and compared against the ledger. An untested backup doesn't count as a backup.

## Read after
- CNPG docs: failover, synchronous replication, Pooler
- How Postgres streaming replication works
- DDIA, ch. 5 (Replication)
