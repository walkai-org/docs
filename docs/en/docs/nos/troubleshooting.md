# Troubleshooting

If you run into issues with Automatic GPU Partitioning, you can troubleshoot by checking the logs of the GPU Partitioner and MIG Agent pods. You can do that by running the following commands:

Example 1 GPU:
```yaml
resources:
        limits:
          nvidia.com/gpu: 1
```
Example MIG:
```yaml
resources:
        limits:
          nvidia.com/mig-1g.10gb: 1
```
Check GPU Partitioner logs:

```shell
 kubectl logs -n nos-system -l app.kubernetes.io/component=nos-gpu-partitioner -f
```

Check MIG Agent logs:

```shell
 kubectl logs -n nos-system -l app.kubernetes.io/component=nos-mig-agent -f
```


