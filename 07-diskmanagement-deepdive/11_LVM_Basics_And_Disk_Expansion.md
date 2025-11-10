# 11 - LVM Basics and Disk Expansion

## Topics Covered
- Introduction
- What Is LVM?
- LVM Architecture
- Real-World Analogy
- Step-by-Step: Creating and Expanding Storage Using LVM
- Expanding LVM Disk (Add New Disk)
- Verification Commands
- Why Companies Use LVM
- Summary Table

---

##  Introduction
In traditional disk setups, resizing or expanding storage is complicated. You have to manually extend partitions, resize filesystems, and often reboot the system.  
To solve this problem, Linux introduced **LVM (Logical Volume Manager)** — a smarter storage management layer that gives you **flexibility to resize, expand, and manage storage without downtime**.

---

##  What Is LVM?
LVM is a system that lets you manage disks in a flexible way.  
Instead of fixed partitions, you get **logical volumes** that can grow or shrink dynamically.

**Example use case:**
- You can easily add new disks and expand your storage pool.
- You can extend existing filesystems online (without unmounting).
- You can take snapshots for backup.

---

##  LVM Architecture

LVM has 3 main building blocks:

| Component | Description | Example Command |
|------------|--------------|----------------|
| **PV (Physical Volume)** | The raw disk or partition that LVM will manage. Think of it as making a disk “LVM-ready.” | `pvcreate /dev/sdb` |
| **VG (Volume Group)** | A storage pool combining one or more physical volumes. All future logical volumes are created from this pool. | `vgcreate myvg /dev/sdb` |
| **LV (Logical Volume)** | The virtual partition carved from the VG. You can format and mount this like a normal partition. | `lvcreate -n mylv -L 10G myvg` |

---

##  Real-World Analogy

| LVM Concept | Real World Example |
|--------------|--------------------|
| PV | Water tanks |
| VG | Big water reservoir (all tanks combined) |
| LV | Water pipes that take water from the reservoir to your house |

When you need more water (storage), you just add another tank (PV). The reservoir (VG) grows, and the pipes (LV) can now supply more water.

---

##  Step-by-Step: Creating and Expanding Storage Using LVM

### 1. Create Physical Volume (PV)
Make the disk or partition ready for LVM.
```bash
sudo pvcreate /dev/sdb
```
Check PV status:
```bash
sudo pvs
```

### 2. Create Volume Group (VG)
Combine one or more physical volumes into a group.
```bash
sudo vgcreate myvg /dev/sdb
```
Verify:
```bash
sudo vgs
```

### 3. Create Logical Volume (LV)
Allocate space from the VG to create a usable logical volume.
```bash
sudo lvcreate -n mylv -L 10G myvg
```
Check:
```bash
sudo lvs
```

### 4. Create Filesystem and Mount
Format the LV and mount it for use.
```bash
sudo mkfs.ext4 /dev/myvg/mylv
sudo mkdir /mnt/mydata
sudo mount /dev/myvg/mylv /mnt/mydata
```

Check mount:
```bash
df -h
```

---

##  Expanding LVM Disk (Add New Disk)

Let’s say your current LV is full, and you’ve attached a new disk `/dev/sdc`.

### Step 1: Create a Physical Volume on the new disk
```bash
sudo pvcreate /dev/sdc
```

### Step 2: Extend the Volume Group
```bash
sudo vgextend myvg /dev/sdc
```

### Step 3: Extend the Logical Volume
Use all the free space available in VG:
```bash
sudo lvextend -l +100%FREE /dev/myvg/mylv
```

### Step 4: Resize the Filesystem
```bash
sudo resize2fs /dev/myvg/mylv
```

Now your logical volume and filesystem have grown successfully.

---

##  Verification Commands

| Command | Description |
|----------|--------------|
| `lsblk` | Lists all block devices (disks, partitions, LVs) |
| `df -h` | Displays current mounted filesystem usage |
| `sudo pvs` | Lists all physical volumes |
| `sudo vgs` | Lists all volume groups |
| `sudo lvs` | Lists all logical volumes |
| `blkid` | Shows UUIDs and filesystem info |

---

##  Why Companies Use LVM

- Resize storage **without downtime**
- Combine multiple disks into a single volume
- Take **snapshots** for backups and testing
- Migrate data easily between disks
- Perfect for servers and cloud environments (AWS, Azure, VMware)

---

##  Note
Non-LVM resizing requires reboots, careful partition handling, and risk of data loss.  
LVM completely removes those headaches and gives you **dynamic and safe storage management**.

---

##  Summary Table

| Task | Command |
|------|----------|
| Create PV | `pvcreate /dev/sdb` |
| Create VG | `vgcreate myvg /dev/sdb` |
| Create LV | `lvcreate -n mylv -L 10G myvg` |
| Extend VG | `vgextend myvg /dev/sdc` |
| Extend LV | `lvextend -l +100%FREE /dev/myvg/mylv` |
| Resize FS | `resize2fs /dev/myvg/mylv` |

---

 **Conclusion:**
LVM gives Linux admins and cloud engineers complete control over their disk space.  
It makes expanding storage simple, live, and risk-free — which is why almost all companies and servers use LVM instead of traditional static partitioning.

---

