# Alert Triage

## Overview
An alert is a notification generated when a security tool detects something that matches a rule or suspicious pattern.

Noise refers to unimportant alerts, such as user typos, parser errors, or legitimate software updates. Noise clutters the dashboard and makes real threats harder to spot.

Signal refers to useful alerts, such as malware detection or privilege escalation.

## Alert Triage
Alert triage is the systematic process of evaluating, prioritizing, and routing security alerts to determine whether they are benign, suspicious, or require escalation.

## Goal
The purpose of triage is to reduce noise, focus on real threats, and decide the right response quickly.

## Alert Fields
- Source.
- Timestamp.
- Severity.
- User.
- Host.
- Rule name.
- Status.
- Related events or logs.

## Triage Steps
1. Read the alert.
2. Check the severity and asset importance.
3. Gather context from logs.
4. Correlate related activity.
5. Check threat intelligence.
6. Decide the next action.

## Decision Outcomes
- **True positive**: The alert fired and a real threat exists.
- **False positive**: The alert fired, but no threat exists.
- **Benign activity**: The behavior is real but authorized, expected, or policy-compliant.
- **Needs further investigation**: There is insufficient evidence to make a final decision.

## Escalation
Escalate when the alert is confirmed suspicious, affects critical assets, shows signs of active compromise, or requires deeper investigation by a higher-tier analyst.

## Alert Fatigue
Alert fatigue happens when analysts receive too many alerts, making it harder to identify important ones.

## Detection Tuning
Detection tuning means improving rules to reduce false positives while still catching attacks.

## Key Takeaway
Alert triage is a core SOC skill because it helps analysts separate real threats from noise and respond in a structured, consistent way.

## References
- CyberDefenders, “What Is the Alert Triage Process? SOC Guide.”
- Networkers Home, “Alert Triage — Investigation Workflow for SOC Analysts.”
- Rapid7, “What Is Alert Triage in Cybersecurity?”
- Corelight, “What is Alert Triage in Cybersecurity? Steps, Challenges, and Tips.”
- MyCyberSecurityPath, “Alert Triage & Escalation: SOC Analyst Core Skill.”
- Strike48, “Alert Triage: A Practical Guide for SOC Teams.”