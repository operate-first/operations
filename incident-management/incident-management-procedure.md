# Operate-First Incident Management Standards and Procedures

This document defines the process for how the Operate-First team will respond to incidents and upgrades.

## 1. Index

- [1. Index][9]
- [2. Roles and Responsibilities][10]
- [3. Criteria for Declaring an Incident][11]
- [4. Incident Management Procedure][12]
- [5. Update Procedure][13]
- [6. Communication Templates][14]

## 2. Roles and Responsibilities

- **Operations Technician:** All members of the operations team actively working to resolve the incident or perform an upgrade.

- **Communications Coordinator:** This person is in charge of communicating with customers during an incident or upgrade.

## 3. Criteria for Declaring an Incident

- Is the incident visible to users, and have we spent more than five minutes working to resolve the issue?
- Will we need to involve a second team in resolving the problem?
- Have we been asked by a customer or person in a management position to declare an incident?

## 4. Incident Management Procedure
At the start of an incident, identify the operation technicians (OT) and communications coordinator (CC).

Communications Coordinator:

1. CC must create an Issue [in the SRE repo][1], use [this][3] template. Assign all (OT) to the issue, along with the CC.
2. CC should paste the same message in the slack [Announcement channel][4]
3. CC should follow along and record any significant updates in the github issue thread.
4. Once the incident is resolved, the CC should send out a [resolution message][5] to the issue thread and close it.
5. Send a resolution announcement to the [Announcement channel][4] with a brief message and link to the Github issue thread.

   E.g.: `INCIDENT NOTICE: <CLUSTER> - <SERVICE> incident, for details please follow this issue: <LINK_TO_GH_ISSUE>`.

Operations Technician:

1. The OTs will coordinate with each other to manage changes to live system directly or via git to resolve the incident.
2. The OTs will communicate progress/changes with the CC so they may relay it to the proper communication channels.

## 5. Criteria for announcing an Update

We should announce an upgrade for a cluster or service if the following criteria are met:

For clusters:
- Requires rebooting of any nodes.
- May result in disruption to cluster services (scheduling, network issues, auth, storage, etc.) for an extended period of time (> 5min).

For services:
- May result in service downtime for an extended period of time (> 5min).
- May result in restarting user workloads/rollouts.

## 6. Update Procedure
When an update is agreed upon, the communications coordinator must be identified.

Communications Coordinator:
1. Before the upgrade the CC should:
    - create an Issue [in the SRE repo][1]. Assign all (OT) to the issue, along with the CC.
        - For cluster update: use [this][6] template
        - For any service update: use [this][7] template
    - send an email out to `operate-first-users@lists.massopen.cloud` using the same template chosen previously.
    - send an announcement to the [Announcement channel][4] with a brief message and link to the Github issue thread.

      E.g.: `UPGRADE NOTICE: <CLUSTER> - <SERVICE> upgrade, for details please follow this issue: <LINK_TO_GH_ISSUE>`.

3. During the upgrade CC should follow along and record any significant updates in the github issue thread **only**.
4. After the upgrade CC should send a resolution notice using [this][8] template to all 3 channels from (1).

Operations Technician:

1. The OTs will coordinate with each other to manage changes to live system directly or via git to perform the upgrade.
2. The OTs will communicate progress/changes with the CC so they may relay it to the proper communication channels.

## 7. Communication templates:

### Main Incident start thread template:
```text
Issue Title: "`[INCIDENT][<CLUSTER/<SERVICE>] - <SHORT MSG>`".

Hello Operate First users,

We are investigating an ongoing incident affecting <SERVICE> on <CLUSTER>.

Impacted users: <List which users we believe to be impacted. This may be all
users, it may be users of a subset of services, it may be users of a specific
data set>

We will provide updates to this issue thread as the incident progresses.

Related Links:
- Link 1
- Link 2
```

### Main Incident resolution thread template:
```text
This incident has been resolved.

<Optional: Any important information about lingering effects of the incident

Please let us know if you continue to see any remaining issues.
```

### Cluster Version Upgrade Notice Template
```text
Title/Subject: [Operate-first-users] OpenShift Cluster upgrade, <DATE>

Hello Operate First users,

We’re reaching out to you since you’re the users of the <ENV>/<CLUSTER> at <CLUSTER_URL>.

We are planning on an upgrade to this cluster, from <CURRENT_VERSION> to <NEW_VERSION>, on <DATE>.

<ADDITIONAL_COMMENTS>

We expect to start around <START_TIME> and expect the upgrade to take approximately <EXPECTED_DURATION>.

Relevant links:
- <LINK_1>
- <LINK_2>
```

### Service Upgrade Notice Template
```text
Title/Subject: [Operate-first-users] Service upgrade, <DATE>

Hello Operate First users,

We’re reaching out to you since you’re the users of the <SERVICE> on <ENV>/<CLUSTER> at <CLUSTER_URL>.

We are planning on an upgrade to this service, from <CURRENT_VERSION> to <NEW_VERSION>, on <DATE>.

<ADDITIONAL_COMMENTS>

We expect to start around <START_TIME> and expect the upgrade to take approximately <EXPECTED_DURATION>.

Relevant links:
- <LINK_1>
- <LINK_2>
```

### Generic Upgrade Resolution Notice Template
```text
This upgrade has now been completed.

<Optional: Any important information about lingering effects of the upgrade>.

Please let us know if you continue to see any remaining issues.
```

[1]: https://github.com/operate-first/sre/issues
[2]: https://github.com/operate-first/alerts/issues
[3]: #main-incident-start-thread-template
[4]: https://operatefirst.slack.com/archives/C01RT0NNCP7
[5]: #main-incident-resolution-thread-template
[6]: #cluster-version-upgrade-notice-template
[7]: #service-upgrade-notice-template
[8]: #generic-upgrade-resolution-notice-template
[9]: #1-index
[10]: #2-roles-and-responsibilities
[11]: #3-criteria-for-declaring-an-incident
[12]: #4-incident-management-procedure
[13]: #6-update-procedure
[14]: #7-communication-templates
