#  Day 16 – Disk & Filesystem

---

##  Topics Covered

- View disk & mount info: `df`, `du`, `mount`, `umount`, `lsblk`, `blkid`
- Create mount point
- Check space usage
- Disk formatting basics

---

##  Key Notes

- **df (disk free)**: Shows used and available disk space of file systems.
- **du (disk usage)**: Shows space used by specific directories or files.
- **mount / umount**: Used to attach/detach devices (like external drives) to the file system.
- **lsblk**: Lists block devices (disks, partitions).
- **blkid**: Displays UUID and filesystem type of partitions.
- **Mount point**: A directory where an external disk or partition is made accessible.
- Disk formatting is usually done before first use of a new disk, using tools like `mkfs`.

---

##  Commands Practiced

```bash
# View disk usage
df -h                      # Human-readable disk space
du -sh /home               # Disk usage of /home directory

# List devices
lsblk                      # Show block devices
blkid                      # Show UUID and FS type of partitions

# Mount and unmount
sudo mount /dev/sdb1 /mnt/mydrive     # Mount a device
sudo umount /mnt/mydrive              # Unmount the device

# Create mount point
sudo mkdir /mnt/mydrive               # Create a mount directory

# Optional: format a disk (careful!)
sudo mkfs.ext4 /dev/sdb1              # Format with ext4 file system
```

___
