#  MBR Disk Layout in Linux (From 0 Bytes Onward)

##  Topics Covered
- Introduction  
- Why the first 1 MB is reserved  
- MBR disk layout (sector-by-sector)  
- Visual representation  
- Detailed explanation of MBR structure  
- Bootloader extension area  
- Usable data region  
- MBR limitations   
- Key takeaways  

---

## 1. Introduction
When a new disk is initialized with **MBR (Master Boot Record)**, Linux divides the first part of the disk into small, fixed regions that store boot code and the partition table.

MBR is the older partitioning standard, used before GPT.  
It supports a maximum of **2 TB per disk** and only **four primary partitions**.  
Despite its limitations, MBR is still common in older BIOS-based systems.

---

## 2. Why the First 1 MB Is Reserved
Even though the **MBR structure** itself only needs the **first 512 bytes**, modern partition tools (like `fdisk` and `parted`) still reserve **1 MB** of space before the first partition.  
This ensures **alignment** for SSDs and compatibility with GPT and newer drives.

**Reasons for 1 MB reservation:**
- Keeps partitions aligned on 1 MB boundaries (sector 2048).
- Avoids overlapping with bootloader code that might extend past the first sector.
- Ensures better performance on 4K-sector disks.

So, although MBR technically needs only one sector, modern Linux tools still start partitions from **sector 2048** (same as GPT).

---

## 3. MBR Layout (Sector-by-Sector)

| Sector Range | Description |
|---------------|--------------|
| **Sector 0 (First 512 bytes)** | **Master Boot Record (MBR)** — contains bootloader code, partition table (4 entries), and a 2-byte signature. |
| **Sectors 1–2047** | **Reserved / Alignment Gap** — reserved for bootloader extensions (like GRUB) or alignment purposes. |
| **Sector 2048 → ...** | **Usable Data Region** — partitions and filesystems begin here (e.g., `/dev/sda1`). |

---

## 4. Visual Layout

```text
| Sector 0 | Sectors 1–2047 | Sector 2048 → ... |
+-----------+----------------+-------------------+
| MBR       | Bootloader     | Data Partitions   |
| (Code +   | Extension or   | start here        |
| Table)    | Alignment Gap  |                   |
+-----------+----------------+-------------------+
```

---

## 5. Detailed Explanation

### Master Boot Record (Sector 0)
- **Size:** 512 bytes  
- **Divided into three main parts:**
  1. **Bootloader Code (446 bytes)**  
     - The small program that runs first when the system boots.  
     - It loads a larger bootloader like GRUB or directly loads an OS.
  2. **Partition Table (64 bytes)**  
     - Contains **4 primary partition entries**.  
     - Each entry describes:
       - Partition type (e.g., Linux, NTFS, FAT)
       - Start sector
       - End sector
       - Boot flag
  3. **Boot Signature (2 bytes)**  
     - Always `0x55AA` — identifies the sector as a valid MBR.

---

## 6. Bootloader Extensions (Sectors 1–2047)
- Many bootloaders, such as **GRUB**, need more space than the 446 bytes available in the MBR.
- These sectors (1–2047) are kept empty to store additional bootloader code (called the **core image**).
- If not used by bootloaders, they remain unallocated for alignment.

---

## 7. Usable Data Region (From Sector 2048)
- Actual partitions and file systems begin at **sector 2048**.
- For example, `/dev/sda1` might start here.
- File systems like ext4, XFS, or NTFS are created in this region.

Example output using `fdisk`:
```bash
sudo fdisk -l /dev/sda
```
Example:
```bash
Device     Boot Start     End Sectors Size Type
/dev/sda1  *    2048  1050623 1048576 512M Linux filesystem
```
Here, **Start = 2048** confirms 1 MB alignment.

---

## 8. Limitations of MBR

- Maximum disk size: 2 TB
- Maximum partitions: 4 primary
- If more are needed, one partition must be marked as extended, containing logical partitions.
- No built-in redundancy — if the first sector gets corrupted, the partition table is lost.

---

## 9. Key Takeaways

- MBR occupies only 512 bytes, but tools reserve 1 MB for safety and alignment.
- The MBR structure includes:
   - Bootloader (446 bytes)
   - Partition table (64 bytes)
   - Boot signature (2 bytes)
- Supports 4 primary partitions maximum.
- Has no backup or redundancy — GPT is preferred for modern systems.
- The first partition starts at sector 2048 (1 MB) for optimal performance.

---
