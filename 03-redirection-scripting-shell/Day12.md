#  Day 12 – Search Tools

---

##  Topics Covered

- Search files: `find`, `locate`, `which`, `whereis`
- Find with conditions: `-name`, `-type`, `-size`, `-mtime`, `-exec`
- Combine with `grep`, `xargs` for powerful searches

---

##  Key Notes

- `find`: Most powerful command to search files **recursively**
  - You can filter by name, type, time, size, permissions, etc.
  - Syntax: `find [path] [conditions]`
- `locate`: Faster than `find`, uses a prebuilt database
  - You must update the database with `updatedb`
- `which`: Shows the **full path** of executables in your `$PATH`
- `whereis`: Finds binary, source, and man pages
- `-exec`: Allows running commands on found results (e.g., delete, copy)
- You can combine `find` with `grep`, `xargs` for deep filtering and automation.

---

##  Commands Practiced

```bash
# Search using 'find'
find /etc -name "hosts"                    # Find file by name
find . -type f                             # Find all files
find . -type d                             # Find all directories
find /var/log -size +10M                   # Files larger than 10MB
find . -mtime -1                           # Modified in the last 1 day

# Using -exec to delete or copy
find . -name "*.log" -exec rm {} \;        # Delete all .log files
find . -name "*.sh" -exec chmod +x {} \;   # Make all .sh files executable

# locate command
sudo updatedb                             # Update locate DB (if needed)
locate passwd                             # Fast search by filename

# which and whereis
which bash                                # Path of bash binary
whereis bash                              # Binary + source + man page

# Combine find + grep
find /etc -type f | grep "conf"           # Find files with 'conf' in name/path

# Use xargs to run commands on found results
find . -name "*.txt" | xargs cat           # View content of all .txt files
```

---
