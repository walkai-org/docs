# Installation

`nos` is distributed as a Helm 3 chart.

If you want `nos` to send cluster insights to the `walk:ai` backend (or your own API), create a `values.yml` file and enable the telemetry exporter:

```yml
clusterInfoExporter:
    config:
      enabled: true
      endpoint: https://api.example.com/cluster/insights
      interval: 30s
      httpTimeout: 15s
    secret:
      create: true
      apiToken: "<your-token>"
```
Then install the chart, pointing Helm at your values.yml:

```bash
helm install oci://ghcr.io/walkai-org/helm-charts/nos \                                                                      
  --version 0.0.3 \
  --namespace nos-system \
  --generate-name \
  --create-namespace \
  -f values.yml
```

If you don't need the telemetry module, you can omit `-f values.yml` and the `ClusterInfoExporter` daemonset will not be deployed.

You can find all the available configuration values in the Chart [documentation](helm-charts-README.md).
