# 🧠 Creating Disk Partitions Using `parted` in Linux

## 📘 Topics Covered
- Introduction to `parted`
- Why use `parted` over `fdisk`
- Checking disk layout before partitioning
- Creating partitions using `parted`
- Formatting and mounting
- Verifying the setup
- Key differences between `fdisk` and `parted`
- Useful disk monitoring commands

---

## 1. Introduction
`parted` is a modern partition management utility that supports **GPT** disks and **large storage devices**.  
It provides both **interactive** and **non-interactive** modes for partitioning.

While `fdisk` is ideal for small or MBR-based disks, `parted` is the standard choice for cloud and enterprise systems with GPT layout.

---

## 2. Check Disk Information
Before partitioning, inspect the disk.

```bash
sudo parted -l
```

Displays all disks with their partition table (GPT or MBR), sector size, and partition info.

```bash
lsblk
```

Lists all block devices and mount points.

---

## 3. Start `parted` on the Disk
```bash
sudo parted /dev/sdb
```

---

## 4. Create a New GPT Label (Optional but Recommended)
If the disk is new or blank:
```
(parted) mklabel gpt
```

---

## 5. Create a New Partition
```
(parted) mkpart primary ext4 1MiB 2048MiB
```
This creates a **primary partition** from 1MB to 2GB.

Check:
```
(parted) print
```

---

## 6. Quit `parted`
```
(parted) quit
```

---

## 7. Format the Partition
```bash
sudo mkfs.ext4 /dev/sdb1
```

---

## 8. Mount the Partition
```bash
sudo mkdir /mnt/parted_disk
sudo mount /dev/sdb1 /mnt/parted_disk
```

Verify:
```bash
df -h
```

---

## 9. Key Differences: `fdisk` vs `parted`

| Feature | `fdisk` | `parted` |
|----------|----------|----------|
| Supports GPT | ❌ | ✅ |
| Works on large disks (>2TB) | ❌ | ✅ |
| Interactive menu | ✅ | ✅ |
| Script automation | ❌ | ✅ |
| Ease for cloud systems | Moderate | High |

---

## 10. Key Takeaways
- `parted` is preferred for GPT and large disks.  
- Always start partitions from **1MiB** for alignment.  
- Use `mklabel gpt` for new disks.  
- Combine `lsblk`, `df -h`, and `parted -l` for verification.  
- Never modify mounted partitions — unmount first.

---

