#  BIOS vs UEFI — Understanding the Boot Firmware

## Topics covered
- What is Firmware?
- BIOS (Legacy Boot)
- UEFI (Modern Boot)
- Key Difference — BIOS vs UEFI
- Diagram — Boot Flow Comparison

---

## 1. What is Firmware?
Firmware is low-level software embedded in the motherboard.  
It acts as a **bridge between hardware and the operating system** — controlling the very first boot steps after power-on.

There are two main types of firmware:
1. **BIOS (Basic Input/Output System)** — Legacy system (old generation)
2. **UEFI (Unified Extensible Firmware Interface)** — Modern replacement for BIOS

---

## 2. BIOS (Legacy Boot)

###  What It Is
- Introduced in the early 1980s.
- Stored in ROM/Flash memory on the motherboard.
- Uses **16-bit real mode**, which limits the amount of memory it can use.
- Works with **MBR (Master Boot Record)** partitioning.

###  How BIOS Boots
1. Runs **POST (Power-On Self-Test)** to check CPU, RAM, and devices.
2. Looks for a **bootable disk**.
3. Reads the **first 512 bytes** of the disk → this contains the **MBR**.
4. MBR contains a small bootloader (Stage 1) → loads the full bootloader (Stage 2).
5. Bootloader loads the **kernel**.

###  Limitations of BIOS
- Supports disks only up to **2 TB**.
- Can only use **4 primary partitions**.
- Slower startup time.
- No native support for secure booting or large disks.
- Outdated for modern hardware.

---

## 3. UEFI (Modern Boot)

###  What It Is
- Stands for **Unified Extensible Firmware Interface**.
- Designed to replace BIOS — faster, more flexible, and secure.
- Works in **32-bit or 64-bit mode** (can access more memory).
- Uses **GPT (GUID Partition Table)** instead of MBR.

###  How UEFI Boots
1. UEFI firmware initializes hardware.
2. It looks for a **bootloader `.efi` file** inside the **EFI System Partition (ESP)** — usually FAT32 formatted.
3. Loads the `.efi` file (e.g., `grubx64.efi`) into RAM.
4. The bootloader starts the **Linux kernel**.

###  Advantages of UEFI
- Supports disks larger than **2 TB**.
- Allows up to **128 partitions** (with GPT).
- Faster boot time.
- **Secure Boot** support (verifies bootloader signature).
- Supports graphical interfaces (mouse, GUI-based setup).
- Can boot directly from drives, USBs, and network (PXE).

---

## 4. Key Difference — BIOS vs UEFI

| Feature | BIOS | UEFI |
|----------|------|------|
| **Full Form** | Basic Input/Output System | Unified Extensible Firmware Interface |
| **Storage Type** | ROM/Flash memory | Flash memory |
| **Partition Type** | MBR (Master Boot Record) | GPT (GUID Partition Table) |
| **Disk Size Support** | Up to 2 TB | Up to 9.4 ZB (zettabytes) |
| **Boot Mode** | Legacy | UEFI |
| **Architecture** | 16-bit | 32/64-bit |
| **Speed** | Slower | Faster |
| **Interface** | Text-based | Graphical (mouse support) |
| **Secure Boot** | ❌ No | ✅ Yes |
| **Max Partitions** | 4 | 128 |
| **Bootloader Location** | MBR | EFI System Partition |
| **Compatibility** | Older systems | Modern systems (post-2015) |

---

## 5. Diagram — Boot Flow Comparison

```text
BIOS Boot Flow:                     UEFI Boot Flow:
---------------                     ----------------
Power On                            Power On
   ↓                                   ↓
POST                                 POST
   ↓                                   ↓
MBR (512 bytes)                      EFI System Partition (FAT32)
   ↓                                   ↓
Bootloader (GRUB Stage 2)            .efi Bootloader (e.g., grubx64.efi)
   ↓                                   ↓
Kernel Loaded                        Kernel Loaded
   ↓                                   ↓
System Init (init/systemd)           System Init (init/systemd)
```

---

