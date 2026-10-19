# T11 — Cilium, NetworkPolicy, DNS
Status: todo · Arc 2

## Setup
Recreate the cluster with flannel disabled and Cilium installed. Getting that working on k3d is
part of the ticket. Default-deny in every namespace, then allow only the flows you need.

## You should see
Something breaks silently. It will.

## Done when
- Every allowed flow is documented with a reason.
- The broken flow was found **with Hubble**, not by guessing.
- DNS lookups per outgoing request measured with default `ndots` and with a tuned setting, using
  Inspektor Gadget, and the difference explained.

## Read after
- Kubernetes DNS for Services and Pods; `resolv.conf` search domains and `ndots`
- Cilium identity-based policy, and why it's not IP-based
