#  07 - Linux Boot Process (Step-by-Step)

## Overview

The **Linux boot process** is the sequence of steps your computer follows from the moment you power it on until the login screen appears.  
Understanding this process helps in troubleshooting boot errors, performance issues, and managing system startup.

---

##  1. Power On and Firmware Initialization

### Step 1: Power Supply to Motherboard

- You press the power button.
- Power flows from the Power Supply Unit (PSU) to the motherboard and its components.
- The CPU starts up — but it doesn’t know what to do yet.
- So it looks for instructions in a small chip — the firmware (UEFI or BIOS).

---

##  2. Firmware (BIOS or UEFI)

### Step 2: Firmware (UEFI/BIOS)

- Firmware lives inside flash memory on the motherboard.
- It runs a test called POST (Power-On Self Test) → checks RAM, CPU, disk, keyboard, etc.
- If all hardware is OK , firmware proceeds to find a bootable disk.
- In modern systems, firmware = UEFI, which reads a special partition called EFI System Partition (ESP) formatted as FAT32.
- Then it loads that file (bootloader) into RAM and gives control to it.

Inside ESP it looks for files like:
```bash
/EFI/BOOT/BOOTX64.EFI
/EFI/GRUB/GRUBX64.EFI
```
  - GRUBX64.EFI → main Ubuntu bootloader (the one that normally runs every day).
  - BOOTX64.EFI → backup/fallback bootloader.

#### BIOS (Legacy)
- Stands for **Basic Input/Output System**.
- Stored on a small **ROM chip**.
- Initializes hardware and performs **POST (Power-On Self Test)**.
- Uses **MBR (Master Boot Record)** partitioning.
- Supports up to **4 primary partitions**.

#### UEFI (Modern)
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

### Step 3: Bootloader (GRUB)

- The bootloader is the first real software your firmware runs.
- Example in Linux: GRUB (GRand Unified Bootloader).

- GRUB’s job:
  1. Show you a **boot menu** (Ubuntu, Rescue, etc.).
  2. **Load the Linux kernel** and **initramfs** into RAM.
- `initramfs` = a small temporary filesystem that helps Linux find and mount the real root filesystem.

#### Common Linux Bootloaders:
- **GRUB2 (GNU GRand Unified Bootloader)**
- **systemd-boot**
- **LILO** (old)

---

##  5. Kernel Initialization

### Step 4: Kernel

- Now the Linux kernel starts running inside RAM.
- Kernel = core of Linux → controls CPU, memory, devices, etc.
- It mounts the real root filesystem (usually ext4) from your disk.
- Then it starts the first process: systemd (PID 1).

The **kernel** is the core part of Linux.  
It performs:
- Hardware detection
- Mounting the **initramfs**
- Setting up the **root filesystem**
- Starting the **init process**

#### Kernel Files:
- Located in `/boot`
- Example: `/boot/vmlinuz-6.5.0-15-generic`

---

##  6. initramfs (Initial RAM Filesystem)

- A temporary, small filesystem loaded into RAM.
- Contains essential drivers and scripts.
- Helps the kernel **find and mount the real root filesystem** (`/`).
- Once the real root filesystem is mounted, initramfs is **discarded**.
- initramfs = a small temporary filesystem that helps Linux find and mount the real root filesystem

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

### Step 5: systemd → User Space

- systemd initializes everything:
  - Mounts other partitions
  - Starts network, login, graphical target
- Finally it spawns your login shell (bash, zsh, etc.) or GUI (if available).

 Now your Linux system is fully booted and ready for user commands.

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
