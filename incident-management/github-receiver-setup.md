# GitHub Alertmanager Receiver

The [GitHub alertmanager receiver](https://github.com/m-lab/alertmanager-github-receiver) is a Prometheus alertmanager webhook receiver that creates GitHub issues from alerts. An example issue would look like https://github.com/operate-first/SRE/issues/34. The GitHub receiver can easily be configured and operated to function with Prometheus alerts. The typical flow of alerting would look like:

`Prometheus (generates rules) -> Prometheus alertmanager (routes alerts) -> GitHub receiver (receives alerts)`

## Setup Instructions

### Prerequisites

* An OCP cluster
* GitHub account

### Create GitHub access token

The github receiver uses user access tokens to create issues in an existing
repository.

Generate a new access token:

* Log into GitHub and visit https://github.com/settings/tokens
* Click the 'Generate new token' button
* Select the 'repo' scope and all subscopes of 'repo'

Because this access token has permission to create issues and operate on
repositories the access token user can access, protect the access token as
you would a password.

### Deploy GitHub Receiver

1. To deploy the GitHub receiver, you can use the deployment configuration defined [here](https://github.com/operate-first/apps/blob/master/odh/base/monitoring/github-receiver.yaml). It is important to note that we need to specify the GitHub auth token in the `spec.containers.env` field which has permissions to create issues in your target repo(s):

```yaml
spec:
      containers:
        - name: github-receiver
          image: quay.io/operate-first/alertmanager-github-receiver:v0.10
          env:
            - name: GITHUB_AUTH_TOKEN
              valueFrom:
                secretKeyRef:
                  name: github-secrets
                  key: auth-token
```

You can configure various arguments in the `spec.containers.args` field as follows:

```yaml
args:
            - --authtoken=$(GITHUB_AUTH_TOKEN)
            - --org=operate-first
            - --repo=SRE
            - --label=in progress
            - --enable-inmemory=false
            - --enable-auto-close=true
```

* `--authtoken` - your GitHub auth token
* `--org` - the organization or user name in GitHub
* `--repo` - the GitHub repo name you wish to have the issues created in
* `--label` - labels to add to issues at creation time
* `--enable-inmemory` - performs all operations in memory, without using github API
* `--enable-auto-close` - once an alert stops firing, automatically close open issues

are some of the additional parameters you can apply to your GitHub receiver.

2. Ensure to add your GitHub auth token as a secret to your project namespace with the same name/key value as specified in the `secretKeyRef` field.

3. Include a service definition for the GitHub receiver as defined [here](https://github.com/operate-first/apps/blob/master/odh/base/monitoring/github-receiver-service.yaml). The service produces a name usable in-cluster like:

`GITHUB_RECEIVER_URL=http://github-receiver-service.default.svc.cluster.local:9393/v1/receiver`

This is the webhook URL that we will need to provide in our Prometheus Alertmanager config, so that the alerts can be routed to it.

Once this is completed successfully, you should have your GitHub receiver running and ready to be configured to a Prometheus alertmanager!
