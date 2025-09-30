#  Cron Jobs & Scheduling

##  Topics Covered
- Scheduled tasks: `cron`, `crontab`, `at`, `sleep`, `watch`
- Setup recurring jobs using crontab
- System vs user crontabs

---

##  Key Notes

- **cron** is used to schedule recurring tasks in Linux.
- **crontab** (cron table) holds the list of scheduled jobs for a user.
- **at** is used for one-time scheduling.
- **sleep** delays a command by seconds.
- **watch** runs a command repeatedly at fixed intervals.

 
 **Cron Job Time Format:**

```
* * * * *  command_to_run
│ │ │ │ │
│ │ │ │ └── Day of week (0 - 7) (Sunday = 0 or 7)
│ │ │ └──── Month (1 - 12)
│ │ └────── Day of month (1 - 31)
│ └──────── Hour (0 - 23)
└────────── Minute (0 - 59)
```

 Example:
```bash
0 5 * * * /home/user/backup.sh
# This runs backup.sh every day at 5:00 AM
```
##  Commands Practiced

```bash
crontab -e               # Edit user's cron jobs
crontab -l               # List user's cron jobs
crontab -r               # Remove all user's cron jobs

at now + 2 minutes       # Schedule a one-time command
atq                      # View at jobs
atrm <job_id>            # Remove at job

sleep 10                 # Pause command execution for 10 seconds

watch df -h              # Watch disk space every 2 seconds
watch -n 5 free -m       # Watch memory usage every 5 seconds
```

---
