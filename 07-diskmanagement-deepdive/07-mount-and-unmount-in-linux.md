#  Mounting and Unmounting in Linux

##  Topics Covered
- What is mounting
- Why we need to mount a file system
- Difference between device and mount point
- What happens when we mount
- Understanding unmounting
- Commands: `mount`, `umount`, `lsblk`, `df -h`
- Step-by-step mounting process
- Real-world examples
- Common errors and troubleshooting
- Key takeaways

---

## 1. Introduction – What Is Mounting?

In Linux, **mounting** is the process of **making a storage device accessible** through the file system hierarchy.

Unlike Windows, where each partition gets a drive letter (like C: or D:),  
Linux merges all devices under **a single directory tree** starting from `/` (root).

When you mount a device, you’re attaching its file system to a **directory** (called a *mount point*).  
After mounting, files and directories inside that device become accessible through that directory path.

---

## 2. Why We Need to Mount

Linux treats **everything as a file** — including disks, partitions, and devices.  
But until a device is mounted, the system doesn’t know *where* to access it.

### Main reasons for mounting:
- To **access** data stored on disks or partitions.  
- To **make external or additional storage usable**.  
- To **attach new partitions** created using `fdisk` or `parted`.  
- To **organize file systems** under a single root structure.  
- To **temporarily attach** drives (like USBs or external volumes).

---

## 3. Mount Point – The Connection Path

A **mount point** is simply a **directory** where a file system is attached.

Example:
```bash
sudo mkdir /mnt/data
sudo mount /dev/sdb1 /mnt/data
```

Now everything inside `/dev/sdb1` becomes accessible inside `/mnt/data`.

If you `cd /mnt/data`, you’re now browsing the contents of that partition.

---

## 4. What Happens When We Mount

1. The Linux kernel reads the file system metadata (like superblock).  
2. It links the block device (e.g., `/dev/sdb1`) to the specified mount point (e.g., `/mnt/data`).  
3. The system adds this information to `/etc/mtab` or `/proc/mounts`.  
4. You can then read/write data as if it were part of the root file system.

---

## 5. Viewing Mounted Devices

### Command: `lsblk`
Lists all block devices and their mount points.
```bash
lsblk
```

### Command: `df -h`
Shows all mounted partitions with size, usage, and mount point.
```bash
df -h
```

Example:
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
/dev/sdb1        10G  1.5G  8.5G  15% /mnt/data
```

---

## 6. Mounting a Partition (Step-by-Step)

### Step 1 – Create a Mount Point
```bash
sudo mkdir /mnt/project
```

### Step 2 – Mount the Partition
```bash
sudo mount /dev/sdb1 /mnt/project
```

### Step 3 – Verify the Mount
```bash
df -h
```
or  
```bash
lsblk
```

Now `/mnt/project` gives access to the `/dev/sdb1` partition.

---

## 7. Unmounting a Partition

Unmounting removes the link between the **device** and the **mount point**.  
It ensures that all pending writes are safely completed before detaching.

### Command:
```bash
sudo umount /mnt/project
```
or
```bash
sudo umount /dev/sdb1
```

### Common Error:
```
umount: /mnt/project: target is busy
```
This means a process is still using that mount point.  
You can check it using:
```bash
lsof | grep /mnt/project
```

Then close the process and try again.

---

## 8. Mount Options

You can mount with specific options using flags:

```bash
sudo mount -t ext4 -o rw,noexec /dev/sdb1 /mnt/data
```

Common options:
| Option | Description |
|---------|--------------|
| `ro` | Mount as read-only |
| `rw` | Mount as read-write |
| `noexec` | Prevent execution of binaries |
| `nosuid` | Ignore set-user-ID bits |
| `user` | Allow normal users to mount |

---

## 9. Temporary vs Permanent Mount

- **Temporary Mount:** Created manually using `mount` command; lost after reboot.  
- **Permanent Mount:** Defined in `/etc/fstab` file; automatically mounts during boot.

Example entry in `/etc/fstab`:
```
/dev/sdb1   /mnt/data   ext4   defaults   0   2
```

---

## 10. Real-World Example

You created a new partition `/dev/sdb1` using `fdisk` and formatted it as `ext4`.  
Now you want to make it accessible permanently.

```bash
sudo mkdir /mnt/storage
sudo mount /dev/sdb1 /mnt/storage
df -h
```

To make it permanent:
1. Get UUID:
   ```bash
   sudo blkid /dev/sdb1
   ```
2. Add entry to `/etc/fstab`:
   ```
   UUID=xxxx-xxxx  /mnt/storage  ext4  defaults  0  2
   ```

---

## 11. Key Takeaways
- Mounting links a block device to the file system tree.  
- Unmounting safely detaches it to prevent data loss.  
- Use `lsblk` and `df -h` to check mounted devices.  
- Always unmount before removing external drives.  
- `/etc/fstab` makes mounts permanent and automatic.

---

