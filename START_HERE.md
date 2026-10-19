# Start here

Tier-0 is a lab, not a product. It exists so that real failures happen and you learn to find
them, fix them, and explain them. The rules for working with Claude are in `CLAUDE.md`. The tools
are in `TOOLS.md`. The problems are in `tickets/`.

**Rule zero: the journal is the deliverable, not the code.** A ticket isn't done until
`incidents/NNN-*.md` is written.

---

## Phase 0: Setup and Go (about 2 weeks)

### 0.1 Install the tools (one evening)
Install the **Core** group from `TOOLS.md` and nothing else yet. You'll add the rest when a
ticket needs them.

Check it works:
- `k3d cluster create scratch --agents 3`, then open `k9s` and look around, then `k3d cluster delete scratch`
- `docker stats` while the cluster runs, so you know how much RAM an empty cluster costs you

### 0.2 Go primer (about 6–8 evenings, and no longer)
You already know Java, TypeScript, Python and C++. Go is a small language, so you're learning
*its idioms*, not how to program. Don't take a course.

1. **A Tour of Go** (go.dev/tour): 2–3 evenings. Skim generics. Take your time on methods,
   interfaces, errors, goroutines, channels and `select`.
2. **Three drills.** Each one is a piece of `ingest`, so nothing is wasted. Put them in
   `scratch/go-drills/` (they get thrown away):
   - **D1, HTTP server:** `POST /events` takes JSON, validates it, and returns 202 or 400. It
     shuts down gracefully on SIGTERM, finishing in-flight requests first. Use only the
     standard library (`net/http`, `encoding/json`, `os/signal`, `context`).
   - **D2, worker pool:** N goroutines take jobs from a channel. Cancel everything with a
     `context` timeout. Run with `go run -race` and fix whatever it finds.
   - **D3, tests:** write table-driven tests for D1's validation, plus one `httptest` test.
3. Keep **Go by Example** (gobyexample.com) open as a reference while you work.
4. Later, around Arc 2, read *100 Go Mistakes and How to Avoid Them*, a chapter at a time. That's
   where your Go gets deep.

Claude in **tutor mode** is fair game for the drills. It must not write them for you.

### 0.3 Look at the physics offline (1–2 evenings, and it's fun)
No Kubernetes yet. Before building a pipeline, understand the data.
- Get the CMS dimuon open dataset. Search opendata.cern.ch for the dataset used in ROOT's
  "df102 NanoAOD dimuon" tutorial: `Run2012BC_DoubleMuParked_Muons.root`, which has only muon
  branches. It's a few GB, so keep it **outside git** in `data/`.
- In a Python script using `uv`, `uproot`, `awkward` and `matplotlib`: keep events with exactly
  2 muons of opposite charge, compute their invariant mass, and plot a log-log histogram from 0.25
  to 300 GeV.
- **Done when** you see the peaks: η, ρ/ω, φ, J/ψ, ϒ, Z. Save this plot. It becomes **the
  offline truth** that the live pipeline must reproduce exactly, which you'll use in T2.

---

## Phase 1: Build and break, one ticket at a time

### Order (do them in this order, not by number)

| # | Ticket | Rough time |
|---|---|---|
| Arc 0 | T00 skeleton → T00.5 ledger and SLOs | 3–4 weeks |
| Arc 1 | T01 → T02 → T03 → T05 → T04 | 5–6 weeks |
| Arc 2 | T08 → T07 → T06 → T09 → **T10 operator** → T11 → T12 | 9–11 weeks |
| Arc 3 | T13 → T15 → T14 (game days spread over 4 weeks) | 4–5 weeks |
| Arc 4 | T16 → (T17) | 3 weeks |
| Optional | GPU trigger | whenever |

That assumes about **10 hours a week** alongside your studies. During exam periods, pause the
tickets and only do reading plus an interviewer-mode session.

### Checkpoint: CERN openlab (the call usually closes around late January, so check the date)
A realistic target for that deadline: **Arc 0 and Arc 1 done, with 6+ journal entries, a README
with the live dimuon plot, and an architecture diagram.** Arc 2 won't be done by then, and that's
fine. A complete Arc 1 with good write-ups beats a half-finished Arc 2.

### Each ticket's loop
1. Read the ticket. Write your **hypothesis about what will break** at the top of a new journal
   entry *before* touching anything.
