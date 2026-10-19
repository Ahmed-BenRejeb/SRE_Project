# TGPU — GPU trigger classifier (optional)
Status: todo

## Setup
A small classifier (signal vs background) in the trigger, running on the GTX 1050 through the
NVIDIA device plugin with time-slicing.

**Heads up:** the 1050 is a Pascal card. Recent NVIDIA drivers dropped Pascal, so on Arch you'll
probably need a legacy driver branch. Check the Arch wiki and Arch news before starting.
Getting the GPU visible inside k3d is part of the ticket.

## Done when
- Latency and throughput compared between CPU and GPU, with the batch size that makes the GPU
  worth using.
- Two pods sharing the GPU through time-slicing, with the trade-off explained.
