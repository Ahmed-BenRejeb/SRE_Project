# T00 — Walking skeleton
Status: doing · Arc 0

The steps are in `START_HERE.md`. This is the only ticket that has steps.

## Done when
- `just down && just up` from nothing ends with the dimuon peaks appearing live.
- Every service is deployed by ArgoCD from git. No manual `kubectl apply` anywhere.
- One dashboard shows events/s in and out at every hop, plus consumer lag.
- `design/budget.md` records the RAM cost of each component.

## Journal
Not an incident, but write `design/arc0-architecture.md`: a diagram, and why you chose each
partition count, key, batch size and flush interval. Those numbers will be wrong, and later
tickets will show you why.