2. **Reproduce** the failure. Record the "before" number and take a screenshot of the graph.
3. Investigate. Write down each hypothesis, *including the wrong ones*, as you go, not afterwards.
4. Fix it. Record the "after" number.
5. **Read after the struggle:** 1–2 hours on the ticket's "Read after" sources. Then add to the
   journal what you'd now do differently.
6. Fill in "What breaks at 100×".
7. 20 minutes of Claude **interviewer mode** on that journal entry. Write the questions you
   couldn't answer into the journal under "Gaps".
8. Commit with a message like `T02: done, 0 lost / 0 harmful dups over 50 kills`.

### When you're stuck
- 0–45 minutes: you on your own, using docs, `kubectl describe`, events, logs and metrics.
- After 45 minutes: ask Claude for **one rung** of the hint ladder (CLAUDE.md), then go back to work.
- Stuck for more than 2 days on the same thing: write up the partial state in the journal, take
  the root cause from Claude, and **then rebuild the failure from scratch yourself** to make sure
  you understand it.

---

## T00: concrete steps (the only ticket with steps, because it's the bootstrap)

Everything after T00 has a symptom and a "done when" only. T00 gets steps so you aren't stuck
before anything exists. The code is still yours.

1. **Repo and cluster as code.** `git init`, use the layout below, and a `justfile` with `up` and
   `down`. Use `ctlptl` to create the k3d cluster plus a local image registry. `just up` from
   nothing must give you a working cluster.
2. **GitOps from day one.** Install ArgoCD and use an app-of-apps pointing at `deploy/`. After
   this, nothing gets deployed with a manual `kubectl apply`.
3. **Platform pieces, in this order:** Redpanda (1 broker, small memory), MinIO, CNPG with a
   1-instance Postgres, then kube-prometheus-stack (trimmed). Check `docker stats` after each one
   and write the RAM numbers in `design/budget.md`.
4. **`detector-sim`** (any language): reads the ROOT file, gives each event an `event_id`, and
   sends events to ingest at a configurable rate. Add the "fill" pattern later in T01.
5. **`ingest`** (Go, built from your drills): `POST /events` → validate → produce to the
   Redpanda topic `raw-events`. You choose the partition count and key. Write down why.
6. **`trigger`** (Python): consume → select dimuon events → compute mass → update the histogram →
   batch the accepted events into Parquet in MinIO → add one row per file to the catalog.
7. **`api`**: serves the current histogram. Show it with a tiny page or a Grafana panel.
8. **Metrics:** every service exposes Prometheus metrics. One dashboard: events/s in and out at
   each hop, consumer lag, error rate.
9. **Tilt** for the inner loop, so you edit code and the cluster updates. Set this up once the
   services exist.

**Done when:** `just down && just up` from nothing ends with the peaks appearing live, and every
service is deployed by ArgoCD from git.

---

## Working with Claude

- **Open Claude in this folder**, so it reads `CLAUDE.md`. In VS Code: File → Open Folder →
  `lhc_data`. In a terminal: `cd lhc_data && claude`.
- **One session per ticket.** Start a fresh one for each ticket, come back to it later with
  `claude --continue` (or pick it from the session list in VS Code), and use `/clear` for
  unrelated questions.
- **Claude doesn't remember between sessions.** The ticket `Status:` lines and `incidents/` are
  its memory, so keep them up to date.
- **Use the default permission mode** (Claude asks before running commands) when working
  tickets. Auto mode is only for game-master sessions.
- **Questions while reading, away from the laptop:** create a Project on claude.ai and paste
  `CLAUDE.md` in as its instructions.

## Repo layout

```
CLAUDE.md  START_HERE.md  TOOLS.md  justfile  Tiltfile
detector-sim/     ingest/ (Go)     trigger/ (Python)     api/     ledger/
operator/ (Go, kubebuilder, from T10)
deploy/           # Helm values + ArgoCD apps
infra/            # Terraform (Arc 4)
scripts/          # chaos, load, kill loops
tickets/          # problems + status
incidents/        # your journal (the real output)
design/           # SLOs, budget, arc-at-scale docs, ADRs
data/             # ROOT files (gitignored)
scratch/          # drills, throwaway
```

## Cut list (if time runs out, drop these first)
T17 → GPU → T12 → T04. Never drop T02, T07, T10 or T14.
