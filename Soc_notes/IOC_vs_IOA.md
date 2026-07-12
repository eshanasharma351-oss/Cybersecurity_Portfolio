# IOC vs IOA

## Overview

Indicators of Compromise (IOCs) and Indicators of Attack (IOAs) are important concepts in cybersecurity used to identify and respond to malicious activity.

An **Indicator of Compromise (IOC)** is evidence that a system has been compromised or that malicious activity has already occurred. IOCs are typically used in a **reactive** manner, helping security analysts investigate incidents and determine the extent of a breach.

An **Indicator of Attack (IOA)** focuses on attacker behavior and suspicious activities that may indicate an attack is in progress. IOAs are generally considered a **proactive** detection approach because they can help identify malicious actions before an attacker achieves their objective.

Together, IOCs and IOAs improve an organization's ability to detect, investigate, and respond to cyber threats effectively.

## Learning Objectives

- Understand what an IOC is.
- Understand what an IOA is.
- Differentiate between IOCs and IOAs.
- Identify examples of IOCs and IOAs.
- Understand how security analysts use them during investigations.

## what is an IOC?

Indicators of Compromise (IOCs) are digital clues or forensic evidence left behind after a cyberattack. They may not prove a breach on their own, but they indicate suspicious activity that helps security teams investigate possible compromise

examples:
| IOC type | Example | Detection methods |
| --- | --- | --- |
| Network | Malicious IP address, suspicious domain | Firewall logs, DNS logs, proxy logs, intrusion detection systems |
| Host | Unknown process, unusual file hash | EDR, antivirus, file integrity monitoring |
| Email | Phishing sender address, malicious attachment | Email security gateway, header inspection, spam filtering |
| Behavior | Foreign login, unusual data transfer | Authentication logs, UEBA, SIEM correlation |
 
## what is an IOA?

 Indicators of Attack (IOAs) are signs of attacker behavior that suggest an attack is happening or about to happen . Unlike IOCs, which are more about evidence left behind after compromise, IOAs focus on what the attacker is trying to do during the attack .That makes IOAs useful for early detection. Security teams look at patterns such as unusual logins, suspicious command execution, privilege escalation, lateral movement, and unexpected data transfer to catch malicious activity before major damage happens.

 examples:
 | IOA type | Example | Detection methods |
| --- | --- | --- |
| Reconnaissance | Port scanning, account enumeration, asset discovery | SIEM correlation, IDS/IPS alerts, firewall logs, threat hunting |
| Initial access | Phishing clicks, exploit attempts, credential misuse | Email security gateway, web proxy logs, authentication logs, EDR |
| Execution | Suspicious PowerShell, encoded commands, unusual script activity | EDR, process monitoring, command-line auditing, SIEM |
| Privilege escalation | Token manipulation, credential dumping, unauthorized admin actions | Privileged access logs, endpoint telemetry, EDR, SIEM |
| Lateral movement | Abnormal RDP, SMB, WMI, or WinRM activity between hosts | Network monitoring, internal traffic analysis, EDR, SIEM |
| Persistence | New scheduled tasks, startup changes, service creation | Host-based monitoring, file integrity monitoring, EDR |
| Exfiltration or impact | Unusual outbound data transfer, mass file encryption, log deletion | Anomaly detection, SIEM, EDR, backup alerts |


# IOA vs IOC

| IOC | IOA |
| --- | --- |
| Evidence of compromise | Behavior of attack |
| Reactive | Proactive |
| Used after or during a breach | Used during an attack |
| Helps with investigation | Helps with early detection |
| Example: malicious hash, IP, domain | Example: privilege escalation, lateral movement, suspicious PowerShell |

## Why IOA  matters more today?
- IOAs can catch an attack while it is happening, which gives defenders more time to block it .
- They are more resilient than IOCs because attackers can quickly change IPs, hashes, domains, and file names, but their behavior is harder to hide completely .
- IOAs help security teams detect unknown or new threats, not just known malicious signatures .
-They support proactive defense, threat hunting, and real-time response instead of only post-incident investigation .

## MITRE ATT&CK Mapping
MITRE ATT&CK is a globally accessible knowledge base of adversary tactics and techniques based on real-world observations . In this project, I used MITRE ATT&CK to map observed IOC and IOA findings to common attacker behaviors such as Initial Access, Execution, Persistence, Credential Access, Lateral Movement, and Exfiltration .
This mapping helps turn raw alerts into meaningful security context. Instead of just identifying suspicious activity, it shows where that activity fits in the attack lifecycle and how a SOC analyst would respond . It also makes detections easier to compare, document, and improve over time .

| SOC investigation example | MITRE ATT&CK tactic | MITRE ATT&CK technique |
| --- | --- | --- |
| Multiple failed logins followed by a successful login from a new location | Initial Access / Valid Accounts | Valid Accounts |
| Phishing email with a malicious link or attachment | Initial Access | Phishing |
| Word launching PowerShell | Execution | Command and Scripting Interpreter |
| New scheduled task created on a workstation | Persistence | Scheduled Task/Job |
| Suspicious admin privilege changes | Privilege Escalation | Abuse Elevation Control Mechanism |
| Repeated RDP connections to another internal host | Lateral Movement | Remote Services |
| Large outbound file transfer to an unknown domain | Exfiltration | Exfiltration Over C2 Channel |
| Unknown process connecting to a rare external IP | Command and Control | Application Layer Protocol |


## challenges and limitations
- IOC can become outdated quickly when attackers rotate infrastructure or change file hashes .
- IOC can create false positives if the same artifact is used by benign software.
- IOA requires more tuning an context to avoid alert noise .
- IOA can be harder to build because behaviour based detections need good telemetry coverage.
- both need continuous testing , updates and validation.

## key takeaway
- IOC is best for confirming and investigating compromise .
- IOA best for detecting attacker behaviour early .
- MITRE ATT&CK helps map detection to real advisory techniques. 
- combining IOC and IOA gives better visibillity than using only one approach.

## References
- CrowdStrike, IOA vs IOC: Understanding the Differences — https://www.crowdstrike.com/en-us/cybersecurity-101/threat-intelligence/ioa-vs-ioc/
- MITRE ATT&CK — https://attack.mitre.org/
- RFC 9424 — https://datatracker.ietf.org/doc/rfc9424/
- CISA Best Practices for MITRE ATT&CK Mapping — https://www.cisa.gov/sites/default/files/2023-01/Best%20Practices%20for%20MITRE%20ATTCK%20Mapping.pdf