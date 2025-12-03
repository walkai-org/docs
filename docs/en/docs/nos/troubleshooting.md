# Troubleshooting

If you run into issues with Automatic GPU Partitioning, you can troubleshoot by checking the logs of the GPU Partitioner and MIG Agent pods. You can do that by running the following commands:

Check GPU Partitioner logs:

```shell
 kubectl logs -n nos-system -l app.kubernetes.io/component=nos-gpu-partitioner -f
```

Check MIG Agent logs:

```shell
 kubectl logs -n nos-system -l app.kubernetes.io/component=nos-mig-agent -f
```
