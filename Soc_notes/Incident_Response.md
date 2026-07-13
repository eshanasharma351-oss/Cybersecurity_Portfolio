# Incident Response Basics

## Overview
Incident response is the process of identifying, investigating, containing, eradicating, and recovering from a security incident. For SOC work, the goal is to reduce impact, preserve evidence, and restore normal operations as fast as possible [web:349][web:351].

## Why It Matters
A good incident response process helps teams limit damage, protect data, and respond in a repeatable way. It also gives analysts a structured method for handling alerts and real attacks instead of reacting randomly [web:350][web:353].

## IR Workflow
The standard workflow usually includes:
- Preparation.
- Detection and analysis.
- Containment.
- Eradication.
- Recovery.
- Lessons learned [web:349][web:351][web:353].

## Preparation
Preparation means having logging, playbooks, escalation paths, and tools ready before an incident happens. For a SOC analyst, this includes knowing where to find logs, who to notify, and what evidence to preserve [web:351][web:364].

## Detection and Analysis
This is the point where you decide whether the alert is real or false positive. You check the source of the alert, review timestamps, identify affected hosts and accounts, and build an initial timeline of activity [web:349][web:366].

## Log Sources
The most useful logs for IR on Windows are:
- Security logs.
- Sysmon logs.
- PowerShell Operational logs.
- Windows Defender logs.
- Network and firewall logs [web:366][web:367][web:370].

## Technical Triage
When investigating a suspected Windows incident, analysts often start with:
- 4624 and 4625 for successful and failed logons.
- 4688 and Sysmon Event ID 1 for process creation.
- 4697 and 7045 for service installation.
- 4698 for scheduled task creation.
- 4657 and Sysmon registry events for persistence.
- 1102 for cleared logs.
- 4104 for PowerShell script block activity [web:366][web:370][web:326].

## Correlation and Scoping
The next step is correlating events across logs to understand how the attacker moved. For example, a failed login followed by a successful login, then privileged activity, then suspicious process execution can show the full attack chain [web:366][web:364].

## Containment
Containment is about stopping the incident from spreading. Common actions include isolating the host, disabling compromised accounts, blocking malicious IPs, and preventing further execution [web:349][web:355].

## Eradication
Eradication means removing the malicious artifacts and fixing the root cause. That may include deleting malware, removing persistence mechanisms, and patching the vulnerability that allowed access [web:349][web:366].

## Recovery
Recovery is restoring systems and validating that the threat is gone. The machine should be monitored after cleanup to make sure the attacker does not return [web:349][web:351].

## Reporting
A good IR report should include the timeline, evidence, impact, actions taken, and recommendations. In GitHub notes, this is where you show that you understand both the technical side and the documentation side of incident response [web:355][web:346].

## Key Takeaway
Incident response is not just theory; it is a practical workflow built on logs, triage, and fast action. The more clearly you can explain the technical steps, the better it works as a GitHub project [web:366][web:367].

## References
- **Incident Response:** Microsoft Learn. *Incident response overview* — https://learn.microsoft.com/en-us/security/operations/incident-response-overview [web:349]
- **IR Planning:** Microsoft Learn. *Incident response planning* — https://learn.microsoft.com/en-us/security/operations/incident-response-planning [web:351]
- **Windows Logging:** Microsoft Learn. *Understanding Sysmon events* — https://learn.microsoft.com/en-us/windows/security/operating-system-security/sysmon/sysmon-events [web:326]
- **Windows Event IDs:** Microsoft Learn. *Windows security event sets that can be sent to Microsoft Sentinel* — https://learn.microsoft.com/en-us/azure/sentinel/windows-security-event-id-reference [web:324]