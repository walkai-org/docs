# Installation

You can install `nos` using Helm 3.

```bash
helm install oci://ghcr.io/walkai-org/helm-charts/nos \                                                                      
  --version 0.0.3 \
  --namespace nos-system \
  --generate-name \
  --create-namespace
```

If you wish to receive insights about your cluster, you should create a values.yml and override the associated configuration:
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

You can find all the available configuration values in the Chart [documentation](helm-charts-README.md).

