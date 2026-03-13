# Installation

`nos` is distributed as a Helm 3 chart.

If you want `nos` to send cluster insights to the `walk:ai` backend (or your own API), create a `values.yml` file and enable the telemetry exporter:

```yml
clusterInfoExporter:
    enabled: true
    config:
      endpoint: https://api.example.com/cluster/insights
      interval: 3s
      httpTimeout: 5s
    secret:
      create: true
      apiToken: "<your-token>"
gpuPartitioner:
  migAgent:
      reportConfigIntervalSeconds: 3
  preemption:
    enabled: true
```

Then install the chart, pointing Helm at your values.yml:

```bash
helm install oci://ghcr.io/walkai-org/helm-charts/nos \
  --version 0.0.9 \
  --namespace nos-system \
  --generate-name \
  --create-namespace \
  -f values.yml
```

!!! note
    If you don't need the telemetry module, you can omit `-f values.yml` and the `ClusterInfoExporter` daemonset will not be deployed.

!!! note
    You can find all the available configuration values in the Chart [documentation](helm-charts-README.md).

After that, you can fetch the token the application uses for its cluster configuration:
```bash
kubectl get secret api-client-permanent-token -n walkai   -o jsonpath='{.data.token}' | base64 -d; echo
```