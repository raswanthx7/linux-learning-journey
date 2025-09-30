# Backup & Recovery (Linux)

## Topics Covered
- Why Backup & Recovery is important
- tar (archive files)
- gzip / gunzip (compress files)
- rsync (sync and backup)
- Real examples
- Best practices
- Key takeaways

---

## Why Backup & Recovery is Important

Imagine you are working in a company, managing servers. Suddenly, a hard drive fails, or someone accidentally deletes configuration files. Without a proper backup, you could lose days of work or even critical system settings.

**Backup & Recovery** is about:

- **Backup:** Copying data to a safe location.  
- **Recovery:** Restoring data from the backup when needed.

In Linux, this is often done with tools like `tar`, `gzip`, `gunzip`, and `rsync`.

---

## 1. tar – Tape Archive

**What it is:**  
`tar` is a command used to archive multiple files/folders into a single file (called a tarball).  
Archiving = collecting files into one file (does not compress by default).

**Why it’s used:**  
Easier to manage a single file instead of many small files.  
Commonly used before compressing data.

**Basic Syntax:**

```bash
tar [options] archive_name.tar files_or_folders
```
**Important Options:**

| Option | Meaning |
|--------|--------|
| c      | Create a new archive |
| v      | Verbose (show files being added) |
| f      | Specify filename for the archive |
| x      | Extract archive |
| t      | List contents of archive |

**Examples:**

- Create archive of folder /etc/config
```bash
tar cvf config_backup.tar /etc/config
```
`c = create`, `v = verbose`, `f = filename`
Output: Shows all files added to archive.

- Extract archive
```bash
tar xvf config_backup.tar
```
- List contents without extracting
```bash
tar tvf config_backup.tar
```

---

## 2. gzip / gunzip – Compression Tools

**What it is:**

- `gzip` compresses a file to reduce its size.
- `gunzip` decompresses it back.

Why it’s used:
Backup files can be very large. Compressing saves disk space.

**Examples:**

- Compress a tar archive
```bash
gzip config_backup.tar
```

Creates `config_backup.tar.gz` (compressed file).
Original `config_backup.tar` is replaced by the compressed version.

- Decompress a tar.gz file
```bash
gunzip config_backup.tar.gz
```

Restores `config_backup.tar`.

**Tip: Combine tar and gzip in one command:**
```bash
tar cvzf config_backup.tar.gz /etc/config
```

`z = gzip compress while creating archive`

Extract with:
```bash
tar xvzf config_backup.tar.gz
```

---

## 3. rsync – Remote & Local Backup Tool

**What it is:**
`rsync` copies files efficiently between local or remote systems.
Only copies differences, saving time and bandwidth.

**Why it’s used:**

- Keep backup updated automatically.
- Sync folders across servers.

**Basic Syntax:**
```bash
rsync [options] source destination
```

**Important Options:**

| Option   | Meaning |
|----------|---------|
| -a       | Archive mode (preserves permissions, timestamps, etc.) |
| -v       | Verbose |
| -z       | Compress during transfer |
| --delete | Delete files in destination that are removed from source |


**Examples:**

- Backup local folder
```bash
rsync -av /var/log /backup/logs
```

Copies `/var/log` to `/backup/logs` with permissions preserved.

- Backup to remote server
```bash
rsync -avz /var/log user@192.168.1.100:/backup/logs
```

`z = compress during transfer`
Efficient for large files.

---

## 4. Real Examples of Backup & Recovery

**A. Backup Logs Folder**
```bash
tar cvzf logs_backup_$(date +%F).tar.gz /var/log
```

- `$(date +%F)` → automatically adds current date: `logs_backup_2025-09-29.tar.gz`
- Makes a timestamped backup of logs.

**B. Backup Config Folder**
```bash
rsync -av /etc /backup/etc_backup
```

- Preserves permissions and structure.
- Can run daily via cron for automated backup.

---

## 5. Compress & Archive Files – Best Practices

- Use `tar + gzip` for archiving + compressing together.
- Use `rsync` for incremental backups or remote backups.
- Always store backups in a separate drive or server.
- Automate backup using cron:

Example cron job to backup `/etc` daily at 2 AM:
```bash
0 2 * * * rsync -av /etc /backup/etc_backup
```

---

## Key Takeaways

- `tar` → Archive multiple files into one.
- `gzip / gunzip` → Compress/decompress files.
- `rsync` → Sync files/folders efficiently.
- Always backup important folders like `/etc`, `/var/log`, `/home`.
- Automate backups for reliability.

---
