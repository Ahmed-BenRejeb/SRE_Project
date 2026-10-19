# T15 — Capacity model
Status: todo · Arc 3

## Done when
Answered with data, not guesses:
- The maximum sustainable events/s for the whole pipeline on this laptop.
- Which resource saturates first in each component (use the USE method).
- The hottest service profiled with pprof and Pyroscope, plus **one** optimisation with a
  measured gain.
- A model: nodes needed for N events/s, and **$ per million events** on AWS.

## Read after
- Brendan Gregg, "The USE Method"
- Optional: the Universal Scalability Law
