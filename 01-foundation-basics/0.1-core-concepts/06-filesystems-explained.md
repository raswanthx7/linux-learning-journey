#  06 - Filesystems Explained

## Topics covered

- Understanding Linux File Systems
- What is a File System?
- Why Are File Systems Needed?
- Common Linux File Systems
- FAT32 and EFI System Partition (ESP)
- ext4 (Default Linux File System)
- Journaling Explained
- Where File Systems Are Used (Real-world Examples)
- Checking File System in Linux


## Understanding Linux File Systems

File systems define **how data is stored, accessed, and managed** on a storage device like HDDs or SSDs.  
Each file system has a specific structure, method for handling data blocks, metadata, and permissions.

---

##  1. What is a File System?

A **file system** is a method that tells the operating system *how to organize and retrieve data* on a storage device.  
When you save a file, the file system decides:
- Where the data blocks are stored
- How the file name, permissions, and metadata are recorded
- How the OS retrieves and manages the data efficiently

---

##  2. Why Are File Systems Needed?

Without a file system, data would just be a **continuous stream of bits** on a disk — the OS wouldn’t know where one file ends and another begins.  
File systems provide:
- **Structure:** Organizes files and directories  
- **Access Control:** Manages permissions  
- **Efficiency:** Handles fragmentation and free space  
- **Reliability:** Detects and recovers from disk errors  

---

##  3. Common Linux File Systems

| File System | Description | Used For |
|--------------|--------------|-----------|
| **ext2** | Second Extended File System. Old but simple. | Legacy Linux systems |
| **ext3** | Adds journaling to ext2 (faster recovery after crash). | Older distros |
| **ext4** | Fourth Extended File System. Default in most modern distros. Supports large files. | Linux root & data partitions |
| **XFS** | High-performance journaling file system by SGI. | Servers, large-scale storage |
| **Btrfs** | Advanced features like snapshots, compression, and checksums. | Modern Linux systems |
| **FAT32** | Old Microsoft file system. Used for boot or EFI partitions. | EFI/UEFI partition (ESP) |
| **NTFS** | Windows file system (read/write supported on Linux via drivers). | Shared drives with Windows |
| **swap** | Not a true file system — used for virtual memory. | Swap partition or swap file |

---

##  4. FAT32 and EFI System Partition (ESP)

- **FAT32** = File Allocation Table 32-bit  
  - “32” refers to the **32-bit address space** used to reference file clusters.  
  - Supported by all operating systems (Windows, Linux, macOS).  
  - Doesn’t support large files (>4GB) or Unix-style permissions.

- **EFI System Partition (ESP):**
  - A small partition (~100–500 MB) formatted with FAT32.
  - Used in **UEFI-based systems**.
  - Contains:
    - Bootloaders (e.g., `grubx64.efi`)
    - Firmware drivers
    - System utilities
  - Example path in Linux: `/boot/efi`

---

##  5. ext4 (Default Linux File System)

- Modern, fast, and reliable  
- Supports journaling (tracks uncommitted changes for crash recovery)  
- Handles large volumes and file sizes  
- Efficient allocation algorithms (delayed allocation, extents)  
- Supports backward compatibility with ext2 and ext3  

### Example Mount Points:

| Mount Point | File System | Description |
|--------------|--------------|-------------|
| `/boot` | ext4 | Stores kernel and initramfs |
| `/boot/efi` | FAT32 | Stores EFI bootloaders |
| `/` | ext4 | Root filesystem (contains all system files) |

---

##  6. Journaling Explained

**Journaling** means the file system keeps a log of changes before applying them.  
If a crash occurs, it replays the journal to recover the file system.

- **ext3/ext4**, **XFS**, and **Btrfs** support journaling.  
- **FAT32** does **not**, which is why it’s used only for boot partitions, not system storage.

---

##  7. Where File Systems Are Used (Real-world Examples)

| Partition | File System | Purpose |
|------------|--------------|----------|
| `/boot/efi` | FAT32 | Stores UEFI bootloader files |
| `/boot` | ext4 | Contains kernel and initramfs |
| `/` (root) | ext4 / XFS | Main operating system |
| `/home` | ext4 / Btrfs | User files and data |
| `/var` | ext4 | Logs, cache, variable data |
| `/swap` | swap | Virtual memory space |

---

##  8. Checking File System in Linux

```bash
# View partitions and their file systems
lsblk -f

# Show mounted file systems
df -Th

# Identify file system type of a specific partition
blkid /dev/sda1
```

---

9. Summary

- **UEFI systems** → use **GPT partition style** and **FAT32** for the bootloader (ESP).
- **Linux root partitions** → use **ext4**, **XFS**, or **Btrfs**.
- **File systems decide** how data is stored, organized, and retrieved efficiently.
- **FAT32** = for compatibility and boot
- **ext4** = for performance and reliability

---

