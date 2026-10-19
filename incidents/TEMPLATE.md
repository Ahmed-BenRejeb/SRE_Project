# NNN — <short title>

- **Ticket:** T__
- **Date(s):**
- **Time spent:**

## Prediction (write BEFORE reproducing)
What I expect to break, and why.

## Symptom
What a user or operator would see. Include a screenshot or graph.

## Impact
At least one number: events lost, requests failed, minutes of SLO burned, seconds of RTO.

## Timeline
| Time | What I saw / did |
|---|---|

## Hypotheses
| # | Hypothesis | How I tested it | Result |
|---|---|---|---|
| 1 | | | ✅ / ❌ |

Keep the wrong ones in. They're the most interesting part of the story.

## Root cause
The mechanism, not just the config change. Explain it as if to someone who knows Kubernetes but
not this system.

## Fix
What changed, and why this fix rather than the alternatives you considered.

## Evidence
| Metric | Before | After |
|---|---|---|

Graphs, ledger output, logs.

## Read after
What I read, and what it changed in my understanding.

## What breaks at 100×
Same system, 100× the traffic, data or nodes. What fails first, and what would I change?

## Gaps (from interviewer mode)
Questions I couldn't answer well, and the answers once I've found them.

## The 2-minute version
How I'd tell this story in an interview: situation, what I did, result with a number, what I
learned.
