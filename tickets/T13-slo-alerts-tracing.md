# T13 — SLO alerting and tracing
Status: todo · Arc 3

## Done when
- Multi-window, multi-burn-rate alerts for your SLOs: **written by hand first**, then compared
  with what Pyrra or Sloth generates.
- Alerts reach your phone (ntfy).
- A trace covers sim → ingest → Redpanda → trigger → MinIO, with context propagated through
  Kafka headers.
- The biggest p99 contributor found using traces alone.
- Compared your manual instrumentation with OBI's eBPF zero-code version: what each one sees
  that the other misses.

## Read after
- Google SRE Workbook: "Alerting on SLOs"
- OpenTelemetry context propagation
