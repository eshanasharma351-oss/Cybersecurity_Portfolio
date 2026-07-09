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
| IOC type | Example |
| Network  | Malicious IP address, suspicious domain |
| Host     | Unknown process, unusual file hash |
| Email    | Phishing sender address, malicious attachment |
| Behavior | Foreign login, unusual data transfer |

Detection methods for IOC 
 - firewall logs, DNS logs, proxy logs, SIEM
 - EDR , antivrus , file integrity monitoring
 - email gateway , harder analysis , spam filtering 
 - authentaiction logs , SIEM correlation , UEBA

 ## what is an IOA?

 Indicators of Attack (IOAs) are signs of attacker behavior that suggest an attack is happening or about to happen . Unlike IOCs, which are more about evidence left behind after compromise, IOAs focus on what the attacker is trying to do during the attack .That makes IOAs useful for early detection. Security teams look at patterns such as unusual logins, suspicious command execution, privilege escalation, lateral movement, and unexpected data transfer to catch malicious activity before major damage happens.

 examples:
| IOA type              | Example | 

| Reconnaissance        | Port scanning, account enumeration, asset discovery 
| Initial access        | Phishing clicks, exploit attempts, credential misuse 
| Execution             | Suspicious PowerShell, encoded commands, unusual script activity
| Privilege escalation  | Token manipulation, credential dumping, unauthorized admin actions | 
| Lateral movement      | Abnormal RDP, SMB, WMI, or WinRM activity between hosts | 
| Persistence           | New scheduled tasks, startup changes, service creation | 
| Exfiltration or impact| Unusual outbound data transfer, mass file encryption, log deletion | 

Detection methods for IOA
- SIEM correlation, IDS/IPS alerts, firewall logs, threat hunting 
- Email security gateway, web proxy logs, authentication logs, EDR 
- Privileged access logs, endpoint telemetry,  SIEM 
- Network monitoring, internal traffic analysis
- Host-based monitoring, file integrity monitoring 
- Anomaly detection,  backup alerts 

# IOA vs IOC

| IOC | IOA |
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