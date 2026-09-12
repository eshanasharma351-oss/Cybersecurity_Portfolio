# Windows Basic Lab

## Objective

The objective of this lab was to learn and document basic Windows 11 administration and security tools.

The lab covered system information, users and groups, processes, services, network connections, Task Scheduler, startup items, Windows Defender, Windows Firewall, and Event Viewer.

## Environment

- **Operating System**[windows 11]
- **RAM:** [4 GB]
- **CPU:** [2 virtual CPUs ]
- **Network:** [NAT]
- **Date completed:** [12/09/2026]

## Topics Completed

| Topic | Command or Location | 
|---|---|---|
| System information | `systeminfo` | 
| Users and groups | `whoami`, `whoami /groups`, `net user` | 
| Processes | `tasklist` or Task Manager |
| Services | `Get-Service` or `services.msc` | 
| Network connections | `ipconfig`, `netstat -ano` | 
| Task Scheduler | `taskschd.msc` | 
| Startup items | Task Manager → Startup apps | 
| Windows Defender | Windows Security → Virus & threat protection | 
| Windows Firewall | Windows Security → Firewall & network protection | 
| Event Viewer | `eventvwr.msc` | 

## 1. System Information

Command used:

```cmd
systeminfo
```

This command was used to view the Windows version, hostname, system type, memory, network configuration, and other operating-system details.



## 2. Users and Groups

Commands used:

```cmd
whoami
whoami /groups
net user

```

These commands were used to identify the current user, view group membership, list local accounts, and review members of the local Administrators group.



## 3. Processes

Command used:

```cmd
tasklist
```

The Windows Task Manager was also used to view running processes, resource usage, and process details.



## 4. Services

PowerShell command:

```powershell
Get-Service
```



This helped me understand how Windows background services are listed and managed.


## 5. Network Connections

Commands used:

```cmd
ipconfig
netstat -ano
```

These commands were used to view network configuration, active connections, listening ports, and process IDs.



## 6. Task Scheduler

Tool used:

```text
taskschd.msc
```

Task Scheduler was reviewed to understand how Windows runs programs automatically at startup, logon, or a specified time.



## 7. Startup Items

Location reviewed:

```text
Task Manager → Startup apps
```

Startup applications were reviewed to see which programs are configured to run automatically when a user signs in.



## 8. Windows Defender

Location reviewed:

```text
Windows Security → Virus & threat protection
```

Windows Defender was reviewed to check antivirus status, scan options, and protection history.



## 9. Windows Firewall

Location reviewed:

```text
Windows Security → Firewall & network protection
```

The firewall status for the available network profiles was reviewed.

Optional command:

```powershell
Get-NetFirewallProfile
```


## 10. Event Viewer

Tool used:

```text
eventvwr.msc
```

The following Windows logs were reviewed:

- Application.
- Security.
- System.

Event Viewer was used to understand how Windows records application, security, operating-system, login, service, and error events.



## What I Learned

During this lab, I learned how to:

- Collect Windows system information.
- Review users, groups, and administrator membership.
- View running processes and services.
- Check network configuration and active connections.
- Explore Task Scheduler and startup applications.
- Review Windows Defender and Firewall status.
- Navigate important Windows Event Viewer logs.
- Use Windows tools that form the foundation of SOC monitoring.

## Key Takeaway

Windows provides several built-in tools for understanding endpoint activity. Learning these tools is an important foundation for future work in Windows administration, SOC monitoring, threat detection, and incident response.



## References

- [Microsoft Learn — systeminfo](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/systeminfo)
- [Microsoft Learn — tasklist](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist)
- [Microsoft Learn — wevtutil](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil)
- [Microsoft Learn — Event Viewer](https://learn.microsoft.com/en-us/shows/inside/event-viewer)