# Filesystem Audit Basics (Linux)

This guide covers two complementary approaches to monitor and audit filesystem changes:
- auditd: real-time monitoring of system calls and file events.
- AIDE: periodic integrity checking against a known-good baseline.

Note: Commands below use Arch Linux package manager (pacman). Adjust for your distro as needed.

## 1) auditd setup (real-time monitoring)

Install and enable the service:

```bash
sudo pacman -S audit
sudo systemctl enable --now auditd
```

Check status:

```bash
sudo auditctl -s
```

Expected output should show enabled and rules loaded.

Basic rule example (monitor edits to /etc/passwd):

```bash
sudo auditctl -a always,exit -F arch=b64 -F path=/etc/passwd -F perm=wa -F key=passwd_changes
```

View logs for the above rule key:

```bash
sudo ausearch -k passwd_changes
```

Persist rules across reboot by placing them in a file such as:

- /etc/audit/rules.d/custom.rules

Example content to persist the rule:

```bash
-a always,exit -F arch=b64 -F path=/etc/passwd -F perm=wa -F key=passwd_changes
```

After editing rules, reload:

```bash
sudo augenrules --load
```

## 2) AIDE setup (integrity checker)

Install:

```bash
sudo pacman -S aide
```

Initialize the database (baseline snapshot of current filesystem):

```bash
sudo aide --init
sudo mv /var/lib/aide/aide.db.new /var/lib/aide/aide.db
```

Run a check (compare current system state to baseline):

```bash
sudo aide --check
```

This will report any changes (file content, permissions, ownership, etc.).

Update database after legitimate changes:

```bash
sudo aide --update
sudo mv /var/lib/aide/aide.db.new /var/lib/aide/aide.db
```

Automate via cron or a systemd timer. Example cron entry (daily check at 02:00):

```bash
sudo crontab -e
# Add the following line
0 2 * * * /usr/bin/aide --check | mail -s "AIDE Report" you@example.com
```

Tip: On systems without a local MTA, pipe to a file or use a mail relay.

## Quick validation

1. Add a harmless change to /etc/passwd (e.g., a comment line) with sudo.
2. Observe that:
   - auditd logs the write event in real time (ausearch with the key).
   - AIDE flags it later as an unexpected file modification during the next check.

---


