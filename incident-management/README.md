## Incident Management

This folder contains information pertaining to how the Operate First team will respond to incidents/outages.

As we have multiple services/applications deployed and monitored in the Operate First environment (ex. Jupyterhub, Argo, Superset, Observatorium, Project Thoth, AICoE CI pipelines etc), we need to implement an incident reporting setup for handling outages/incidents related to these services. Whether its a major bug, capacity issues, or an outage, our users expect an immediate response. Having an efficient incident management process is critical in ensuring that the incidents are always communicated to the users (via on-call notification systems) and handled by the team immediately. An on-call notification system is a system/software that provides an automated means of contacting users and communicating pertinent information during an incident. It also has additional on-call scheduling features that can be used to ensure that the right people on the team are available to address a problem during an incident.

### GitHub Alertmanager Receiver

We have chosen the [GitHub Alertmanager](https://github.com/m-lab/alertmanager-github-receiver) as our tool for reporting the outages/incidents to our users. The GitHub alertmanager is a Prometheus alertmanager webhook receiver that creates GitHub issues from alerts. It can easily be configured and operated to function with Prometheus alerts. All communication/updates/concerns related to the incident can be easily handled by adding comments in the issues created by the GitHub receiver, making the incident management process completely transparent to our users. We can also configure GitHub bots for different chatting platforms such as Slack, Google Chat to notify us of the issues immediately.

To deploy and setup your own GitHub alertmanager receiver, follow the instructions [here](https://github.com/operate-first/SRE/blob/master/incident-management/github-receiver-setup.md).

### Alerting with Prometheus

All the services are being monitored by Prometheus. Prometheus scrapes and stores time series data identified by metric key/value pairs for each of the available services. We use these metrics to define the [SLI/SLOs](https://github.com/operate-first/SRE/tree/master/sli-slo) for each of our services and alert on any possible service degradation.

Alerting with Prometheus is separated into two parts:

1. [Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)  in Prometheus servers send alerts to an [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/). Alerting rules allow you to define alert conditions based on Prometheus expression language expressions.
2. The Alertmanager then manages those alerts, including silencing, inhibition, aggregation and sending out notifications via methods such as email, on-call notification systems, and chat platforms.

We have added our GitHub alertmanager receiver as the default receiver for our Prometheus alertmanager to route the alerts. For any alert that is being fired, the GitHub receiver will automatically create an issue in this repository like so: https://github.com/operate-first/SRE/issues/34.

You can refer to the documentation [here](https://github.com/operate-first/SRE/blob/master/incident-management/configure-prometheus-alerts.md) which describes how these alerting rules are setup for our Operate First monitoring stack.
