KeyDB
=====
A Helm chart for a replicated [KeyDB](https://keydb.dev/) cluster optionally with a Redis module loaded. ServiceMonitor and Redis Exporter sidecar are included. 

## Install

```
$ helm install keydb --namespace redis ./ -f values.yaml --debug
```

## Chart Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| replicaCount | int | `3` | Number of Pods to create. |
| keydb.password | string | `"CHANGEMEFIRST"` | Every deployment should have one. |
| keydb.activereplica | string | `"yes"` | ["Active-Active" Replication](https://docs.keydb.dev/docs/active-rep/) |
| keydb.multimaster | string | `"yes"` | ["Multiple-Master" Replication](https://docs.keydb.dev/docs/multi-master/) |
| keydb.appendonly | string | `"no"` | Append Only File [persistence](https://docs.keydb.dev/docs/persistence/) |
| keydb.module | string | `nil` | A custom [Redis Module](https://redis.io/modules) to load e.g. "redistimeseries.so" |
| keydb.threads | int | `2` | Number of worker threads serving requests. This number should be related to the performance of your network hardware, not the number of cores on your machine. |
| keydb.port | int | `6379` |  |
| keydb.extraArgs | object | `{}` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"pozetroninc/keydb"` | The Docker (Hub) repository for the image. |
| image.tag | string | `"v6.0.16"` | The version of KeyDB to install e.g. "v5.3.3" |
| persistence.enabled | bool | `false` |  |
| persistence.size | string | `3G` | How much persistent storage for each pod. |
| persistence.storageClass | string | `` | For no storage class enter "-", empty or not specifying will result in default storage class |
| persistence.accessMode | string | `ReadWriteOnce` | The [storage class](https://kubernetes.io/docs/concepts/storage/storage-classes/) |
| serviceMonitor.enabled        | bool | `true` | Prometheus operator ServiceMonitor |
| serviceMonitor.labels         | sting | `{}` | Additional labels for ServiceMonitor |  
| serviceMonitor.annotations    | string | `{}` | Additional annotations for ServiceMonitor |
| serviceMonitor.interval       | string | `30s` | ServiceMonitor scrape interval |
| serviceMonitor.scrapeTimeout  | string | `nil` | ServiceMonitor scrape timeout |
| exporter.enabled              | bool   | `true` | Prometheus Exporter sidecar contaner |
| exporter.image                | string | `oliver006/redis_exporter:v1.12.1-alpine` | Exporter Image
| exporter.pullPolicy           | string | `IfNotPresent` | Exporter imagePullPolicy |
| exporter.port                 | int    | `9121` | `prometheus.io/port` |
| exporter.scrapePath           | string | `/metrics` | `prometheus.io/path` |
| exporter.livenessProbe        | string | Look values.yaml | LivenessProbe for sidecar Prometheus exporter |
| exporter.readinessProbe       | string | Look values.yaml | ReadinessProbe for sidecar Prometheus exporter |
| exporter.startupProbe         | string | Look values.yaml | StartupProbe for sidecar Prometheus exporter |
| exporter.resources            | array | `{}` | Resources for sidecar Prometheus container |
| exporter.extraArgs            | array | `{}` | Additional arguments for exporter |

Features
=====
Based on Pozetron chart with added redis exporter and servicemonitor