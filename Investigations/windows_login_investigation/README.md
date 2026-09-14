# Windows Security Event Investigation

## Overview

I performed a defensive investigation using the Windows Security Event Log to review authentication activity, account lockouts, credential validation, logoff activity, and process creation.

The investigation focused on the following Windows Event IDs:

- **4624** — Successful Logon
- **4625** — Failed Logon
- **4634** — Account Logoff
- **4647** — User Initiated Logoff
- **4648** — Explicit Credential Logon
- **4672** — Special Privileges Assigned to New Logon
- **4740** — A User Account Was Locked Out
- **4771** — Kerberos Pre-Authentication Failed
- **4776** — Domain Controller Attempted to Validate Credentials
- **4688** — A New Process Has Been Created



---

## Environment

- **Operating System:** Windows 11
- **Platform:** Windows PC
- **Investigation Tool:** Windows Event Viewer
- **Log Reviewed:** Windows Security Log

---

## Objective

The objective of this investigation was to analyze Windows security events and identify unusual authentication, account, and process activity.

The investigation examined:

- Successful and failed logons
- User logoff activity
- Explicit credential usage
- Privileged logons
- Account lockouts
- Kerberos authentication failures
- Credential validation activity
- Process creation

---

# Evidence Collected

## 1. Successful Logon — Event ID 4624

Event ID **4624** was reviewed to identify successful logon activity.

### Information Reviewed

- Account Name
- Account Domain
- Logon Type
- Logon ID
- Timestamp
- Workstation Name
- Source Network Address, when available



### Finding

Multiple successful logon events (Event ID 4624) were observed in the Windows Security Log. The events were recorded as **Audit Success** on the local Windows system. No suspicious activity was identified from the available 4624 event information.

## 2. Failed Logon — Event ID 4625

Event ID **4625** was reviewed to identify unsuccessful authentication attempts.

### Information Reviewed

- Account Name
- Failure Reason
- Logon Type
- Timestamp
- Workstation Name
- Source Network Address, when available


### Finding
A failed logon attempt was recorded for account esham at [11-09-2026/20:21:35]. The failure reason was [Unknown username or bad password]. No repeated or unusual pattern was observed during the reviewed period.

## 3. Account Logoff — Event ID 4634

Event ID **4634** was reviewed to identify when a logon session was terminated.

### Information Reviewed

- Account Name
- Logon ID
- Logon Type
- Timestamp



### Finding
A logoff event was recorded for account esham at [12-09-26/23:02:30], indicating that the logon session was terminated.


---

## 4. User Initiated Logoff — Event ID 4647

Event ID **4647** was reviewed to identify user-initiated logoff activity.

### Information Reviewed

- Account Name
- Logon ID
- Timestamp



### Finding

A user-initiated logoff was recorded for account esham at [12-09-2026/20:50:07]. This indicates that the user initiated the logoff.

---

## 5. Explicit Credential Logon — Event ID 4648

Event ID **4648** was reviewed to identify instances where explicit credentials were used for authentication.

### Information Reviewed

- Account Name
- Target Account
- Target Server
- Process Name
- Network Address
- Timestamp



### Finding

An explicit credential logon was recorded for account esham. The event showed [C:\Windows\sytem32\Isass.exe/localhost] using supplied credentials. No suspicious activity was identified from this event alone.
---

## 6. Special Privileges Assigned — Event ID 4672

Event ID **4672** was reviewed to identify logons associated with special privileges.

### Information Reviewed

- Account Name
- Logon ID
- Assigned Privileges
- Timestamp



### Finding
Special privileges were assigned to account esham during logon.  This is consistent with administrative activity if the account is expected to have elevated privileges.

---

## 7. Account Lockout — Event ID 4740

Event ID **4740** was reviewed to identify accounts that were locked out because of repeated unsuccessful authentication attempts.

### Information Reviewed

- Target Account Name
- Caller Computer Name
- Timestamp
- Account Domain


### Finding

### 4740 — Account Lockout

**Finding:** No Event ID 4740 was observed during the reviewed period. No account lockout activity was recorded for the reviewed user account.
---

## 8. Kerberos Pre-Authentication Failure — Event ID 4771

Event ID **4771** was reviewed to identify failed Kerberos pre-authentication attempts.

### Information Reviewed

- Account Name
- Client Address
- Failure Code
- Service Name
- Timestamp



### Finding

### 4771 — Kerberos Authentication Failure

**Finding:** No Event ID 4771 was observed during the reviewed period. No Kerberos pre-authentication failure was recorded for the reviewed user account.

---

## 9. Credential Validation — Event ID 4776

Event ID **4776** was reviewed to identify credential validation activity.

### Information Reviewed

- Account Name
- Source Workstation
- Authentication Package
- Error Code
- Timestamp



### Finding

### 4776 — Credential Validation

**Finding:** No Event ID 4776 was observed during the reviewed period. No credential validation activity was recorded for the reviewed user account.

---

## 10. Process Creation — Event ID 4688

Event ID **4688** was reviewed to identify newly created processes.

### Information Reviewed

- New Process Name
- Creator Process Name
- Account Name
- Process ID
- Parent Process
- Command Line, when available


### Finding

A new process [C:\Windows\System32\Isass.exe] was created by [C:\Windows\System32\wininit.exe] under account esham. The process activity appeared [normal] based on the available information.

---

# Event Analysis

| Event ID | Event | What It Helps Investigate |
|---|---|---|
| 4624 | Successful Logon | Successful authentication |
| 4625 | Failed Logon | Failed authentication |
| 4634 | Account Logoff | Session termination |
| 4647 | User Initiated Logoff | User-initiated session termination |
| 4648 | Explicit Credential Logon | Use of supplied credentials |
| 4672 | Special Privileges | Privileged logons |
| 4740 | Account Lockout | Repeated authentication failures |
| 4771 | Kerberos Pre-Auth Failure | Kerberos authentication failures |
| 4776 | Credential Validation | Credential validation activity |
| 4688 | Process Creation | New process execution |

---



# Analysis

The collected events were compared to determine whether there was a suspicious authentication pattern.

The following questions were considered:

- Were there repeated failed logons?
- Did failed logons occur before a successful logon?
- Was an account locked after repeated failures?
- Were Kerberos authentication failures observed?
- Were credentials validated from an unusual workstation?
- Was explicit credential usage observed?
- Did privileged logons occur?
- Were suspicious processes created around the same time?
- Did the account, source system, and timestamps match expected activity?



# Conclusion

Based on the Windows Security Event Log review:

**Conclusion:** Normal activity


No conclusions were made based on a single event alone. Events were reviewed together with their timestamps, accounts, source information, and surrounding activity.



