# Wazuh Alert Investigation Lab

## Objective

The objective of this lab was to investigate Wazuh security alerts generated from a monitored Windows endpoint and practice basic SOC alert-triage steps.

The lab focused on:

- Reviewing Wazuh security alerts.
- Understanding rule IDs, alert levels, and event details.
- Investigating Windows failed logon alerts.
- Investigating process execution alerts.
- Investigating File Integrity Monitoring alerts.
- Filtering alerts in the Wazuh Dashboard.
- Mapping alerts to MITRE ATT&CK.
- Documenting alert investigation findings.

## Environment
| Component | Details |
|---|---|
| Wazuh platform | [Wazuh All-in-One] |
| Wazuh server OS | [ubuntu 26.04 LTS] |
| Windows endpoint | [Windows 11] |
| Platform | [VMware] |
| Endpoint hostname | [Eshana] |
| Endpoint RAM | [4 GB] |
| Endpoint CPU | [2 virtual CPUs] |
| Network | [NAT] |
| Wazuh version | [4.14.9] |
| Wazuh agent version | [v4.14.9] |
| Date completed | [20/10/2026] |


## Alert Fields Reviewed

Each Wazuh alert was reviewed using the following fields:

| Field | Meaning |
|---|---|
| Timestamp | Time the event occurred or was processed |
| Rule ID | Unique identifier for the detection rule |
| Rule level | Severity or priority of the alert |
| Rule description | Explanation of the detected event |
| Agent name | Endpoint that generated the alert |
| Event data | Details such as user, process, IP address, or file path |
| MITRE ATT&CK | Tactic and technique associated with the event |

Wazuh rules include identifiers and severity levels, and rules can be mapped to MITRE ATT&CK tactics, techniques, and subtechniques. 

## Accessing Alerts

Alerts were accessed through the Wazuh Dashboard.

### Steps

1. Sign in to the Wazuh Dashboard.
2. Open **Threat Hunting**.
3. Select the **Events** tab.
4. Review the available alerts.
5. Select an alert to view its full details.
6. Review the rule ID, severity level, agent, timestamp, and event data.



## Filtering Alerts

Filters were used to narrow the investigation to relevant alerts.

### Filter by agent

```text
agent.name: "Windows-11"
```

### Filter by alert severity

```text
rule.level: >= 7
```

### Filter by a specific rule

```text
rule.id: 60122
```

### Filter by multiple rules

```text
rule.id: (60122 OR 60204)
```

Rule `60122` detects an individual Windows logon failure, while rule `60204` detects multiple Windows logon failures and can indicate a possible brute-force pattern. 


## Investigation 1 — Failed Logon Alert

### Alert details

| Field | Details |
|---|---|
| Alert title | [Logon Failure - Unknown user or bad password] |
| Rule ID | [60122] |
| Severity | [5] |
| Agent | [Windows-11] |
| Timestamp | [Oct 21, 2026 @ 19:43:36.333] |
| Affected user | [Eshana] |
| Source IP address | [127.0.0.1] |
| MITRE ATT&CK | [Account Access Removal] |

### Investigation steps

1. Open the failed-logon alert.
2. Review the timestamp and affected user account.
3. Check whether multiple failed attempts occurred from the same source IP.
4. Check whether the failed attempts were followed by a successful logon.
5. Determine whether the source IP is internal, external, known, or unexpected.
6. Review related alerts for the same user or IP address.
7. Document the final assessment.

### Example query

```text
rule.id: 60122 AND agent.name: "Windows-11"
```

### Findings

```text
Number of failed logon events: [1]
Source IP address: [127.0.0.1]
Affected account: [Eshana]
Successful logon observed: [Yes]
Assessment: [Expected]
```



## Investigation 2 — Process Execution Alert

### Alert details

