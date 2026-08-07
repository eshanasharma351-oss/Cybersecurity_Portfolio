# MITRE ATT&CK Basics

## Overview
MITRE ATT&CK is a knowledge base that documents how real-world adversaries operate. SOC analysts use it to understand attacks, map suspicious behavior, improve detections, and organize threat-hunting activities .

## What ATT&CK Means
ATT&CK stands for **Adversarial Tactics, Techniques, and Common Knowledge**.

It is made up of:
- **Tactics**: why an attacker performs an action.
- **Techniques**: how an attacker achieves a tactical goal.
- **Sub-techniques**: more specific versions of techniques.
- **Procedures**: real-world examples of how an adversary used a technique .

## MITRE ATT&CK Matrix
The Enterprise Matrix organizes attacker behavior into tactics. Each tactic contains techniques and sub-techniques that describe how attackers achieve their objectives .

## Main Enterprise Tactics
- **Reconnaissance**: gathering information about a target.
- **Resource Development**: preparing resources for an attack.
- **Initial Access**: getting into a network or system.
- **Execution**: running malicious code.
- **Persistence**: maintaining access to a compromised system.
- **Privilege Escalation**: gaining higher-level permissions.
- **Stealth**: hiding malicious activity.
- **Defense Impairment**: weakening security tools or controls.
- **Credential Access**: stealing usernames, passwords, or other credentials.
- **Discovery**: learning about systems, users, and the network.
- **Lateral Movement**: moving from one compromised system to another.
- **Collection**: gathering data of interest.
- **Command and Control**: communicating with compromised systems.
- **Exfiltration**: stealing data from the environment.
- **Impact**: disrupting, damaging, or destroying systems or data .

## Tactics vs Techniques
A tactic describes the attacker’s **goal**, while a technique describes the attacker’s **method**.

Example:
- Tactic: **Persistence**
- Technique: **Scheduled Task/Job**
- Procedure: Creating a scheduled task that launches malware after a user logs in .

## Common Techniques

| Technique | ID | Tactic | Example |
|---|---|---|---|
| Phishing | T1566 | Initial Access | Sending a malicious email to a user. |
| PowerShell | T1059.001 | Execution | Using PowerShell to run commands or scripts. |
| Scheduled Task/Job | T1053 | Persistence | Creating a task that launches malware automatically. |
| Valid Accounts | T1078 | Initial Access / Persistence | Using stolen credentials to access a system. |
| Remote Services | T1021 | Lateral Movement | Using RDP or SMB to access another host. |
| System Information Discovery | T1082 | Discovery | Checking the operating system and system details. |
| Obfuscated Files or Information | T1027 | Defense Evasion | Hiding commands or malware code. |
| Data Encrypted for Impact | T1486 | Impact | Encrypting files during a ransomware attack. |

## Example: Suspicious PowerShell
Suppose an alert shows an encoded PowerShell command launched by an unusual parent process.

A SOC analyst may map it to:
- **Tactic**: Execution.
- **Technique**: T1059.001 — PowerShell.
- **Possible technique**: T1027 — Obfuscated Files or Information.
- **Evidence**: Windows Event ID 4104, process creation logs, command-line arguments, and the parent process.

The analyst should then investigate the user, host, command, timestamp, network connections, and whether the same activity occurred elsewhere.

## Example: Windows Persistence
Suppose a new scheduled task launches a suspicious executable after system startup.

The activity may map to:
- **Tactic**: Persistence.
- **Technique**: T1053 — Scheduled Task/Job.
- **Evidence**: Windows Event ID 4698, Task Scheduler logs, Sysmon process events, and the executable path.

The analyst should verify who created the task, when it was created, what it launches, and whether the file is malicious.

## How SOC Analysts Use ATT&CK
SOC analysts use MITRE ATT&CK to:
- classify alerts,
- map attacker behavior,
- create detection rules,
- build threat-hunting queries,
- identify missing telemetry,
- support incident reports,
- and measure detection coverage .

## Detection Mapping Example

| Evidence | Possible ATT&CK Mapping |
|---|---|
| Event ID 4104 with encoded PowerShell | T1059.001 PowerShell and possibly T1027 |
| Event ID 4698 for a new scheduled task | T1053 Scheduled Task/Job |
| Event ID 4624 from an unusual source | T1078 Valid Accounts or remote access investigation |
| Event ID 4688 showing suspicious process execution | Execution technique investigation |
| Large outbound transfer to an unusual destination | Exfiltration technique investigation |

## Important Limitation
An ATT&CK mapping is not proof that an attack occurred. It describes behavior that may be consistent with a technique. Analysts still need to validate the user, host, command, timing, context, and supporting evidence.

## Key Takeaway
MITRE ATT&CK provides a common language for describing attacker behavior. For SOC work, the most useful process is:

1. Identify the suspicious behavior.
2. Find the related tactic.
3. Map it to a technique or sub-technique.
4. Review supporting logs.
5. Document the evidence and response.

## References
- **MITRE ATT&CK Getting Started:** MITRE. *Get Started* — https://attack.mitre.org/resources/ 
- **Enterprise Matrix:** MITRE. *Enterprise Matrix* — https://attack.mitre.org/matrices/enterprise/ 
- **Enterprise Tactics:** MITRE. *Enterprise Tactics* — https://attack.mitre.org/tactics/enterprise/ 
- **Enterprise Techniques:** MITRE. *Enterprise Techniques* — https://attack.mitre.org/techniques/enterprise/ 
- **MITRE Coverage:** Microsoft Learn. *View MITRE ATT&CK framework coverage in Microsoft Sentinel* — https://learn.microsoft.com/en-us/azure/sentinel/mitre-coverage 
