#  EFI System Partition (ESP)

## Topics covered

- What Is the EFI System Partition (ESP)?
- Location and Format
- What’s Inside the ESP?
- Why FAT32 File System?
- Role in the Boot Process
- ESP vs /boot
- Commands to Identify ESP in Linux
- Real-World Example (Red Hat / CentOS)

---

## 1. What Is the EFI System Partition (ESP)?

The **EFI System Partition (ESP)** is a **small, special partition** on a UEFI-based disk.  
It stores **bootloaders**, **drivers**, and **system startup files** required by the **UEFI firmware** to boot the operating system.

 Think of the ESP as the “starting point” for the computer —  
the place your UEFI firmware looks at immediately after powering on.

---

## 2. Location and Format

| Attribute | Description |
|------------|--------------|
| **Partition Type** | EFI System Partition |
| **File System** | FAT32 |
| **Typical Size** | 100 MB – 512 MB |
| **Mount Point (Linux)** | `/boot/efi` |
| **Visibility** | Hidden from normal users |
| **Disk Type** | GPT (GUID Partition Table) |

The **ESP** is mandatory on all **UEFI + GPT** systems.

---

## 3. What’s Inside the ESP?

Inside the ESP, you’ll find bootloaders and related files organized in a standard structure.

Example directory layout:

```text
/boot/efi/
├── EFI/
│ ├── BOOT/
│ │ └── BOOTX64.EFI
│ ├── redhat/
│ │ └── grubx64.efi
│ ├── Microsoft/
│ │ └── Boot/bootmgfw.efi
│ └── ubuntu/
│ └── grubx64.efi
```

###  Explanation:
- `EFI/BOOT/BOOTX64.EFI` → Default fallback bootloader (used if no vendor boot entry found).
- `EFI/redhat/grubx64.efi` → GRUB bootloader for Red Hat–based systems.
- `EFI/Microsoft/Boot/bootmgfw.efi` → Windows boot manager.
- `EFI/ubuntu/grubx64.efi` → Bootloader for Ubuntu.

Each OS places its own `.efi` executable file here.

---

## 4. Why FAT32 File System?

- **UEFI firmware** is built to understand **FAT32**, not EXT4 or NTFS.
- FAT32 is lightweight, cross-platform, and readable by all operating systems.
- UEFI needs a **universal file system** it can access *before* any OS drivers are loaded.

That’s why ESP always uses **FAT32**, even if your main OS partition uses **EXT4** (Linux) or **NTFS** (Windows).

---

## 5. Role in the Boot Process

Here’s how the ESP fits into the complete boot flow:

1. **Power On**
   - Power reaches the motherboard.
   - UEFI firmware stored in flash memory starts executing.

2. **POST (Power-On Self-Test)**
   - UEFI checks hardware (RAM, CPU, keyboard, disks, etc.).

3. **UEFI Firmware Looks for the ESP**
   - Scans the disk for a **partition with type = EFI System Partition** (GPT ID: `EF00`).

4. **UEFI Reads Bootloader from ESP**
   - It loads the `.efi` executable (like `grubx64.efi`) into RAM.

5. **Bootloader Executes**
   - GRUB (or other bootloader) takes over.
   - GRUB loads the Linux **kernel** and **initramfs** from `/boot` (which is usually EXT4).

6. **Kernel Initialization**
   - The kernel mounts the **root filesystem** and starts the Linux OS.

---

## 6. ESP vs /boot

| Feature | `/boot` | `/boot/efi` (ESP) |
|----------|----------|------------------|
| **Purpose** | Stores Linux kernel & GRUB configs | Stores EFI bootloaders |
| **File System** | EXT4 (Linux-specific) | FAT32 (UEFI-compatible) |
| **Read by** | GRUB (after bootloader runs) | UEFI firmware (first step) |
| **Managed By** | Linux OS | Firmware + Bootloader |

So, ESP = firmware-level; `/boot` = OS-level.

---

## 7. Commands to Identify ESP in Linux

Run:
```bash
lsblk -f
```
Example Output:
```bash
NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
sda
├─sda1 vfat         1C1F-2323                            /boot/efi
├─sda2 ext4         3b9c7e1d-73d3-44d1-a4c1-77c6be5b9d50 /
```
Here:

- `/dev/sda1` → FAT32 → `/boot/efi` → ESP
- `/dev/sda2` → EXT4 → `/` → Root filesystem

---

## 8. Real-World Example (Red Hat / CentOS)

In Red Hat–based systems:

- ESP is mounted at `/boot/efi`
- GRUB’s `.efi` binary lives inside `/boot/efi/EFI/redhat/grubx64.efi`
- Kernel and `initramfs` are stored in `/boot`
- Together they form the complete boot process.

---
