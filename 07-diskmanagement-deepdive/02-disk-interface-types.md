#  Disk Interface Types

### 1. Introduction to Disk Interfaces
In Linux and cloud environments, disks are connected to systems through interfaces — these define how the OS communicates with storage devices.  
You’ll often see disks named like `/dev/sda`, `/dev/nvme0n1`, or `/dev/vda`. Each naming pattern depends on the disk’s interface type.

---

### 2. IDE (Integrated Drive Electronics)
- **Old technology** used mostly before 2005.  
- Data and control signals share a **single ribbon cable**.  
- Slow transfer rate (~133 MB/s).  
- Appears in Linux as:
`/dev/hd[a-z]`

- **Use case:** Legacy systems only.

---

### 3. SATA (Serial ATA)

- Came after IDE — used in modern desktops and laptops.
- Simpler and cheaper than SCSI.
- Suitable for everyday workloads (booting OS, browsing, personal use).
- Limited performance for high-load server or cloud environments.


- Replaced IDE; still used widely in desktops and servers.  
- Uses **serial communication**, faster and more reliable.  
- Transfer speed: up to 6 Gb/s (≈ 600 MB/s).  
- Appears in Linux as:
`/dev/sd[a-z]`


- **Use case:** Standard hard drives (HDDs) and SSDs in budget systems.

---

### 4. SCSI (Small Computer System Interface)

- Designed for enterprise servers and data centers.
- Supports multiple disks efficiently through one controller.
- Handles high I/O workloads, meaning it can process many read/write requests simultaneously.
- Known for reliability and scalability.
- Still widely used in many virtualization platforms (like VMware, Hyper-V) and enterprise-grade storage systems.


- **Server-grade** disk interface used in enterprise systems.  
- Supports multiple devices on a single bus (up to 16).  
- Reliable and supports high-performance workloads.  
- Appears in Linux as:
`/dev/sd[a-z]`

(same naming pattern as SATA)
- **Use case:** Data centers, RAID arrays, servers.

---

### 5. NVMe (Non-Volatile Memory Express)

- Designed only for **SSDs**, not for spinning hard drives.
- Works through the **PCIe bus (very close to the CPU)** — giving extreme speed and low latency.
- Supports **parallel data paths**, so it can read/write from multiple queues at once (like thousands of simultaneous users).
- Used in modern laptops, high-end PCs, and especially cloud data centers (AWS, Azure, GCP).


- **Modern high-speed interface** designed for SSDs using the PCIe bus.  
- Works directly with CPU lanes → ultra-fast, low latency.  
- Transfer speed: several GB/s (5–7× faster than SATA SSD).  
- Appears in Linux as:
`/dev/nvme0n1`

- `nvme0` → controller  
- `n1` → namespace (logical disk)
- **Use case:** Cloud servers, modern laptops, performance-critical apps.

---

### 6. SAS (Serial Attached SCSI)
- Improved version of SCSI with **point-to-point serial connection**.  
- High performance and supports dual-port redundancy.  
- Appears in Linux as:
`/dev/sd[a-z]`

- **Use case:** Enterprise storage, large database servers.

---

### 7. How to Identify Disk Type in Linux
You can check what interface type your disk uses with:

```bash
lsblk -d -o name,rota,tran
Example output:


NAME     ROTA  TRAN
sda         1  sata
nvme0n1     0  nvme
```
- `-d` (devices only)
- `-o` (output columns)
- ROTA=1 → rotational (HDD)
- ROTA=0 → non-rotational (SSD)
- TRAN → transport type (sata, nvme, etc.)

---
