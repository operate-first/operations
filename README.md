# Site Reliability Engineering (SRE) Support

This repository contains all the SRE (Site Reliability Engineering) principles and guidelines for managing the [Operate-First](https://operate-first.github.io/operate-first/) services.

## What is SRE?

SRE is a software engineering approach to manage operations for systems, applications and services. We use software as a tool to manage systems, solve problems, and automate operations tasks.

## Services Monitored

We have [Open Data Hub](https://opendatahub.io/) applications deployed and running in a MOC [(Mass Open Cloud)](https://massopen.cloud/) cluster. Open Data Hub is an end-to-end AI/ML platform on top of OpenShift Container Platform which provides various tools for Data Scientists and Engineers.

The components we currently have available are:
* [JupyterHub](https://jupyterhub-opf-jupyterhub.apps.cnv.massopen.cloud/hub/login)

## SLI and SLOs

For each of the Operate-First managed services, we define Service Level Indicators (SLI) and Service Level Objectives (SLO) to help us monitor the availability of the service and improve its reliability.

The `sli-slo` folder [here](https://github.com/operate-first/SRE/tree/master/sli-slo) lists out the SLI/SLO for each of the applications in separate markdown files. For eg, you can find the SLI/SLO defined for the `JupyterHub` application [here](https://github.com/operate-first/SRE/blob/master/sli-slo/jupyterhub.md).

## Operational Tools

TO DO

## Runbooks

For all the services being monitored, we define runbooks to provide information on how to administer, debug and effectively troubleshoot common problems of a service.

The runbook for each of the individual services are defined in the `runbooks`
[folder](https://github.com/operate-first/SRE/tree/master/runbooks).

## Incident Management

TO DO
