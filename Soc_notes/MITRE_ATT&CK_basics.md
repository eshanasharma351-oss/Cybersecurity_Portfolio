# MITRE ATT&CK

## Overview
MITRE ATT&CK is a knowledge base of real attacker behavior. It helps SOC analysts understand how attacks work, map activity to techniques, and improve detection coverage [web:374][web:373].

## What It Tracks
ATT&CK organizes adversary behavior into:
- **Tactics** = the attacker’s goal.
- **Techniques** = the method used.
- **Sub-techniques** = a more specific variation of a technique.
- **Procedures** = the real-world action seen in an incident [web:374][web:391].

## Main Tactics
Some of the most important enterprise tactics are:
- Initial Access.
- Execution.
- Persistence.
- Privilege Escalation.
- Defense Evasion.
- Credential Access.
- Discovery.
- Lateral Movement.
- Collection.
- Command and Control.
- Exfiltration.
- Impact [web:383][web:391].

## Example Techniques
Here are common techniques a SOC analyst may see:
- Phishing.
- Command and Scripting Interpreter.
- Scheduled Task/Job.
- Registry Run Keys/Startup Folder.
- Valid Accounts.
- Remote Services.
- PowerShell.
- System Information Discovery.
- Obfuscated Files or Information [web:383][web:385].

## Real-World Mapping
If an attacker uses a malicious email attachment, that can map to **Phishing** under Initial Access. If they create a scheduled task to survive reboot, that maps to **Scheduled Task/Job** under Persistence [web:383][web:391].

If they run encoded PowerShell to hide commands, that may map to **Command and Scripting Interpreter** and **Defense Evasion** depending on the behavior [web:385][web:379].

## Why Analysts Use It
SOC teams use ATT&CK to:
- map alerts to attacker behavior,
- identify detection gaps,
- build hunting queries,
- and improve SIEM coverage [web:379][web:393].

## Microsoft Sentinel Connection
Microsoft Sentinel can show MITRE coverage for active detections and available rules. This helps analysts see which techniques are already covered and where they still need detections [web:379].

## Key Takeaway
MITRE ATT&CK gives security teams a structured way to describe and detect attacker behavior. It is especially useful when turning raw logs into meaningful incident response and threat hunting data [web:374][web:379].