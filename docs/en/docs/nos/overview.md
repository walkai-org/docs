# nos

`walk:ai nos` is the in-cluster component of the platform. It runs on your Kubernetes cluster and is responsible for two main tasks: dynamically partitioning GPUs so that Pods can share them efficiently, and exporting GPU and workload telemetry that powers the `walk:ai` application.

## Origin and architecture

`walk:ai nos` is a fork of [`nos` by nebuly-ai](https://github.com/nebuly-ai/nos). We reuse its MIG Agent and GPU Partitioner components and extend them to support dynamic reconfiguration of MIG based on the current workloads. On top of that, we add our own telemetry pipeline that continuously publishes cluster and GPU state to the `walk:ai` backend.


## Dynamic GPU partitioning

`walk:ai nos` allows Pods to request fractions of a physical GPU. Instead of assigning entire devices to a single Pod, GPUs are automatically split into MIG slices that can be requested by individual containers (for example, `1g.10gb`, `3g.40gb`, etc.). This lets multiple Pods share the same GPU and increases overall utilization.

The partitioning is adjusted in real time:

- `walk:ai` continuously watches pending Pods that request GPU fractions.
- Given the current and pending workload, the GPU Partitioner computes the best MIG configuration it can apply to fit as many of those Pods as possible.
- The MIG Agent then applies that configuration on the nodes, creating or tearing down MIG instances as needed.

Under the hood, the GPU partitioning is implemented using [NVIDIA Multi-Instance GPU (MIG)](multi-instance-gpu.md), with [runtime reconfiguration and scheduling improvements](multi-instance-gpu.md#how-nos-improves-mig-based-scheduling)

## Telemetry and cluster visibility

Beyond partitioning GPUs, `walk:ai nos` also acts as the telemetry layer of the platform. The in-cluster agents:

- Collect low-level GPU metrics (utilization, memory usage, MIG layout, allocations per Pod).
- Observe Jobs, Pods and their GPU requests.
- Periodically push a summarized view of this state to an api endpoint you configure during installation.

This funcionality is opt-in. If you do not declare the configuration related to it, the daemonset will not be deployed.

