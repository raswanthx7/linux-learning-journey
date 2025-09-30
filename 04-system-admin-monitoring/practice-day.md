#  Practice Day


##  Topics Covered:
- Troubleshoot broken service
- Fix script + log permissions
- Write a cron job for log backup

---

##  Key Notes:

- **Service troubleshooting** often involves:
  - Checking status: `systemctl status <service>`
  - Viewing logs: `journalctl -xe`, `/var/log/messages`, or `dmesg`
  - Restarting services and watching error messages

- **Permissions Issues**:
  - Ensure scripts are executable with `chmod +x script.sh`
  - Logs must be writable by the correct user/group
  - Use `ls -l` to check and fix ownership with `chown` or `chmod`

- **Cron Job for Log Backup**:
  - Schedule tasks with `crontab -e`
  - Example:  
    ```bash
    0 2 * * * /home/user/backup_logs.sh
    ```
    Runs every day at 2:00 AM

---

##  Commands Practiced:

```bash
# Check service status
systemctl status nginx

# Restart a service
sudo systemctl restart nginx

# Check logs
journalctl -xe
cat /var/log/messages

# Check script permissions
ls -l script.sh
chmod +x script.sh

# Schedule a cron job (edit crontab)
crontab -e

# List current user's cron jobs
crontab -l
```

---
