# Wazuh Lab

## Objective

The objective of this lab was to deploy and use Wazuh to monitor a Windows 11 endpoint, review security alerts, and understand how endpoint telemetry is collected and analyzed.

The lab focused on:

- Wazuh dashboard access.
- Wazuh agent deployment and enrollment.
- Windows endpoint monitoring.
- Agent status verification.
- Security alert review.
- File Integrity Monitoring.
- Vulnerability Detection.
- Security Configuration Assessment.
- Basic alert investigation.

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

## Wazuh Architecture

Wazuh uses agents to collect endpoint data. The agent sends information to the Wazuh server, where the Wazuh manager analyzes events and generates alerts. The Wazuh indexer stores the data, and the Wazuh dashboard provides a web interface for monitoring and investigation.

```text
Windows 11 Endpoint
        │
        │ Wazuh Agent
        ▼
Wazuh Manager
        │
        ▼
Wazuh Indexer
        │
        ▼
Wazuh Dashboard
```

Wazuh can collect and analyze endpoint security data, including Windows event logs, file changes, software inventory, vulnerabilities, configuration assessments, and detection alerts. 

## Wazuh Dashboard Access

The Wazuh Dashboard was accessed through a web browser.

```text
https://192.168.121.132
```

After signing in, the dashboard displayed the Wazuh overview page, available agents, security events, and endpoint-monitoring modules.



## Windows Agent Enrollment

A Wazuh agent was deployed on the Windows 11 endpoint and connected to the Wazuh manager.

### Agent installation steps

1. Log in to the Wazuh Dashboard.
2. Open **Agents Management**.
3. Select **Deploy new agent**.
4. Select **Windows** as the operating system.
5. Enter the Wazuh manager address.
6. Copy the generated installation command.
7. Open PowerShell as Administrator on the Windows endpoint.
8. Run the generated installation command.
9. Start the Wazuh agent service.
10. Confirm that the agent appears in the dashboard.

Example service verification command:

```powershell
Get-Service WazuhSvc
```

Expected result:

```text
Status : Running
```



## Agent Status

The Wazuh Dashboard was used to verify that the Windows endpoint was successfully enrolled and active.

Information reviewed:

- Agent name.
- Agent ID.
- Agent IP address.
- Operating system.
- Agent version.
- Connection status.
- Last keep-alive time.



## Windows Event Monitoring

Wazuh collected Windows endpoint activity and displayed relevant events in the dashboard.

Examples of Windows activity that may be collected include:

- Successful and failed logon events.
- Account and group changes.
- Process execution events.
- Windows Defender events.
- PowerShell activity.
- System and application events.

The exact event visibility depends on the Windows Event Channel configuration and the Wazuh agent configuration.



## Alert Review

Security alerts were reviewed from the Wazuh Dashboard.

Each alert can include:

- Timestamp.
- Rule ID.
- Rule severity level.
- Rule description.
- Agent name.
- Event source.
- User account.
- Process information.
- Related event fields.
- MITRE ATT&CK mapping, when available.

| Field | Details |
|---|---|
| Alert title | [Windows audit failure event] |
| Rule ID | [60104] |
| Severity | [5] |
| Agent | [windows-11] |
| Timestamp | [Oct 20, 2026 @ 17:30:00.708] |




## File Integrity Monitoring

File Integrity Monitoring (FIM) was reviewed to detect file and directory changes on the monitored Windows endpoint.

Wazuh FIM can identify:

- File creation.
- File modification.
- File deletion.
- Permission changes.
- Ownership changes.
- File hash changes.

A harmless test file can be created on the endpoint:

```powershell
New-Item -Path "$env:TEMP\fim.txt" -ItemType File
```

Then modify the file:

```powershell
Add-Content -Path "$env:TEMP\fim.txt" -Value "Wazuh FIM test"
```

Information to review in the alert:

- File path.
- Event type.
- User account.
- File hash.
- Timestamp.
- Agent name.



## Vulnerability Detection

The Vulnerability Detection module was reviewed to identify known vulnerabilities affecting software installed on the Windows endpoint.

The module uses endpoint software inventory and vulnerability intelligence to identify software that may be affected by known CVEs. 

Information reviewed:

- CVE ID.
- Severity.
- Affected software.
- Installed package version.
- Vulnerability status.
- Published date.
- Detection date.




## Security Configuration Assessment

Security Configuration Assessment checks the endpoint configuration against security policies and benchmarks.

The assessment may identify:

- Weak password policies.
- Unnecessary services.
- Insecure Windows settings.
- Missing security settings.
- Configuration values that do not meet a selected policy.

Information reviewed:

- Policy name.
- Check ID.
- Result.
- Compliance status.
- Remediation guidance.



## What I Learned

During this lab, I learned how to:

- Access and navigate the Wazuh Dashboard.
- Deploy or verify a Wazuh agent on a Windows endpoint.
- Confirm that an endpoint agent is active.
- Review Windows endpoint events.
- Examine Wazuh alert details and severity levels.
- Use File Integrity Monitoring to identify file changes.
- Review software vulnerabilities on an endpoint.
- Review endpoint security configuration results.
- Connect endpoint activity to SOC monitoring and alert investigation.

## Key Takeaway

Wazuh provides centralized visibility into endpoint security activity. By collecting Windows logs, endpoint inventory data, file changes, and security configuration information, it helps a security analyst identify and investigate potential security events from one dashboard.



## References

- [Wazuh Documentation](https://documentation.wazuh.com/current/)
- [Wazuh Agent Deployment](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/)
- [Wazuh File Integrity Monitoring](https://documentation.wazuh.com/current/user-manual/capabilities/file-integrity/index.html)
- [Wazuh Vulnerability Detection](https://documentation.wazuh.com/current/user-manual/capabilities/vulnerability-detection/how-it-works.html)
- [Wazuh Security Configuration Assessment](https://documentation.wazuh.com/current/user-manual/capabilities/sec-config-assessment/)