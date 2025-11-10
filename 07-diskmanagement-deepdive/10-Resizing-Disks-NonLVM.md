#  Resizing Disks (Non-LVM)

## Topics Covered
- Introduction
- Why Resizing Is Needed
- How Non-LVM Disk Resizing Works
- Step-by-Step: Resizing Partition Using `parted`
- Why Non-LVM Resizing Is Risky
- Next Step: Introduction to LVM
---

##  Introduction

Resizing a disk means increasing or decreasing the space allocated to a partition.  
In real-world Linux systems, this is often required when:

- A disk or partition runs out of space.  
- Storage was expanded from the virtualization or cloud console.  
- You want to extend a filesystem without reinstalling or recreating partitions.

This guide explains how to **resize a disk without using LVM**, using the **`parted` command** — the most reliable and widely used tool in companies for GPT-based systems.

---

##  Why Resizing Is Needed

In production servers or virtual machines, disks often get filled over time.  
Instead of migrating data or attaching a new disk separately, resizing helps you use the **same disk** but with **expanded capacity**.

Example scenario:
- Your `/dev/sdb` was initially **10GB**.  
- You extended it to **20GB** in your virtualization settings or cloud console.  
- Now, Linux detects the 20GB disk but the partition still shows 10GB.  
- You must **resize the partition** and then **extend the filesystem** to use the new space.

---

##  How Non-LVM Disk Resizing Works

In a traditional setup, partitions are **fixed in size** and **tightly bound** to the disk structure.  
So to resize:
1. You expand the virtual disk.  
2. You use a tool like `parted` to extend the existing partition boundary.  
3. You tell the OS to reload the partition table.  
4. Finally, you resize the filesystem to use the newly available space.

---

##  Step-by-Step: Resizing Partition Using `parted`


###  Step 1: Check Disk and Partition Info

Use `lsblk` to see disks and their partitions:

```bash
lsblk
```
Then verify size details with:
```bash
sudo parted /dev/sdb print
```

Output example:
```bash
Model: VMware Virtual disk (scsi)
Disk /dev/sdb: 21.5GB
Partition Table: gpt
Number  Start   End     Size    File system  Name  Flags
 1      1049kB  10.7GB  10.7GB  ext4
```

### Step 2: Resize the Partition Boundary
Run `parted` in interactive mode:

```bash
sudo parted /dev/sdb
```

Inside the parted shell:
```bash
(parted) print
(parted) resizepart 1 100%
```

This command tells parted:

> “Resize partition number 1 to use 100% of the available disk space.”

Then quit:
```bash
(parted) quit
```

### Step 3: Re-read Partition Table
Force Linux to reload the new partition layout without rebooting:

```bash
sudo partprobe /dev/sdb
```

or
```bash
sudo partx -u /dev/sdb
```

### Step 4: Resize the Filesystem
Now that the partition boundary is extended, you need to grow the filesystem to use that new space.

For **EXT4 filesystem:**
```bash
sudo resize2fs /dev/sdb1
```

For XFS filesystem:
```bash
sudo xfs_growfs /mount/point
```

### Step 5: Verify the Change
Check the new size using:

```bash
df -h
```
Example output:
```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb1        20G   3G   17G  15% /mnt/data
```
 The filesystem now uses the full expanded space.

---

## Why Non-LVM Resizing Is Risky

While it works, non-LVM resizing has major downsides:

- ❌ Requires manual partition editing.
- ❌ Risk of mistakes leading to boot or mount issues.
- ❌ Must unmount in some cases, causing downtime.
- ❌ Limited flexibility for future expansion.

Because of these limitations, modern systems prefer LVM (Logical Volume Manager).

---

## Next Step: Introduction to LVM
LVM removes all these manual steps.
With LVM, you can:

- Combine multiple disks into one storage pool.
- Resize or extend volumes online.
- Add new disks without touching partition boundaries.
- Grow filesystems dynamically with zero downtime.

Continue to: `11_LVM_Basics_And_Disk_Expansion.md

---
