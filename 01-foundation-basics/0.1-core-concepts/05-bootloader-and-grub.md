#  Bootloader and GRUB

## Topics covered
- What Is a Bootloader?
- Role of the Bootloader in the Boot Process
- GRUB — The Most Common Linux Bootloader


## 1. What Is a Bootloader?

A **bootloader** is a small software program responsible for **loading the operating system** into memory (RAM) after the firmware (BIOS or UEFI) finishes its job.

 Think of it like a “bridge” between:
- The **firmware** (which initializes hardware), and  
- The **Operating System** (which manages the entire computer).

Without a bootloader, your system wouldn’t know **where** or **how** to start the OS.

---

## 2. Role of the Bootloader in the Boot Process

Here’s a simplified sequence:

1. **Power ON → Firmware starts**
   - UEFI firmware (in flash memory) initializes the hardware (POST).
2. **UEFI locates the Bootloader**
   - It searches for an `.efi` file (like `grubx64.efi`) inside the **ESP (EFI System Partition)**.
3. **UEFI loads the Bootloader into RAM**
   - The bootloader now takes control.
4. **Bootloader locates the OS kernel**
   - It reads configuration files (like `/boot/grub2/grub.cfg`) to find which OS to boot.
5. **Bootloader loads Kernel + Initramfs into RAM**
   - It hands over control to the kernel.
6. **Kernel initializes system**
   - Mounts the root filesystem and starts user space processes.

---

## 3. GRUB — The Most Common Linux Bootloader

**GRUB (GRand Unified Bootloader)** is the default bootloader for most Linux distributions.

It’s powerful, flexible, and supports:
- **Multiple OS booting** (Linux, Windows, etc.)
- **Kernel selection**
- **Recovery modes**
- **Command-line troubleshooting**

---




