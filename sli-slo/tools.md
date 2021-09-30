This page highlights the operational tools and services monitored through Operate First.

## Operational Tools

* [Prometheus](https://prometheus.io/) - Monitoring tool capturing time series metrics for available Operate First services
* [Prometheus Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) - Alertmanager for handling alerts and routing them to the appropriate receiver integration such as Email, Slack, PagerDuty etc
* [Grafana](https://grafana.com/) - Visualization tool for creating monitoring dashboards
* [GitHub Receiver](https://github.com/m-lab/alertmanager-github-receiver) - Incident reporting tool for handling outages/incidents

## Services Monitored

We have [Open Data Hub](https://opendatahub.io/) applications deployed and running in a MOC [(Mass Open Cloud)](https://massopen.cloud/) cluster. Open Data Hub is an end-to-end AI/ML platform on top of OpenShift Container Platform which provides various tools for Data Scientists and Engineers.

The components we currently have available are:
* [JupyterHub](https://jupyterhub-opf-jupyterhub.apps.smaug.na.operate-first.cloud/login): which hosts interactive Jupyter notebooks. You can see the runbook for how to administer SRE practices on JupyterHub [here](https://www.operate-first.cloud/users/sre/runbooks/jupyterhub.md).
