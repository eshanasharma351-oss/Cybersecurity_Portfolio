# Sysmon Lab

## Objective

The objective of this lab was to install Sysmon on a Windows 11 system and review the detailed endpoint telemetry generated in Windows Event Viewer.

The lab focused on:
- Sysmon installation.
- Sysmon service verification.
- Process creation events.
- Network connection events.
- File creation events.
- Registry modification events.
- Event filtering and analysis.

## Environment

| Component | Details |
|---|---|
| Operating system | Windows 11 |
| Platform | [VMware] |
| Hostname | [Eshana] |
| RAM | [4 GB] |
| CPU | [2] |
| Network | [NAT ] |
| Sysmon version | [Sysmon v15.22] |
| Date completed | [23-09-2026] |

## What Is Sysmon?

Sysmon is a Windows system service and driver that records detailed system activity in the Windows Event Log.

It can provide information about:
- Process creation.
- Parent and child processes.
- Network connections.
- File creation.
- Registry changes.
- File hashes.
- Process access events.

Sysmon events can be reviewed in:

```text
Event Viewer
→ Applications and Services Logs
→ Microsoft
→ Windows
→ Sysmon
→ Operational
```

Sysmon event types are identified by Event ID. Microsoft documents Event ID 1 for process creation, Event ID 3 for network connections, Event ID 11 for file creation, and Event IDs 12–14 for Registry activity. 

## Installation

Sysmon was installed on the Windows system using the official Microsoft Sysinternals package

## Event Log Location

Sysmon events were reviewed at:

```text
Applications and Services Logs
→ Microsoft
→ Windows
→ Sysmon
→ Operational
```



## Sysmon Events Reviewed

| Event ID | Event | Information Provided |
|---:|---|---|
| 1 | Process Create | New process, command line, parent process, user, and hash |
| 2 | File creation time changed | Changes to file creation timestamps |
| 3 | Network Connection | Network connection linked to a process |
| 11 | File Create | File creation activity |
| 12 | Registry object created or deleted | Registry key or value creation/deletion |
| 13 | Registry value set | Registry value modification |
| 14 | Registry object renamed | Registry key or value rename |

Network connections and some other event types may depend on the Sysmon configuration in use. 

## Process Creation — Event ID 1

Event ID 1 was reviewed to understand how Sysmon records process execution.

Information reviewed:

- Process ID.
- Image path.
- Command line.
- Parent process.
- Parent command line.
- User account.
- File hash.
- Event timestamp.

Harmless commands were run to generate normal process activity:

```powershell
whoami
Get-Date
Get-Process | Select-Object -First 5
```



## Network Connection — Event ID 3

Event ID 3 was reviewed to understand how network activity can be linked to a process.

Information reviewed:

- Source IP address.
- Source port.
- Destination IP address.
- Destination port.
- Protocol.
- Process ID.
- Process image.

PowerShell command used:

```powershell
Get-NetTCPConnection
```



## File Creation — Event ID 11

Event ID 11 was reviewed to understand how Sysmon records file creation.

A harmless test file was created in a temporary lab folder:

```powershell
New-Item -Path "$env:TEMP\sysmon-test.txt" -ItemType File
```

Information reviewed:

- Created file path.
- Creating process.
- User account.
- Timestamp.



## Registry Activity — Event IDs 12–14

Registry events were reviewed to understand how Sysmon records Registry key and value activity.


## Event Filtering

Event Viewer was used to filter Sysmon events by Event ID.

Events filtered:

```text
1, 3, 11, 12, 13, 14
```




## What I Learned

During this lab, I learned how to:

- Install Sysmon on Windows.
- Verify the Sysmon service.
- Locate Sysmon logs in Event Viewer.
- Review process creation events.
- Connect network activity to a process.
- Review file creation events.
- Review Registry modification events.
- Filter Sysmon logs by Event ID.
- Understand how endpoint telemetry supports SOC monitoring.

## Key Takeaway

Sysmon provides more detailed endpoint visibility than basic Windows process and event information alone. Process, network, file, and Registry events help analysts understand what happened on a Windows endpoint and support future threat-hunting and incident-response work.



## References

- [Microsoft Learn — Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Microsoft Learn — Understanding Sysmon events](https://learn.microsoft.com/en-us/windows/security/operating-system-security/sysmon/sysmon-events)
- [Microsoft Learn — Sysmon configuration files](https://learn.microsoft.com/en-us/windows/security/operating-system-security/sysmon/sysmon-configuration-files)