# Incident Response Basics

## Overview
Incident response is the practice of investigating and remediating active attack campaigns on an organization. In a SOC, it is used to detect, contain, and recover from security incidents in a structured way .

## What Incident Response Is
Incident response is not just reacting after an attack. It is a structured process used to handle active threats, limit impact, and learn from the event so future incidents can be handled better .

## Why It Matters
A strong incident response process helps reduce downtime, protect sensitive data, and minimize business disruption. It also gives analysts a repeatable method for handling security events consistently.

## Incident Response Phases
The common phases of incident response are:
- Preparation.
- Detection and analysis.
- Containment.
- Eradication.
- Recovery.
- Lessons learned .

## Preparation
Preparation means having tools, procedures, contacts, and logging in place before an incident happens. This includes playbooks, escalation paths, and access to important logs.

## Detection and Analysis
This phase is where the team confirms whether an alert or event is a real incident. Analysts gather evidence, review logs, identify affected systems, and determine the scope of the attack .

## Technical Triage
When investigating a suspected Windows incident, analysts often start with:
- 4624 and 4625 for successful and failed logons.
- 4688 and Sysmon Event ID 1 for process creation.
- 4697 and 7045 for service installation.
- 4698 for scheduled task creation.
- 4657 and Sysmon registry events for persistence.
- 1102 for cleared logs.
- 4104 for PowerShell script block activity.

## Log Sources
The most useful logs for IR on Windows are:
- Security logs.
- Sysmon logs.
- PowerShell Operational logs.
- Windows Defender logs.
- Network and firewall logs .

## Correlation and Scoping
The next step is correlating events across logs to understand how the attacker moved. For example, a failed login followed by a successful login, then privileged activity, then suspicious process execution can show the full attack chain .

## Containment
Containment is about stopping the incident from spreading. Common actions include isolating the host, disabling compromised accounts, blocking malicious IPs, and preventing further execution.

## Eradication
Eradication means removing the malicious artifacts and fixing the root cause. That may include deleting malware, removing persistence mechanisms, and patching the vulnerability that allowed access .

## Recovery
Recovery is restoring systems and validating that the threat is gone. The machine should be monitored after cleanup to make sure the attacker does not return .

## Reporting
A good IR report should include the timeline, evidence, impact, actions taken, and recommendations. In GitHub notes, this is where you show that you understand both the technical side and the documentation side of incident response .

## Key Takeaway

Incident response is not just theory; it is a practical workflow built on logs, triage, and fast action. 

## References
- **Incident Response:** Microsoft Learn. *Incident response overview* — https://learn.microsoft.com/en-us/security/operations/incident-response-overview 
- **IR Planning:** Microsoft Learn. *Incident response planning* — https://learn.microsoft.com/en-us/security/operations/incident-response-planning 
- **Windows Logging:** Microsoft Learn. *Understanding Sysmon events* — https://learn.microsoft.com/en-us/windows/security/operating-system-security/sysmon/sysmon-events 
- **Windows Event IDs:** Microsoft Learn. *Windows security event sets that can be sent to Microsoft Sentinel* — https://learn.microsoft.com/en-us/azure/sentinel/windows-security-event-id-reference 
- **IR Playbooks:** Microsoft Learn. *Incident response playbooks* — https://learn.microsoft.com/en-us/security/operations/incident-response-playbooks.