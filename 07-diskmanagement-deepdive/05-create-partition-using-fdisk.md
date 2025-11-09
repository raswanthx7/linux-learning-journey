#  Creating Disk Partitions Using `fdisk` in Linux

##  Topics Covered
- What is a disk partition
- Why partitions are created
- Understanding partition tables (MBR/GPT)
- Identifying disks before partitioning
- Creating partitions using `fdisk`
- Formatting and mounting the new partition
- Useful commands (`lsblk`, `df -h`, `du -sh`, etc.)
- Real-world examples
- Key takeaways

---

## 1. Introduction – What is a Partition?

A **partition** is a logical division of a physical disk.  
It allows the disk to be split into independent sections, each acting like a separate storage unit.

Example:  
A 500 GB disk can be divided into:
- 100 GB for `/` (root filesystem)
- 100 GB for `/home`
- 50 GB for `/var`
- 250 GB for backups

Each partition can hold its own **file system**, helping in data organization, system recovery, and performance optimization.

---

## 2. Why We Create Partitions
- **Organization:** Separate system files, user data, and logs.  
- **Security:** Prevent full disk errors (one partition full doesn’t crash the OS).  
- **Performance:** File systems can be optimized per use case.  
- **Backup & Recovery:** Easier to restore specific partitions.

---

## 3. Identify Disks Before Partitioning
Before creating any partition, you must list all available disks.

### Command: `lsblk`
```bash
lsblk
```
Displays block devices, mount points, and hierarchy.

### Command: `sudo fdisk -l`
```bash
sudo fdisk -l
```
Shows detailed disk info, including sector sizes and partition tables.

### Command: `df -h`
```bash
df -h
```
Shows mounted file systems and used/free space in human-readable format.

### Command: `du -sh /path`
```bash
du -sh /home
```
Displays the disk usage of a specific directory.

---

## 4. Creating a Partition Using `fdisk`

### Step 1 – Open the Disk
```bash
sudo fdisk /dev/sdb
```
(`/dev/sdb` is the target disk — change as per your setup.)

### Step 2 – Verify Current Partitions
Inside `fdisk`:
```
Command (m for help): p
```

### Step 3 – Create a New Partition
```
Command (m for help): n
```
You’ll be asked for:
- Partition type (Primary/Extended)
- Partition number (1–4)
- First sector (default: 2048)
- Last sector or size (`+2G` for 2GB, etc.)

### Step 4 – Write the Changes
```
Command (m for help): w
```
Writes changes to disk and exits.

---

## 5. Formatting the New Partition
After creation, format it with a file system:
```bash
sudo mkfs.ext4 /dev/sdb1
```

Check the result:
```bash
lsblk -f
```

---

## 6. Mounting the Partition
Create a directory and mount it:
```bash
sudo mkdir /mnt/newdisk
sudo mount /dev/sdb1 /mnt/newdisk
```

Verify:
```bash
df -h
```

---

## 7. Real-World Example
Creating and mounting a 5 GB partition for testing:
```bash
sudo fdisk /dev/xvdf
# (Create partition as shown above)
sudo mkfs.ext4 /dev/xvdf1
sudo mkdir /data
sudo mount /dev/xvdf1 /data
```

Check:
```bash
lsblk
```

---

## 8. Key Takeaways
- Always check the disk using `lsblk` before partitioning.  
- `fdisk` works well for **MBR** and smaller disks (<2TB).  
- For **GPT** or larger disks, use `parted`.  
- Always unmount before formatting or deleting partitions.  
- Use `df -h` and `du -sh` to monitor disk usage effectively.

---

