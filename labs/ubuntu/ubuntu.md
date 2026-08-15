# Ubuntu Lab

## Objective

The objective of this lab was to practice basic Linux administration and investigation skills on an Ubuntu virtual machine.


## Environment

- **Operating system:** [ubuntu 26.04 LTS]
- **VM platform:** [ VMware]
- **RAM:** [6 GB]
- **CPU:** [ 2 virtual CPUs]
- **Disk:** [40 GB]
- **Network configuration:** [ NAT ]

## Tools 

| Command | Purpose |
|---|---|
| `pwd` | Display the current directory. |
| `ls -la` | List files, including hidden files and permissions. |
| `cd` | Change directories. |
| `find` | Search for files and directories. |
| `grep` | Search text within files or command output. |
| `cat` | Display file contents. |
| `less` | Read large files page by page. |
| `whoami` | Display the current user. |
| `id` | Display user and group information. |
| `sudo` | Run a command with elevated privileges. |
| `ps aux` | Display running processes. |
| `top` | Monitor running processes in real time. |
| `systemctl` | Manage and inspect systemd services. |
| `journalctl` | Read systemd journal logs. |
| `ss -tulnp` | Display listening network sockets. |
| `df -h` | Display disk usage. |
| `free -h` | Display memory usage. |
| `uname -a` | Display kernel and system information. |

## Linux Log Locations

Important Linux log locations include:

- `/var/log/auth.log` — authentication and privilege-related activity.
- `/var/log/syslog` — general system activity.
- `/var/log/kern.log` — kernel-related messages.
- `/var/log/dpkg.log` — package installation and removal activity.
- `/var/log/apt/` — APT package manager logs.
- `/var/log/cron.log` — scheduled job activity, when enabled.
- `journalctl` — systemd journal entries.

Useful commands:

```bash
sudo less /var/log/auth.log
sudo grep -i "failed\|accepted\|sudo" /var/log/auth.log
sudo journalctl -b
sudo journalctl -p err
sudo journalctl -u ssh
sudo journalctl --since "1 hour ago"
```

`journalctl` is used to query logs stored by `systemd-journald`. Access to complete system logs is normally restricted to root and selected privileged groups [145].

## Users & Permissions

Linux uses users, groups, and file permissions to control access to resources.

Useful commands:

```bash
whoami
id
getent passwd
getent group
ls -l
stat /etc/passwd
sudo -l
```

Linux permissions are represented as:

```text
-rwxr-xr--
```

The permissions are divided into:

- **Owner** permissions.
- **Group** permissions.
- **Other users’** permissions.

Permission types include:

- `r` — read.
- `w` — write.
- `x` — execute.

Important files to review during a defensive investigation include:

- `/etc/passwd` — local user account information.
- `/etc/group` — group membership information.
- `/etc/shadow` — password hash information; normally restricted.
- `/etc/sudoers` — sudo configuration.
- `/etc/sudoers.d/` — additional sudo rules.

Example investigation commands:

```bash
getent passwd
getent group sudo
sudo cat /etc/sudoers
sudo ls -la /etc/sudoers.d/
find /home -type f -perm /4000 2>/dev/null
```

Unexpected users, unauthorized group membership, unusual sudo rules, or suspicious SUID files may require further investigation.

## Processes & Services

Processes are currently running programs. Services are background programs that may start automatically during boot.

Useful process commands:

```bash
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
top
pgrep -a ssh
pstree
```

Useful service commands:

```bash
systemctl --type=service --state=running
systemctl --failed
systemctl status ssh
systemctl list-unit-files --state=enabled
journalctl -u ssh
```

Network-related process checks:

```bash
ss -tulnp
sudo lsof -i -P -n
```

During an investigation, check for:

- Unknown processes.
- Processes running from temporary directories.
- Unexpected parent-child process relationships.
- Services enabled without a business reason.
- Programs listening on unusual ports.
- Services running as root unnecessarily.

## Persistence Mechanisms

Persistence allows an attacker or unauthorized program to remain active after logout or reboot.

Common Linux persistence locations include:

- `/etc/cron.d/`
- `/etc/cron.daily/`
- `/etc/cron.hourly/`
- `/var/spool/cron/`
- `/etc/systemd/system/`
- `/lib/systemd/system/`
- `/etc/rc.local`
- User shell profiles such as `~/.bashrc` and `~/.profile`
- SSH authorized keys in `~/.ssh/authorized_keys`

Useful defensive checks:

```bash
crontab -l
sudo crontab -l
sudo ls -la /etc/cron.d/
sudo ls -la /var/spool/cron/
systemctl list-unit-files --state=enabled
sudo find /etc/systemd/system /lib/systemd/system -type f -ls
find ~/.ssh -maxdepth 2 -type f -ls
grep -Rni "curl\|wget\|bash\|nc\|python" ~/.bashrc ~/.profile 2>/dev/null
```

When reviewing a suspicious persistence mechanism, record:

- File or service name.
- Creation and modification times.
- Owner and permissions.
- Command that will execute.
- User context.
- Destination IP or domain.
- Related log entries.

Do not delete suspicious files immediately. Preserve the evidence first and follow the approved incident response process.



## What I Learned

During this lab, I learned how to:

- Navigate the Ubuntu file system using the command line.
- Locate and review important Linux logs.
- Identify users, groups, and file permissions.
- Inspect running processes and system services.
- Check listening network ports.
- Investigate common Linux persistence locations.
- Collect basic evidence before making changes.
- Use commands such as `journalctl`, `systemctl`, `ps`, `ss`, `find`, and `grep` during security investigations.


## Key Takeaway

A basic Ubuntu lab provides a foundation for Linux investigation. Knowing where logs are stored and how to inspect users, permissions, processes, services, and persistence mechanisms helps a SOC analyst understand what happened on a system.

## References

- **Linux system logs:** Ubuntu Manpages. *journalctl — Print log entries from the systemd journal* — https://manpages.ubuntu.com/manpages/noble/man1/journalctl.1.html 
- **Sysmon and event-based monitoring:** Microsoft Learn. *Understanding Sysmon events* — https://learn.microsoft.com/en-us/windows/security/operating-system-security/sysmon/sysmon-events 