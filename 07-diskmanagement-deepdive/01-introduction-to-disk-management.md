# 01 - Introduction to Disks

##  Topics Covered
- What is a Disk (Physical vs Virtual)
- Sectors and Blocks 
- Disk naming in Linux (`/dev/sd*`, `/dev/nvme*`, etc.)  
- Commands to check disks and their types (`lsblk`, `fdisk -l`, `parted -l`, `blkid`)  
- Storage types summary (HDD vs SSD and common interfaces) 


---

## 1. What is a Disk?

A **disk** is a storage device used to hold data permanently.  
There are two kinds of disks used in Linux systems:
- **Physical disks:** Real hardware drives like HDDs or SSDs connected to your machine.  
- **Virtual disks:** Created inside virtual environments (like VMware or VirtualBox) to simulate physical storage.

In Linux, every disk is treated as a **block device**, meaning data is transferred in blocks (chunks) instead of individual bytes.

---

## 2. Sectors and Blocks

- Every disk is divided into small units called **sectors**.
- Each sector typically holds **512 bytes** (some use 4096 bytes, called “4K sectors”).
- A modern disk can have **billions of sectors**.
- The file system groups multiple sectors into **blocks** (e.g., 1 block =  8 sectors).

**Example:**
1 sector = 512 bytes
1 block = 4096 bytes = 8 sectors

- **How many sectors on a disk?** depends on disk size:
  - Formula: `number_of_sectors = disk_size_in_bytes / sector_size`
  - Examples:
    - 1 GiB = 1,073,741,824 bytes → sectors = 1,073,741,824 / 512 = **2,097,152 sectors**
    - 2 GiB = 2,147,483,648 bytes → sectors = 2,147,483,648 / 512 = **4,194,304 sectors**
- **Why blocks exist**: the filesystem groups many sectors into blocks to manage allocation and metadata more efficiently. Blocks are the filesystem’s “allocation unit.”

---

## 3. Disk naming in Linux 
- **SCSI / SATA / USB block devices**: `/dev/sda`, `/dev/sdb`, `/dev/sdc` …  
  - Partitions: `/dev/sda1`, `/dev/sda2`, ...
- **NVMe devices**: `/dev/nvme0n1`, `/dev/nvme0n1p1` (controller `nvme0`, namespace `n1`, partition `p1`)  
- **Virtio block in VMs**: `/dev/vda`, `/dev/vdb` …  
- **LVM logical volumes**: `/dev/<vgname>/<lvname>` (example: `/dev/myvg/lv_data`)  
- **Why naming matters**: device names can change when disks are added/removed; use UUIDs or LABELs in `/etc/fstab` for stable mounts.

---


## 4. Commands to check disks and their types (quick reference)
- `lsblk` — list block devices in a tree (shows NAME, SIZE, TYPE, MOUNTPOINT)  
  - `lsblk -d -o NAME,ROTA,TRAN,SIZE` — shows transport (`TRAN` like sata/nvme) and rotational flag (`ROTA` 1=HDD, 0=SSD).

| **Column**     | **Meaning**                | **Example**               | **Explanation**                                                                 |
|----------------|----------------------------|---------------------------|---------------------------------------------------------------------------------|
| **NAME**       | Device name                | `nvme0n1`, `nvme0n1p1`    | The name Linux gives to each storage device and partition.<br>• `nvme0n1` = Whole NVMe disk.<br>• `nvme0n1p1` = Partition 1 of that disk. |
| **MAJ:MIN**    | Major & minor device numbers | `259:0`                   | Internal kernel identifiers used to track devices.<br>Not used in commands — handled by Linux’s device manager. |
| **RM**         | Removable flag             | `0`                       | `0` = Not removable (fixed disk).<br>`1` = Removable (e.g. USB, SD card).      |
| **SIZE**       | Disk/Partition size        | `8G`, `1M`, `10M`         | Displays total capacity of each disk or partition.                            |
| **RO**         | Read-only flag             | `0`                       | `0` = Read/Write.<br>`1` = Read-only (cannot write data).                      |
| **TYPE**       | Type of device             | `disk`, `part`, `lvm`     | Indicates whether it’s a full disk (`disk`), a partition (`part`), or a logical volume (`lvm`). |
| **MOUNTPOINTS**| Mounted path               | `/`, `/boot/efi`          | Shows where the partition is attached (mounted) in the Linux filesystem tree.  |

- `fdisk -l` — show partition tables (disklabel type: gpt/msdos), sectors, start/end.  
- `parted /dev/sdX print` or `parted -l` — detailed GPT/MBR info; `print free` shows unallocated space.  
- `blkid` — show UUIDs, LABELs and filesystem types.  
- `df -h` - Display mounted file system and usage
- `du -sh` - Checks directory-level space usage

---

## 5. Storage types summary (HDD vs SSD and common interfaces)
- **HDD (Hard Disk Drive)**  
  - Mechanical storage (spinning platters). Typical interfaces: **SATA**, **SAS**.  
  - Slower, cheaper, higher capacity. Rotational = `ROTA=1`.
- **SSD (Solid State Drive)**  
  - Flash memory (no moving parts). Interfaces: **SATA**, **SAS**, **NVMe (PCIe)**.  
  - Faster and lower latency. Rotational = `ROTA=0`.
  - **NVMe** is a protocol over **PCIe** used **only** by SSDs — NVMe devices appear as `/dev/nvme*`.
- **Key rule**: you can have SATA SSDs and NVMe SSDs; HDDs normally use SATA/SAS (HDDs are not NVMe).

---

## Final practical notes 
- **Always check** device names with `lsblk` or `fdisk -l` before partitioning or formatting.  
- **Partitions usually start at sector 2048** to meet alignment standards — do not assume partitions start at sector 63 (old legacy).  
- **Use UUIDs or LABELs** in `/etc/fstab` for stable mounting across reboots and disk reordering.  
- If you need to compute sectors or check disk size: use `fdisk -l` (it prints sectors, sector size, and total sectors).

---
