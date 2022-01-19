# Configuring Prometheus Alerts

Follow the instructions described below for configuring alerts to our Prometheus instance.

## Adding Alerts

If you wish to add new alerts for your team/applications to our Prometheus instance, you will need to:

1. Define your Prometheus alerting rules in a `example-prometheus-rules.yaml` file in our [`operate-first/apps`](https://github.com/operate-first/apps/) repo:

```yaml
---
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  labels:
    prometheus: odh-monitoring
    role: alert-rules
  name: example-prometheus-alerting-rules
spec:
  groups:
    - name: Example Rules
      rules:
        - alert: ExampleAlert
          expr: vector(1)
          for: 2m
          annotations:
            summary: "This is an example alert"
```
Save this file under `odh/overlays/moc/monitoring/alerts/example-prometheus-rules.yaml`.

3. Add your prometheus alerting rule file to `odh/overlays/moc/monitoring/alerts/kustomization.yaml` by running the following:

````bash
$ cd odh/overlays/moc/monitoring/alerts/
$ kustomize edit add resource example-prometheus-rules.yaml
````

4. Once you have the alerting rules defined, you will need to add your preferred alert receiver to which the alerts will be routed. The receiver can be Slack, Email, PagerDuty etc. We currently route our alerts to the [GitHub receiver](https://github.com/m-lab/alertmanager-github-receiver) which automatically creates issues in this repo for any alerts being fired. If you would like to try out the GitHub receiver, we would recommend you to have your own deployment so that you can configure it to create issues in your preferred repo(s) and customize it according to your needs. The deployment instructions can be found [here](https://github.com/operate-first/SRE/blob/master/incident-management/github-receiver-setup.md).

5. Once you have your alert receiver setup, you will need to add this receiver to our Prometheus alertmanager config [here](https://github.com/operate-first/apps/blob/master/odh/base/monitoring/alertmanager-config.yaml):

```yaml
alertmanager.yaml: |
    routes:
    - match_re:
        # This matches all metrics having the `job` label beginning with the string `example`
        job: ^(example).*$
        receiver: your-receiver

    receivers:
    - name: 'your-receiver'
      webhook_configs:
        - url: 'your-receiver-address'
```

In order to differentiate and route the alerts to their respective receivers, you will need to provide a suitable regex expression matching your metrics so that the alerts for those metrics only get routed to your receiver. This is specified in the `match_re` field.

Depending on your receiver type, you will add it under the `receivers` field with a suitable name. For more information on the various receivers supported by Prometheus and how they are defined, you can refer to the documentation [here](https://prometheus.io/docs/alerting/latest/configuration/#receiver).

6. Submit a PR with all the above changes to our `operate-first/apps` repo.
