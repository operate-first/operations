Let's divide the monitoring stack and the long term metric storage stack and solve them individually. <br>
This is because we are certain that we will use Prometheus and Grafana for Monitoring and Thanos for long term storage.

## Monitoring
### 1. Install the ODH Monitoring Stack
**Pros**
* Should already have monitoring enabled for other ODH components
* No need of extra privileges to install the Operator once ODH is installed

**Cons**
* Operator version might be older than the current upstream version

### 2. Install Prometheus and Grafana using their Community Operators
**Pros**
* Configuration should be simple (Using kubernetes/ocp resource labels/annotations + Service Monitors)

**Cons**
* Need extra privileges to set up Operators on the cluster

### 3. Manual Deployments for Prometheus and Grafana
**Pros**
* Don't need privileges that are required to install Operators

**Cons**
* Configuration might be a lot of work
* All future updates will be manual and might break stuff

## Long Term Metric Storage
### 1. Install the Observatorium stack for Thanos
**Pros**
* Scaleable
* Might be expanded to support logs using Loki in the near future

**Cons**
* Documentation is not very up to date

### 2. Set up own custom Thanos deployment
**Pros**
* Some team members might have the experience maintaining such a set up

**Cons**
* Not scaleable
* Need to maintain multiple different components manually (Thanos-Store, Thanos-Compactor...)


---
#### Notes
* For the monitoring stack, once we decide the solution, we will also need to figure out how to distribute Prometheus instances. <br>
i.e. Should each namespace have it's own Prometheus instance or not.
