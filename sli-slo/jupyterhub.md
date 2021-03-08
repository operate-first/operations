# Service Level Indicators

SLIs are carefully defined quantitative measures of some aspect of the level of service that is provided. You can find more information on SLIs in the [Site Reliability Engineering Book](https://landing.google.com/sre/sre-book/chapters/service-level-objectives).

| What are we monitoring?                     | Category     | Component | Comments                                                                                                                                       | Expression                                                                                                                                                                                                                        |
| ------------------------------------------- | ------------ | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Status of Jupyterhub                        | Availability | Hub       | This expression reports the current availability status of jupyterhub master.                                                                  | up{service="jupyterhub"}                                                                                                                                                                                                            |
| TODO: Availibility/Traffic for JH DB        | Availability | DB        | The ODH team is working on monitoring this                                                                                                     |                                                                                                                                                                                                                                   |
| Notebook container creation errors          | Error        | Users     | This keeps a track of the errors encountered when spawning a notebook container.                                                               | count(kube\_pod\_container\_status\_waiting\_reason{pod=\~"jupyterhub-nb-.\*", reason=\~"CrashLoopBackOff\|ErrImagePull\|ImagePullBackOff"} == 1) by(reason,pod)                                                                    |
| Jupterhub Notebook Spawn Success Rate       | Error        | Users     | This keeps a track of what fraction of notebook servers have succeeded in spawning                                                             | sum(jupyterhub\_server\_spawn\_duration\_seconds\_count{status="success"})/sum(jupyterhub\_server\_spawn\_duration\_seconds\_count{})                                                                                             |
| Jupyterhub Main Pod Restarts                | Error        | Hub       | This keeps track of the number of hub restarts                                                                                                 | avg by (pod) (kube\_pod\_container\_status\_restarts\_total{pod=\~"jupyterhub-\\\\d.\*-\\\\D\\\\D\\\\D\\\\D\\\\D"})                                                                                                                |
| OOMKilled Jupyterhub Pods                   | Error        | Hub, DB   | Counts the number of pods that have been OOMKilled                                                                                             | avg by (pod) (sum\_over\_time(kube\_pod\_container\_status\_terminated\_reason{pod=\~"jupyterhub-\\\\d.\*-\\\\D\\\\D\\\\D\\\\D\\\\D\|jupyterhub-db-\\\\d.\*", reason="OOMKilled"}[1h\]))                                         |
| Percent of Jupyterhub Requests that are 5xx | Error        | Hub       | This counts the number of 5xx errors in JH operations.                                                                                         | sum(jupyterhub\_request\_duration\_seconds\_count{code=\~"5.\*"})/sum(jupyterhub\_request\_duration\_seconds\_count)                                                                                                               |
| TODO: Error Measurement for JH DB           | Error        | DB        | The ODH team is working on monitoring this                                                                                                     |                                                                                                                                                                                                                                   |
| Jupyterhub Notebook Spawner Init time       | Latency      | Users     | This measures the time it takes Jupyterhub to spin up a new notebook pod spawner                                                               | jupyterhub\_init\_spawners\_duration\_seconds\_bucket                                                                                                                                                                             |
| Jupyterhub Startup Time                     | Latency      | Hub       | This measures the time it takes for the main Jupyterhub pod to come online                                                                     | jupyterhub\_hub\_startup\_duration\_seconds\_bucket                                                                                                                                                                               |
| TODO: Latency for JH DB                     | Latency      | DB        | The ODH team is working on monitoring this                                                                                                     |                                                                                                                                                                                                                                   |
| Jupyterhub Memory Usage                     | Saturation   | Hub,DB    | This measures the memory usage of key Jupyterhub pods over time                                                                                | sum(container\_memory\_rss{pod=\~"jupyterhub-\\\\d.\*\|jupyterhub-db.\*"}) by (pod)                                                                                                                                                 |
| Jupyterhub CPU usage                        | Saturation   | Hub,DB    | This measures the CPU usage of key Jupyterhub pods over time                                                                                   | (sum(rate(container\_cpu\_usage\_seconds\_total{pod=\~"jupyterhub-\\\\d.\*\|jupyterhub-db.\*"}[5m\])) by (pod))\*100                                                                                                               |
| Jupyterhub Database PVC Usage               | Saturation   | DB        | This expression monitors the current consumption % of the DB PVC. If it hits 100% the DB pod will not start up and we are at risk of data loss | (avg by (persistentvolumeclaim) (kubelet\_volume\_stats\_used\_bytes{persistentvolumeclaim=\~"jupyterhub-db"}))/(avg by (persistentvolumeclaim) (kubelet\_volume\_stats\_capacity\_bytes{persistentvolumeclaim=\~"jupyterhub-db"})) |

## SLI Descriptions

1. **Availability:** This tells us whether the Jupyterhub service is available or not.
2. **Error:** This tells us how Jupyterhub is performing, if the error rate is high we know there's some issue in the system
3. **Latency:** This tells us how long it took Jupyterhub to process a request.
4. **Saturation:** This tells us how "full" our service is

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
