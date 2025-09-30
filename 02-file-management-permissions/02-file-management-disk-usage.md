# File Management & Disk Usage

## 🔹 Topics Covered:

- File Types: regular, directory, device, symlink, etc.
- File Extensions: `.sh`, `.log`, `.conf`, etc.
- File Operations: `cp`, `mv`, `rm`, `cp -r`, `mv -f`, `rm -rf`
- Disk Usage Tools: `df -hT`, `du -sh`, `find -size`, `find -mtime`
- Real-world task: copying logs, backups, config files

---

## File Types:

- **Regular file**: Normal file with data or code. (`-`)
- **Directory**: Folder that holds files or subfolders. (`d`)
- **Symbolic Link**: Shortcut/reference to another file. (`l`)
- **Character device**: For input/output devices (e.g., keyboard). (`c`)
- **Block device**: For storage devices (e.g., hard disks). (`b`)

 Check type using:

```bash
ls -l       # File type indicated by first character
file <filename>
```

## File Extensions:

- `.sh` – Shell script 
- `.log` – Log files 
- `.conf` – Configuration files 
- `.txt` – Plain text 

## File Operations

```bash
cp file1 file2              # Copy file
mv file1 /path/             # Move file to another directory
rm file1                    # Delete file

cp -r dir1 dir2             # Copy folder recursively
mv -f file1 file2           # Force move (no prompt)
rm -rf folder/              # Force delete folder recursively
```

## Disk Usage Commands:

```bash
df -hT                     # Shows disk free space with filesystem type
du -sh folder/             # Size of folder
find . -size +10M          # Find files larger than 10MB
find . -mtime -7           # Files modified in last 7 days
```
---
