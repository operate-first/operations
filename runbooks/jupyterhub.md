# Data Hub Jupyterhub Runbook

This document provides information on how to administer our Jupyterhub
deployment.

## Key Locations
| Env                                    | Namespace                                                    | GitHub Repo                        |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------|-------------------------------|
| Prod                        | namespace deployed                                                                            | link to repo                        |

## Dashboards
Links to the monitoring dashboards for this service.
| Grafana Dashboard                                    | Dashboard Description                                                    |
|----------------------------------------|----------------------------------|
| dashboard URL                        | description/purpose of dashboard    |

## Contact Info
Team contact info.  Potentially contact info for who to escalate to.  What services do we have dependencies on?  How do we escalate to them?  Define this information here.

| Primary SME                                    | Secondary SME                                                                                                     | Contact                        | Chat Channel |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------|-------------------------------|-----------------------------|
| Name                        | Name                                                                            | <name@email.com>                         |   gchat/slack channel   |

## Prerequisites

Our Jupyterhub instance is managed by the Open Data Hub operator.

## Deployment

The Jupyterhub server is deployed using Kustomize. As such, any changes should be made
in the necessary files in git and redeployed (rather than manually editing
OpenShift objects directly).

Please read the <insert link to documentation> for deployment instructions.

## Upkeep and Administration
Include relevant information pertaining to administrating the service.

## Common Problems
The following is a list of common issues we've encountered with Jupyterhub and
how to fix them.

## Smoke Test
Verify that the service is `up` and `available` by checking if you can access/login to the service endpoint.

## Monitoring

### Health Checks
How is the health of the service and its dependencies?

### Alerts
Links to the Alerts for this service.

For Every Alert there should be a corresponding section in alphabetical order
#### Alert Title
Alert Description:  Why do we have this alert?  What does it mean?  What is typically the cause of this alert?

##### Impact to Customers:
How does this situation impact our customers?  If the customers are not being impacted, this is a good indicator that the alert can be deleted.

##### Remediation Steps:
Checklist manifesto style steps for how to resolve this alert.  A person who has never worked on our stack should be able to follow these steps and remediate the incident.  If it cannot be remediated, include escalation steps here.
 1. Do this
 2. Check this graph
 3. Do this thing
 4. Do this other thing
 5. Verify service has recovered
