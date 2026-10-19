# T17 — Home → cloud dispatch (stretch)
Status: todo · Arc 4

## Setup
The home cluster submits jobs that Kueue's MultiKueue dispatches to the EKS cluster, over
WireGuard or Tailscale.

## Done when
A job submitted at home runs in the cloud, and its results come back. Cutting the tunnel halfway
through doesn't lose or duplicate work.
