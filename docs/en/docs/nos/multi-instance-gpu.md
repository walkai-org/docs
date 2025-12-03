# Multi-instance GPU (MIG)

Multi-instance GPU (MIG) is a technology available on NVIDIA Ampere and newer architectures that allows a single physical GPU to be securely partitioned into separate GPU instances for CUDA applications, each fully isolated with its own high-bandwidth memory, cache and compute cores.

The isolated GPU slices are called MIG devices, and they are named adopting a format that indicates the compute and memory resources of the device. For example, 2g.20gb corresponds to a GPU slice with 20 GB of memory.

## Limitations of MIG
MIG is the GPU sharing approach that offers the strongest isolation between workloads, but it comes with an important trade-off: flexibility.

First, MIG does not allow arbitrary slice sizes or counts. Each GPU model only supports a [fixed set of MIG profiles](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#supported-profiles), which limits how granularly you can partition the device. 

This also makes the configuration structurally rigid: at any point in time, the profiles you can schedule are constrained by the current layout of the GPU.

For example, if an H100 is configured as:
- `4g.40gb x 1`
- `1g.10gb x 3`

and a Pod requests a `2g.20gb` profile, that Pod cannot be scheduled, even if the GPU is completely idle, because there is no compatible slice in the current configuration. It would only be schedulable after reconfiguring the device to include a `2g.20gb` profile.

Second, with the tools that NVIDIA provides today, such as [NVIDIA GPU Operator](https://github.com/NVIDIA/gpu-operator) or [mig-parted](https://github.com/NVIDIA/mig-parted), all running workloads have to be evicted in order to change the current configuration. 

This makes dynamic reconfiguration disruptive and hard to use in practice.

## How `nos` improves MIG-based scheduling

`nos` is designed to mitigate these limitations. Through our MIG Agent, it can dynamically reconfigure a GPU’s unused MIG slices at runtime, without touching slices that are currently in use. This allows the cluster to adapt the MIG layout to incoming workloads while keeping existing jobs running undisturbed.


You can find out more on how MIG technology works in the official [NVIDIA MIG User Guide](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/).