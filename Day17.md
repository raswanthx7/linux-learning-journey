# Day 17 – System & Process Monitoring

## Topics Covered
- System Info Commands: `top`, `htop`, `uptime`, `free`, `vmstat`
- Process Control Commands: `ps`, `kill`, `kill -9`, `jobs`, `bg`, `fg`, `nice`, `renice`
- Zombie Process Explanation

---

##  Key Notes

###  System Info Commands
- `top` – Real-time display of running processes and resource usage.
- `htop` – Enhanced version of `top` (colorful and interactive; may require installation).
- `uptime` – Shows how long the system has been running, plus load average.
- `free -h` – Displays memory usage in a human-readable format.
- `vmstat` – Reports memory, CPU, I/O, and system performance.

###  Process Control Commands
- `ps aux` – Shows all processes running on the system.
- `kill <PID>` – Gracefully terminates the process with the given PID.
- `kill -9 <PID>` – Forcefully terminates the process (cannot be blocked).
- `jobs` – Lists background jobs in the current shell.
- `bg` – Resumes a job in the background.
- `fg` – Brings a background job to the foreground.
- `nice` – Starts a process with a specific priority (default is 0).
- `renice` – Changes the priority of a running process.

###  Zombie Process
- A zombie process has finished executing but still has an entry in the process table.
- It occurs when the parent process hasn't read the child's exit status.
- Use `ps aux | grep Z` to identify zombie processes.

---

##  Commands Practiced

```bash
top                    # Display running processes
htop                   # Interactive process viewer (if installed)
uptime                 # Show system uptime and load average
free -h                # Show memory usage
vmstat                 # Show system performance stats

ps aux                 # Show all processes
kill 1234              # Terminate process with PID 1234
kill -9 1234           # Force kill process with PID 1234
jobs                   # List background jobs
bg %1                  # Resume job 1 in background
fg %1                  # Bring job 1 to foreground

nice -n 10 sleep 100   # Start sleep with lower priority
renice 5 1234          # Change priority of process with PID 1234

ps aux | grep Z        # List zombie processes
```

---