| Field | Details |
|---|---|
| Alert title | [A process was created] |
| Rule ID | [67027] |
| Severity | [3] |
| Agent | [Windows-11] |
| Timestamp | [Oct 21, 2026 @ 20:10:22.737] |
| Process name | [C:\\Windows\\System32\\whoami.exe] |
| Command line | [\"C:\\WINDOWS\\system32\\whoami.exe\"] |
| Parent process | [C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe] |
| User account | [Eshana] |


### Investigation steps

1. Review the process name and full command line.
2. Identify the parent process.
3. Determine whether the user account is expected to run the process.
4. Check whether the command line is normal for the endpoint.
5. Review whether the process created files or made network connections.
6. Check for related alerts involving the same process or user.
7. Document the final assessment.

### Findings

```text
Process observed: [C:\\Windows\\System32\\whoami.exe]
Parent process: [C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe]
User account: [Eshana]
Command line: [\"C:\\WINDOWS\\system32\\whoami.exe\"]
Assessment: [Expected]
```


## Investigation 3 — File Integrity Monitoring Alert

### Alert details

| Field | Details |
|---|---|
| Alert title | [Integrity checksum changed.] |
| Rule ID | [550] |
| Severity | [7] |
| Agent | [Windows-11] |
| Timestamp | [Oct 20, 2026 @ 19:43:48.761] |
| File path | [c:\users\public\documents\fim.txt] |
| Event type | [ Modified] |
| User account | [Eshana] |


### Investigation steps

1. Open the FIM alert.
2. Review the file path and event type.
3. Determine whether the file location is expected.
4. Identify the user or process responsible for the change.
5. Review the file hash, if available.
6. Check whether the change was authorized.
7. Document the final assessment.

### Findings

```text
File changed: [c:\users\public\documents\fim.txt]
Change type: [ Modified]
Responsible user: [Eshana]
Authorized change: [Yes ]
Assessment: [Expected]
```



## MITRE ATT&CK Review

The MITRE ATT&CK module was used to review the tactics and techniques associated with Wazuh alerts.

### Steps

1. Open **Threat Detection and Response**.
2. Select **MITRE ATT&CK**.
3. Select **Framework**.
4. Review the available tactics and techniques.
5. Select a technique.
6. Review the alerts and events associated with that technique.

The Wazuh MITRE ATT&CK module displays available tactics and techniques and shows events in the environment associated with a selected technique. 


## Alert Triage Checklist

For each alert, complete the following checks:

- [ ] Confirm the affected agent.
- [ ] Review the alert timestamp.
- [ ] Identify the rule ID and severity.
- [ ] Read the rule description.
- [ ] Review the event data.
- [ ] Identify the user, process, IP address, or file involved.
- [ ] Check for related alerts.
- [ ] Review the MITRE ATT&CK mapping.
- [ ] Determine whether the activity is expected.
- [ ] Document the conclusion.
- [ ] Escalate if the activity is suspicious or unexplained.

## Investigation Summary

## Investigation Summary

| Alert | Rule ID | Severity | Assessment | Action Taken |
|---|---:|---:|---|---|
| Logon Failure - Unknown user or bad password | 60122 | 5 | Expected | Closed |
| A process was created (`whoami.exe`) | 67027 | 3 | Expected | Closed |
| Integrity checksum changed (`fim.txt`) | 550 | 7 | Expected | Closed |

## What I Learned

During this lab, I learned how to:

- Locate and review Wazuh alerts.
- Interpret rule IDs, severity levels, and alert descriptions.
- Filter alerts by agent, rule ID, and severity.
- Investigate Windows authentication activity.
- Investigate process execution activity.
- Review File Integrity Monitoring alerts.
- Use MITRE ATT&CK to understand possible attacker behavior.
- Document alert triage and investigation findings.

## Key Takeaway

Wazuh alert investigation requires reviewing more than the alert title. A SOC analyst should examine the affected endpoint, event details, related alerts, user activity, process behavior, and MITRE ATT&CK mapping before deciding whether an alert is expected, suspicious, or requires escalation.



## References

- [Wazuh Documentation — Threat Intelligence and MITRE ATT&CK](https://documentation.wazuh.com/current/compliance/nist/threat-intelligence.html)
- [Wazuh Documentation — Log data analysis](https://documentation.wazuh.com/current/compliance/nist/log-data-analysis.html)
- [Wazuh Documentation — Rules](https://documentation.wazuh.com/5.0-rc/user-manual/data-analysis/rules.html)
- [Wazuh Documentation — Global configuration](https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/global.html)