#  MBR vs GPT — Understanding Disk Partition Tables

## Topics Covered

- What Is a Partition Table?
- MBR (Master Boot Record)
- GPT (GUID Partition Table)
- Advantages of GPT over MBR
- Real-World Linux Example
- Protective MBR — Why GPT Still Has It

---

## 1. What Is a Partition Table?

A **partition table** defines how data is organized on a storage device (HDD/SSD).  
It acts like a **map of the disk**, telling the system:
- Where each partition starts and ends
- Which partition is bootable
- What file system is used

Without a partition table, the OS cannot locate files or even boot.

Two major types exist:
- **MBR (Master Boot Record)** → used by BIOS
- **GPT (GUID Partition Table)** → used by UEFI

---

## 2. MBR (Master Boot Record)

###  What It Is
- Introduced in 1983.
- Stored in the **first 512 bytes** of the disk.
- Used in **Legacy BIOS** systems.

###  Structure of MBR

```text
+----------------------------+
| Bootloader (Stage 1) | 446 bytes
+----------------------------+
| Partition Table Entries | 64 bytes
+----------------------------+
| Boot Signature (55AAh) | 2 bytes
+----------------------------+
Total = 512 bytes
```

- The **bootloader** starts the OS boot process.
- The **partition table** holds entries for up to **4 partitions**.
- The **signature (0x55AA)** tells BIOS that this is a bootable disk.

###  Limitations
- Supports disks **only up to 2 TB**.
- Maximum **4 primary partitions**.
- Cannot store backup partition tables.
- If the MBR is corrupted → the whole disk becomes unreadable.

###  Trick Used in Old Systems
To bypass the 4-partition limit:
- 3 partitions were made **Primary**
- 1 partition was made **Extended**, which contained **Logical** partitions inside.

---

## 3. GPT (GUID Partition Table)

###  What It Is
- Introduced with the UEFI standard.
- Each partition is identified by a **Global Unique Identifier (GUID)**.
- Designed for modern, large disks and servers.

###  Structure of GPT

```text
GPT Disk Layout (Physical)
+-----------------------------+
| Protective MBR              |  ← Fake MBR to protect GPT
+-----------------------------+
| Primary GPT Header          |  ← Partition metadata
+-----------------------------+
| Partition Entries           |  ← ESP, root, swap listed here
+-----------------------------+
| Partition 1 (ESP - FAT32)   |  ← Bootloader lives here
| Partition 2 (Root - ext4)   |  ← OS files live here
| Partition 3 (Swap)          |
+-----------------------------+
| Backup GPT Header           |  ← Safety copy at disk end
+-----------------------------+

UEFI → Reads this structure → Finds ESP → Runs GRUB → Loads Kernel

```

- The **Protective MBR** prevents old BIOS tools from damaging GPT disks.
- **Primary GPT Header** — stores disk and partition metadata.
- **Backup GPT Header** — ensures recovery if the main one is corrupted.
- Each partition entry is **128 bytes** and can have a name, GUID, and attributes.

---

## 4. Advantages of GPT over MBR

| Feature | MBR | GPT |
|----------|-----|-----|
| **Max Disk Size** | 2 TB | 9.4 ZB (zettabytes) |
| **Max Partitions** | 4 Primary | 128 Primary |
| **Partition Backup** | ❌ No | ✅ Yes (Backup Header) |
| **Partition Identifier** | Numeric | GUID (Globally Unique) |
| **Corruption Recovery** | Poor | Strong |
| **Used By** | BIOS | UEFI |
| **Bootloader Location** | MBR (512 bytes) | EFI System Partition (ESP) |
| **Performance** | Slower (sequential read) | Faster (parallel access) |

---

## 5. Real-World Linux Example

###  Check Disk Partition Type
Run this command:
```bash
sudo parted -l
```
Output example:

```bash
Model: ATA VBOX HARDDISK (scsi)
Disk /dev/sda: 32.2GB
Partition Table: gpt
Disk Flags:
```

Here, `Partition Table: gpt` → confirms GPT.

If you see: `Partition Table:` msdos. That means MBR (msdos = MBR type).

Alternate Commands:

```bash
lsblk -f
```

Example:
```bash
NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
sda
├─sda1 vfat         1C1F-2323                            /boot/efi
├─sda2 ext4         3b9c7e1d-73d3-44d1-a4c1-77c6be5b9d50 /
```
`vfat` on `/boot/efi` → UEFI system (FAT32 EFI partition = GPT disk).

---

## 6. Protective MBR — Why GPT Still Has It

Even GPT disks include a small Protective MBR at sector 0.

**Purpose:**

- Prevents old BIOS/MBR tools from overwriting GPT disks.
- It “protects” modern disks from legacy software by pretending to be MBR.

---
