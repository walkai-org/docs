---
hide:

- toc

---

# Prerequisites

1. Kubernetes version 1.23 or newer
2. [MIG](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/) enabled on your GPU
3. NVIDIA GPU Operator
4. [Cert Manager](https://github.com/cert-manager/cert-manager) (optional, but recommended)

## Enable MIG

Enable MIG mode on your GPU:

```bash
sudo nvidia-smi -i 0 -mig 1
```

Verify that MIG mode is active:

```bash
nvidia-smi -i 0 -q | grep -A3 "MIG Mode"
```

!!! note
    A reboot may be required for the change to take effect.

## NVIDIA GPU Operator

Before installing `nos`, you must enable GPU support in your Kubernetes cluster, and we recommend using the NVIDIA GPU Operator for that.

Create a `values.yml` file with the required tolerations for the `nos.nebuly.com/repartitioning=planned:NoSchedule` taint used during GPU repartitioning. You can start from [values.yml](../values.yml) and customize it for your cluster.

You can install the [NVIDIA GPU Operator](https://github.com/NVIDIA/gpu-operator) as follows:

```bash
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia && helm repo update
helm install --wait --generate-name \
     -n gpu-operator --create-namespace \
     nvidia/gpu-operator --version v22.9.0 \
     -f values.yml
```

If you set `driver.enabled=true` in the values file, the GPU Operator will automatically install a recent version of NVIDIA Drivers and CUDA on all the GPU-enabled nodes of your cluster, so you don't have to manually install them.

For further information you can refer to the [NVIDIA GPU Operator Documentation](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/getting-started.html).
