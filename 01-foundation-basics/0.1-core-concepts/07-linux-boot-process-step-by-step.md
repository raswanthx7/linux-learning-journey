#  07 - Linux Boot Process (Step-by-Step)

## Overview

The **Linux boot process** is the sequence of steps your computer follows from the moment you power it on until the login screen appears.  
Understanding this process helps in troubleshooting boot errors, performance issues, and managing system startup.

---

##  1. Power On and Firmware Initialization

### Step 1: Power Supply to Motherboard
When you press the power button:
- Power Supply Unit (PSU) sends electricity to the **motherboard**.
- The motherboard activates the **CPU** and the **firmware** (BIOS or UEFI).

---

##  2. Firmware (BIOS or UEFI)

### BIOS (Legacy)
- Stands for **Basic Input/Output System**.
- Stored on a small **ROM chip**.
- Initializes hardware and performs **POST (Power-On Self Test)**.
- Uses **MBR (Master Boot Record)** partitioning.
- Supports up to **4 primary partitions**.

### UEFI (Modern)
- Stands for **Unified Extensible Firmware Interface**.
- Stored on **flash memory**, not ROM.
- Provides **graphical interface** and mouse support.
- Uses **GPT (GUID Partition Table)** instead of MBR.
- Supports up to **128 partitions**.
- Loads bootloader from **EFI System Partition (ESP)** formatted with **FAT32**.

---

##  3. EFI System Partition (ESP)

The **ESP** is a small FAT32 partition that contains:
- Bootloaders (like `grubx64.efi` for Linux or `bootmgfw.efi` for Windows)
- Firmware drivers
- System utilities

### Example paths:
- `/boot/efi/EFI/ubuntu/grubx64.efi`
- `/boot/efi/EFI/redhat/grubx64.efi`

The firmware (UEFI) reads this partition to find which bootloader to execute next.

---

##  4. Bootloader Stage

Once UEFI finds and loads the bootloader, control passes to it.

### Common Linux Bootloaders:
- **GRUB2 (GNU GRand Unified Bootloader)**
- **systemd-boot**
- **LILO** (old)

### Bootloader’s Job:
1. Display a boot menu (if multiple OSs exist).
2. Load the **Linux kernel** and **initramfs** (initial RAM filesystem) into memory.
3. Pass control to the **kernel**.

---

##  5. Kernel Initialization

The **kernel** is the core part of Linux.  
It performs:
- Hardware detection
- Mounting the **initramfs**
- Setting up the **root filesystem**
- Starting the **init process**

### Kernel Files:
- Located in `/boot`
- Example: `/boot/vmlinuz-6.5.0-15-generic`

---

##  6. initramfs (Initial RAM Filesystem)

- A temporary, small filesystem loaded into RAM.
- Contains essential drivers and scripts.
- Helps the kernel **find and mount the real root filesystem** (`/`).
- Once the real root filesystem is mounted, initramfs is **discarded**.

---

##  7. Root File System Mounting

The **root filesystem** (e.g., ext4) contains:
- `/bin`, `/etc`, `/usr`, `/var`, `/home`, etc.
- System libraries, configuration files, and user data.

Once mounted:
- Kernel executes `/sbin/init` (or `systemd`).
- System startup officially begins.

---

##  8. init / systemd

- **init** was the original first user-space process (PID 1).
- Modern systems use **systemd**.
- Responsible for:
  - Starting background services (daemons)
  - Mounting filesystems
  - Managing targets (multi-user, graphical)
  - Logging system activity

### Example:
```bash
systemctl list-units --type=target
```
---

## 9. User Space and Login

After all services start:

- The login prompt (CLI or GUI) appears.
- The user logs in.
- System is ready for use.

---

## 10. Visual Overview (Simplified Flow)

```text
Power ON
   │
   ▼
[UEFI Firmware]
   │
   ▼
[EFI System Partition → grubx64.efi]
   │
   ▼
[Bootloader (GRUB)]
   │
   ▼
[Kernel + initramfs loaded into RAM]
   │
   ▼
[Mount root filesystem (/)]
   │
   ▼
[systemd starts services]
   │
   ▼
[Login prompt ready ✅]
```
---
