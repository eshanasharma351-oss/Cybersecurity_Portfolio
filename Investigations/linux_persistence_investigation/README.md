# Linux Persistence Investigation

## Overview

I performed a defensive investigation on an Ubuntu VM to identify common Linux persistence mechanisms that could allow unauthorized access or command execution after reboot, login, or on a schedule.

**Result:** No suspicious persistence artifacts were identified during this review.

## Environment

- **Operating System:**  [ubuntu 26.04 LTS]
- **VM Platform:** [ VMware]
- **Network Mode:** [NAT ]
- **Method:** Read-only investigation

## Investigation Areas

| Area | What I Checked |
|---|---|
| User activity | Logged-in users and active sessions |
| Processes and ports | Running processes and listening services |
| Authentication logs | Failed logins, SSH activity, and sudo usage |
| Cron jobs | User, root, and system-wide scheduled tasks |
| Systemd | Enabled services and scheduled timers |
| SSH | `authorized_keys` files for passwordless access |
| Startup files | `.bashrc`, `.profile`, and `/etc/profile.d/` |
| Privilege paths | Sudo rules and `/etc/ld.so.preload` |

## Commands Used

```bash
# Users, processes, and ports
who
w
ps auxf
sudo ss -tulnp

# Authentication activity
sudo grep -Ei "failed|accepted|sudo|session opened" /var/log/auth.log | tail -n 20

# Cron persistence
crontab -l
sudo crontab -l
sudo cat /etc/crontab


# Systemd persistence
systemctl list-unit-files --state=enabled
systemctl list-timers --all

# SSH and startup persistence
ls -la ~/.ssh/
cat  ~/.ssh/authorized_keys
ls -la ~/.bashrc ~/.profile
grep -nEv '^#|^$' ~/.bashrc ~/.profile
sudo ls -la /etc/profile.d/

# Privilege and preload checks
sudo -l
sudo ls -la /etc/sudoers.d/
sudo grep -vE '^[[:space:]]*#|^[[:space:]]*$' /etc/sudoers
sudo cat /etc/ld.so.preload

## Findings

| Check | Result |
|---|---|
| Active users | Only expected local-user activity observed |
| Running processes | No suspicious process path identified |
| Listening ports | No unexpected listening service identified |
| Authentication logs | No suspicious login pattern observed |
| Cron jobs | No unexpected user, root, or system cron job identified |
| Systemd services and timers | No suspicious enabled service or timer identified |
| SSH authorized keys | No unexpected SSH key identified |
| Startup files | No suspicious startup command identified |
| Sudo and `LD_PRELOAD` | No unexpected sudo rule or preload configuration identified |

## MITRE ATT&CK Mapping

| Persistence / Privilege Mechanism | Technique |
|---|---|
| Cron jobs | T1053.003 — Scheduled Task/Job: Cron |
| Systemd services | T1543.002 — Create or Modify System Process: Systemd Service |
| Systemd timers | T1053.006 — Scheduled Task/Job: Systemd Timers |
| SSH authorized keys | T1098.004 — Account Manipulation: SSH Authorized Keys |
| Shell startup files | T1546.004 — Event Triggered Execution: Unix Shell Configuration Modification |
| Sudo rules | T1548.003 — Abuse Elevation Control Mechanism: Sudo and Sudo Caching |
| LD_PRELOAD | T1574.006 — Hijack Execution Flow: Dynamic Linker Hijacking |


## References

- [MITRE ATT&CK Enterprise Matrix](https://attack.mitre.org/matrices/enterprise/linux/)
- [MITRE ATT&CK — Persistence](https://attack.mitre.org/tactics/TA0003/)