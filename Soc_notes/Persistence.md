# Persistence

## Overview
Persistence is how an attacker maintains access to a system after the initial compromise. It allows malicious access to survive logouts, reboots, and other interruptions .

## Definition
In cybersecurity, persistence refers to the ability of a threat actor or malware to remain active in a compromised environment over time .

## Why Persistence Matters
Persistence is important because it helps attackers keep long-term access, steal data, move laterally, and continue malicious activity without being easily removed .

## Common Persistence Techniques
Common persistence methods include:
- Scheduled tasks.
- Services.
- Registry run keys.
- Startup folder items.
- WMI event subscriptions.
- Linux cron jobs.
- SSH authorized keys .

## Windows Persistence Examples
On Windows, attackers may use scheduled tasks, new services, registry changes, or suspicious process execution to remain active after reboot.

## Linux Persistence Examples
On Linux, persistence can appear in cron jobs, startup scripts, shell profiles, or modified SSH keys used for remote access .

## Detection Indicators
Look for:
- Rare or suspicious scheduled tasks.
- New or unknown services.
- Registry changes in run keys.
- Suspicious process creation.
- Unusual startup entries.
- Strange cron entries.
- Unexpected SSH key changes.

## Logs and Evidence
Useful logs and event sources include:
- Windows Security Event ID 4698 for scheduled task creation.
- Windows Security Event ID 4697 for service installation.
- Windows Security Event ID 4688 for process creation.
- Windows Security Event ID 4657 for registry modifications when auditing is enabled.
- Sysmon registry and process events for deeper visibility .

## MITRE ATT&CK Mapping
Persistence techniques often map to MITRE ATT&CK persistence tactics such as scheduled task execution, service creation, and registry-based startup mechanisms .

## SOC Relevance
For a SOC analyst, persistence is a high-value concept because it often indicates an attacker trying to stay hidden and maintain long-term access. Detecting it early can stop deeper compromise .

## Key Takeaway
Persistence is one of the clearest signs that an attacker is trying to survive and stay inside a system. Strong logging and careful correlation help defenders find it before it causes more damage .

## References
Microsoft Learn. Understanding Sysmon events — useful for persistence detection and timeline analysis. https://learn.microsoft.com/en-us/windows/security/operating-system-security/sysmon/sysmon-events
