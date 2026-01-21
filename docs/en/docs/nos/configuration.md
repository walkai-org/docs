# Configuration

You can customize the GPU Partitioner settings by editing the values file of the [nos](helm-charts-README.md) Helm chart.
In this section we focus on some of the values that you would typically want to customize.

## Requeue interval

The GPU partitioner periodically scans all pending, unschedulable pods that request MIG resources and evaluates whether a different partitioning would make them schedulable. You can control how often this happens with:

- `requeueIntervalSeconds`: how often the controller wakes up even if no new Pod events occur.

Shorter intervals react faster to changes (for example when the scheduler no longer emits events) at the cost of more frequent reconciliation cycles. Longer intervals reduce churn but defer partitioning updates.

## Priority Awareness

The chart can provision four PriorityClasses for GPU workloads: `nos-priority-low` (0), `nos-priority-medium` (1000, global default), `nos-priority-high` (2000), and `nos-priority-extra-high` (3000). Preemption is disabled on all of them. The GPU partitioner plans MIG repartitioning in priority order (age tie-breaker) and will not plan lower-priority pods if a higher-priority pod cannot be satisfied. You can disable or override these classes via the `priorityClasses` values.


## Available MIG geometries

The GPU Partitioner determines the most proper partitioning plan to apply by considering the possible MIG geometries allowed for each of the GPU models present in the cluster.

You can set the MIG geometries supported by each GPU model by editing the `gpuPartitioner.knownMigGeometries` value of the [installation chart](helm-charts-README.md).

You can edit this file to add new MIG geometries for new GPU models, or to edit the existing ones according to your specific needs. For instance, you can remove some MIG geometries if you don't want to allow them to be used for a certain GPU model.

## How it works

The GPU Partitioner component watches for pending pods that cannot be scheduled due to lack of MIG resources they request. If it finds such pods, it checks the current partitioning state of the GPUs in the cluster and tries to find a new partitioning state that would allow to schedule them without deleting any of the used resources, taking into account the constraints presented by each GPU model's allowed MIG geometries.


### MIG Partitioning

The actual partitioning specified by the GPU Partitioner for MIG GPUs is performed by the MIG Agent, which is a daemonset running on every node labeled with `nos.nebuly.com/gpu-partitioning: mig` that creates/deletes MIG profiles as requested by the GPU Partitioner.

The MIG Agent exposes to the GPU Partitioner the used/free MIG resources of all the GPUs of the node on which it is running through the following node annotations:

- `nos.nebuly.com/status-gpu-<index>-<mig-profile>-free: <quantity>`
- `nos.nebuly.com/status-gpu-<index>-<mig-profile>-used: <quantity>`

The MIG Agent also watches the node's annotations and, every time the desired MIG partitioning specified by the GPU Partitioner does not match the current state, it tries to apply it by creating and deleting the MIG profiles on the target GPUs. The GPU Partitioner specifies the desired MIG geometry of the GPUs of a node through annotations in the following format:

`nos.nebuly.com/spec-gpu-<index>-<mig-profile>: <quantity>`

Note that in some cases the MIG Agent might not be able to apply the desired MIG geometry specified by the GPU Partitioner. This can happen for two reasons:

1. the MIG Agent never deletes MIG resources being in use by a Pod
2. some MIG geometries require the MIG profiles to be created in a certain order, and due to reason (1) the MIG Agent might not be able to delete and re-create the existing MIG profiles in the order required by the new MIG geometry.

In these cases, the MIG Agent tries to apply the desired partitioning and if it fails it rolls-back to its previous state.

For further information regarding NVIDIA MIG and its integration with Kubernetes, please refer to the [NVIDIA MIG User Guide](https://docs.nvidia.com/datacenter/tesla/pdf/NVIDIA_MIG_User_Guide.pdf) and to the [MIG Support in Kubernetes](https://docs.nvidia.com/datacenter/cloud-native/kubernetes/mig-k8s.html) official documentation provided by NVIDIA.
