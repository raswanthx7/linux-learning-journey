#  Links & File Metadata

---

##  Topics Covered

- Hard link vs Soft link: `ln`, `ln -s`
- Use cases: backups, shared configs
- File metadata: inode, size, timestamps
- Extra commands: `basename`, `dirname`

---

##  Key Notes

- **Hard Link**:
  - Created with `ln`
  - Points directly to the **same inode** (same file)
  - Changes to original file reflect in hard link
  - If original file is deleted, the hard link **still works**

- **Soft Link (Symbolic Link)**:
  - Created with `ln -s`
  - Acts like a **shortcut**, points to the original file **by path**
  - Breaks if the original file is deleted
  - Can link to directories too

- **Use Cases**:
  - Backups without duplication (hard links)
  - Shared config pointing to original (soft links)
  - Shortcuts to long paths

- **File Metadata**:
  - **Inode**: unique number assigned to each file on disk
  - **Timestamps**:
    - `ctime` – change time (metadata)
    - `mtime` – modification time (content)
    - `atime` – access time (last read)
  - Use `stat` to view full metadata

- **Extra Commands**:
  - `basename /path/to/file.txt` - shows `file.txt`
  - `dirname /path/to/file.txt` - shows `/path/to`

---

##  Commands Practiced

```bash
# Create a hard link
ln original.txt hardlink.txt

# Create a soft (symbolic) link
ln -s original.txt softlink.txt

# Check inode numbers
ls -li original.txt hardlink.txt softlink.txt

# View full metadata
stat original.txt

# Check access and modification time
ls -lu original.txt   # Access time
ls -l original.txt    # Modification time

# Get basename and dirname
basename /home/user/file.txt     # Output: file.txt
dirname /home/user/file.txt      # Output: /home/user
```

---
