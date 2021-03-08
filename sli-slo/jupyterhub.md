# Service Level Indicators

SLIs are carefully defined quantitative measures of some aspect of the level of service that is provided. You can find more information on SLIs in the [Site Reliability Engineering Book](https://landing.google.com/sre/sre-book/chapters/service-level-objectives).

| SLI                                    | Query                                                                                                     | Status                        |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------|-------------------------------|
| Availability                           | up{job="JupyterHub Metrics"} == 1                                                                             | Done                          |
| Latency             | < to-be-implemented >    | In Progress |
| Quality                  |           < to-be-implemented >                                              | In Progress                          |

## SLI Descriptions

1. **Availability:** This tells us whether the Jupyterhub service is available or not.
2. **Latency:** This tells us how long it took Jupyterhub to process a request.
3. **Quality:** This tells us the performance of Jupyterhub such as tracking CPU/Memory usage

## Usage Metrics

These metrics are being used to describe/highlight important user activity when using the Jupyterhub service.

| Usage Metric              | Query                                                                 | Status   |
|---------------------------|-----------------------------------------------------------------------|----------|
| Number of Pods running | count(count(container_cpu_usage_seconds_total{namespace="dh-prod-jupyterhub",pod_name=~".*jupyterhub-nb.*"}) by(pod_name)) | Done     |
| Number of GPU Enabled Pods Running    | count(container_accelerator_duty_cycle{pod_name=~".*jupyterhub-nb.*"})                        | Done     |
| Total Number of Users    | < metric-not-supported-in-current-JH version>        | On Hold      |
| Number of User Servers currently Running            | < metric-not-supported-in-current-JH version>   | On Hold |
| Number of User PVCs exceeding 90% capacity            | count(kubelet_volume_stats_used_bytes{namespace='dh-prod-jupyterhub'}/kubelet_volume_stats_capacity_bytes{namespace='dh-prod-jupyterhub'} > 0.9)   | Done |
| Number of OOMKilled Pod Events            | < metric-not-supported-in-current-kube state metric version >  | On Hold |

### Possible Service Level Indicators

These metrics are interesting to look at but their usefulness has not yet been determined.

| Metric                                 | Query                                                                                                                                                                                                                             | Possible Usage | Status                  |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|-------------------------|
| Number of Server Spawn Operations             | server_spawn_duration_seconds_count{job="JupyterHub Metrics"} | SRE SLI  | Done                    |
| Hub Pod Restart Count           | kube_pod_container_status_restarts_total{namespace="dh-prod-jupyterhub", pod=~"jupyterhub-nb.*"}                                                                            | SRE SLI        | Done                    |
| Error Codes Count    | request_duration_seconds_count{job="JupyterHub Metrics", code="$error_codes"}                                                                                                                                                                                                            | SRE SLI  | Done                |
| CPU Usage       | sum(rate(container_cpu_usage_seconds_total{pod_name=~".*jupyter.*"}[2m])) by (pod_name)                                                                                                                         | SRE SLI        | Done |
| Memory Usage | sum(container_memory_rss{pod_name=~".*jupyter.*"}) by (pod_name)                                                                                                                         | SRE SLI        | Done |
| Container Creation Errors (grouped by each user's pod) | (kube_pod_container_status_waiting_reason{namespace="dh-prod-jupyterhub",pod=~"jupyterhub-nb-.*", reason="$reason"} == 1)                                                                                                                         | SRE SLI        | Done |

## Metric Descriptions

1. **Number of Server Spawn Operations:** A spawner starts each single-user notebook servers, this metric keeps track of the number of server spawner operations
2. **Hub Pod Restart Count:** This metric keeps track of the number of restarts each pod undergoes
3. **Pod Error Codes Count:** This metric keeps track of the number of different HTTP request error codes
4. **CPU Usage:** This metric describes the CPU usage for each user's pod
5. **Memory Usage:** This metric describes the memory usage for each user's pod
6. **Container Creation Errors:** This metric describes the status of the user's pod during the container creation and if any errors are observed such as "ErrImgPull, ImagePullBackOff, CrashLoopBackoff"
