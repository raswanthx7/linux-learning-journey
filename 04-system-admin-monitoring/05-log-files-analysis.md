# Log Files & Analysis

##  Topics Covered
- View logs: `journalctl`, `/var/log/messages`, `dmesg`
- Track errors and system issues
- Introduction to log rotation

---

##  Key Notes

###  Log Files in Linux
- Linux stores system logs in `/var/log/`
  - `/var/log/messages` → General system logs
  - `/var/log/secure` → Security/authentication logs
  - `/var/log/cron` → Cron job logs
  - `/var/log/dmesg` → Boot and kernel ring buffer

###  Viewing Logs
- `journalctl` (used with `systemd`):
  - `journalctl` → View all logs
  - `journalctl -xe` → View recent logs with extra info
  - `journalctl -u sshd` → Logs of specific service (e.g., SSH)

- `dmesg` → View kernel and boot messages

###  Log Monitoring
- Use `tail -f /var/log/messages` to monitor logs in real-time
- Use `grep` to filter specific errors or warnings

###  Log Rotation (Intro)
- Linux uses `logrotate` to:
  - Automatically archive logs
  - Compress old logs
  - Prevent disk overuse

- Config file: `/etc/logrotate.conf`
- Log rotation runs usually via cron

---

##  Commands Practiced

```bash
journalctl                     # View all logs
journalctl -xe                # View recent system errors
journalctl -u sshd            # Logs for SSH service
dmesg                          # Kernel and boot logs
cat /var/log/messages          # General system log
grep error /var/log/messages  # Search for 'error' in system log
tail -f /var/log/secure        # Live view of secure log
less /var/log/cron             # View cron logs
```

---
