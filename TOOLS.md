# Tools

Install **Core** now. Install everything else **when its ticket arrives**. Installing 40 tools up
front only feels like progress.

Most of these install through `mise` (`mise use -g <tool>`; check names with
`mise registry | grep <tool>`). Use pacman or the AUR for anything that isn't there.

## Core (Phase 0)

| Tool | What it's for in this project |
|---|---|
| **mise** | One version manager for go, kubectl, helm, terraform and so on. Pin versions in `mise.toml` in the repo. |
| **just** | Command runner (`just up`, `just chaos-kill`). Easier to read than Makefiles. |
| **direnv** | Per-repo environment variables (KUBECONFIG, AWS_PROFILE) loaded automatically when you `cd` into the folder. |
| **k3d** + **ctlptl** | Local cluster plus a local image registry, declared in a file. |
| **kubectl**, **helm**, **argocd** CLI | Basics. |
| **k9s** | Terminal UI for the cluster. Learn `:events`, `:pulse`, `:xray`, log view, and `shift-f` port-forward. |
| **kubectx / kubens** | Switch context and namespace quickly. |
| **stern** | Tail logs from many pods at once (`stern -n tier0 'ingest\|trigger'`). |
| **Tilt** | Inner dev loop: edit code and the cluster rebuilds and redeploys in seconds. Has a web UI per service. |
| **go**, **gopls**, **golangci-lint**, **delve** | Go toolchain, linter, debugger. |
| **uv**, **ruff** | Python environments and lint/format (fast). |
| **jq**, **yq**, **fzf** | JSON/YAML wrangling and fuzzy finding. |

## Arc 0–1: Data and streaming

| Tool | Why |
|---|---|
| **rpk** + **Redpanda Console** | Topics, partitions, consumer-group lag, offset resets. You'll live in `rpk group describe`. |
| **DuckDB** | Query your Parquet files in MinIO with SQL directly. This is how you measure T04. |
| **mc** (MinIO client) | Count and list objects, set lifecycle rules. |
| **kubectl cnpg** plugin | Postgres cluster status, promote, backups (T06). |
| **pgcli**, **pgbench** | Postgres shell and load generation. |
| **k6** (you know it), **oha** | Load generation. oha is a quick terminal HTTP load tool with a live latency histogram. |
| **toxiproxy** | Add latency, packet loss or connection cuts *between two specific services* (ingest ↔ Redpanda). More precise than cluster-wide chaos. |

## Arc 2: Kubernetes internals

| Tool | Why |
|---|---|
| **krew** (kubectl plugin manager), with plugins `tree`, `neat`, `resource-capacity`, `who-can` | `tree` shows owner references, which is essential for the operator (T10). `resource-capacity` compares requests and limits to actual usage (T09). `neat` strips noise from YAML. |
| **`kubectl debug`** + the **netshoot** image | Ephemeral debug containers with tcpdump, dig, curl and ss inside a pod's network namespace. Built into kubectl. |
| **Inspektor Gadget** (`kubectl gadget`) | eBPF tracing aimed at pods: DNS queries, TCP connections, OOM kills, exec calls. T11's DNS measurement. The command syntax changed recently, so check `--help` for your version. |
| **Cilium CLI** + **Hubble** | CNI with a flow-level network view: see dropped packets and the policy that dropped them (T11). |
| **kubebuilder**, **envtest** | Operator scaffolding plus a real API server for tests (T10). |
| **popeye** | Cluster linter (from the k9s author). Run it once per arc and fix what it finds. |
| **etcdctl** | Talk to k3s's embedded etcd directly (T12). |

## Arc 3: SRE

| Tool | Why |
|---|---|
| **kube-prometheus-stack**, **Loki**, **Grafana Alloy** | You already know these. |
| **Tempo** + **OpenTelemetry SDKs** | Traces across ingest → Redpanda → trigger. Instrument by hand first. |
| **OpenTelemetry eBPF Instrumentation (OBI)**, formerly Grafana Beyla | Zero-code tracing using eBPF. Compare it with your manual instrumentation in T13: what does it see that yours doesn't, and the reverse? Still pre-1.0. |
| **Pyroscope** + Go **pprof** | Continuous profiling, to find where CPU actually goes (T15). |
| **Pyrra** or **Sloth** | Generate SLO burn-rate alert rules, *after* you've written one set by hand. |
| **Chaos Mesh** | Scheduled fault injection: pod kill, network delay or partition, IO faults, time skew (T14). |
| **ntfy** | Push alerts to your phone so a real page interrupts you (T13/T14). |

## Arc 4: AWS

| Tool | Why |
|---|---|
| **terraform** (or OpenTofu), **tflint** | Infrastructure as code. |
| **Infracost** | Shows the $ cost of a `terraform plan` *before* you apply. Important with only $120. |
| **eks-node-viewer** | Live view of Karpenter nodes, their bin-packing, and $ per hour. |
| **AWS FIS** | Inject real spot interruptions (T16). |
| **Steampipe** | Query your AWS account with SQL. Use it to find leftover resources after `destroy`. |
| **aws-nuke** (the maintained ekristen fork) | Last-resort cleanup. **Destructive:** use dry-run and only on an account holding nothing else. |

## Diagrams and practice

- **Excalidraw** or **D2**: architecture and timeline diagrams for journals and design docs.
- **SadServers** (sadservers.com): broken-Linux and Kubernetes scenarios against the clock.
  About 20 minutes a week of debugging practice on systems you didn't build.
- **Killercoda**: free browser-based Kubernetes scenarios, useful for trying something without
  touching your cluster.
- **Kubernetes the Hard Way** (Kelsey Hightower): build a cluster by hand. Do it before T12, or
  any time the control plane still feels like magic.

## Off during tickets

Tools that diagnose for you: **k8sgpt**, **HolmesGPT**, Headlamp's AI assistant, Claude running
diagnostics. Using them on a ticket means the root cause wasn't found by you, so the story isn't
yours. They're fine *after* a postmortem, to compare against what you found.

GUIs: **Headlamp** (the official successor to the archived Kubernetes Dashboard) or
**Freelens** if you want one. k9s is enough for most of this project.
