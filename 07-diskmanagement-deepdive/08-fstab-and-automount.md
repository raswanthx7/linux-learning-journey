#  /etc/fstab and Automount Configuration

##  Topics Covered
- What is `/etc/fstab`
- Purpose of `/etc/fstab` in Linux
- Structure of `/etc/fstab` entries
- Explanation of each field (UUID, mount point, type, options, dump, pass)
- Understanding `defaults`
- Meaning of `0`, `1`, and `2` in the last two fields
- Using labels instead of UUIDs
- Backup precautions before editing
- Real-world examples
- Troubleshooting and recovery tips

---

##  Introduction: What is `/etc/fstab`

`/etc/fstab` (File System Table) is a configuration file that defines how and where disk partitions, devices, and remote filesystems should be mounted automatically during system boot.  

It allows Linux to mount all the required filesystems without manual intervention.

When you execute:
```bash
mount -a
```
Linux reads `/etc/fstab` and mounts all entries listed in it

---

## Purpose of `/etc/fstab`

- Automates mounting of disks or partitions at boot.
- Ensures consistent mount points for devices.
- Supports UUIDs and LABELs to avoid dependency on device names like `/dev/sdb1`.
Enables control over mount options (permissions, performance, or access restrictions).

---

## Structure of `/etc/fstab`

Each entry in `/etc/fstab` follows this format:

```
<file system>   <mount point>   <type>   <options>   <dump>   <pass>
```

Example:

```
UUID=2a4d6d2c-9b3b-4c4a-bd90-0d1b89d8210c   /       ext4   defaults   0   1
UUID=4a1b2c3d-7e8f-4d9a-bcde-1234567890ab   /home   ext4   defaults   0   2
```

---

## Understanding Each Field

### 1. File System

Can be specified as:

- UUID=xxxxxxxxx
- LABEL=MyDisk
- Or directly `/dev/sda1` (not recommended)

Use UUID or LABEL because `/dev/sdX` names can change when disks are reattached or new ones are added.

Get all UUIDs:
```bash
blkid
```

### 2. Mount Point

The directory where the filesystem will be attached (mounted).
Examples:
```bash
/       → root filesystem
/home   → user data
/boot   → bootloader files
/mnt/data → custom data mount
```

### 3. Type

Specifies the filesystem type:
```bash
ext4, xfs, ntfs, vfat, swap, nfs
```

### 4. Options

Defines how the filesystem should be mounted.

### Common Options

| Option | Description |
|--------|-------------|
| defaults | Standard safe settings (`rw`, `suid`, `dev`, `exec`, `auto`, `nouser`, `async`) |
| ro | Mount read-only |
| rw | Mount read-write |
| noexec | Prevent execution of binaries |
| nosuid | Ignore SUID bits |
| nodev | Ignore device files |
| noauto | Don’t mount at boot |
| user | Allow non-root users to mount |
| x-systemd.automount | Enables automounting on access |

Combine multiple:
```bash
defaults,noexec,nosuid
```

### 5. Dump (0 or 1)

Controls if the filesystem should be backed up by `dump`.

| Value | Meaning |
|-------|---------|
| 0     | Do not back up |
| 1     | Back up (rarely used today) |

> Most modern systems use `0`.

### 6. Pass (0, 1, 2)

Specifies the order for filesystem check (`fsck`) during boot.

| Value | Meaning |
|-------|---------|
| 0     | Skip filesystem check |
| 1     | Check first — reserved for `/` (root) |
| 2     | Check later — for `/home`, `/data`, etc. |

**Example `/etc/fstab` File**
```
UUID=1234-root   /       ext4   defaults   0   1
UUID=5678-home   /home   ext4   defaults   0   2
UUID=9abc-data   /mnt/data   ext4   defaults,noexec,nosuid   0   2
```

---

## Precaution Before Editing `/etc/fstab`

Why Backup Is Critical

`/etc/fstab` is a boot-critical file.
If you make a mistake (like a wrong UUID, a missing field, or invalid filesystem type), your system may fail to boot and drop you into emergency mode.

To stay safe:
```bash
cp /etc/fstab /etc/fstab.bak
```

If your system fails to boot, boot into rescue mode and restore:
```bash
cp /etc/fstab.bak /etc/fstab
```

This instantly reverts your configuration to the last known working version.

---

## Using Labels Instead of UUIDs

Sometimes, using **LABEL** instead of UUID can make `/etc/fstab` more readable.

Example:
```bash
LABEL=DataDisk   /mnt/data   ext4   defaults   0   2
```
**To Create or Change a Label**

For ext2/ext3/ext4 filesystems:
```bash
e2label /dev/sdb1 DataDisk
```

For XFS filesystems:
```bash
xfs_admin -L DataDisk /dev/sdb1
```

For FAT/NTFS filesystems:
```bash
mlabel -i /dev/sdb1 ::DataDisk
```

Then verify it:
```bash
blkid
```

You’ll see output like:
```bash
/dev/sdb1: LABEL="DataDisk" UUID="9abc-1234" TYPE="ext4"
```

Now, you can safely use:
```bash
LABEL=DataDisk   /mnt/data   ext4   defaults   0   2
```

## Real-World Example
Add a new disk for permanent mount:

1. Create a mount point:
```bash
mkdir /mnt/data
```

2. Find UUID:
```bash
blkid
```

3. Add entry to `/etc/fstab`:
```bash
UUID=abcd1234-5678-90ef-ghij-klmnopqrstuv   /mnt/data   ext4   defaults   0   2
```

4. Test:
```bash
mount -a
```
If no errors appear — success 

---

##  Troubleshooting Tips

| Problem | Solution |
|----------|-----------|
| System doesn’t boot | Boot into rescue mode and restore `/etc/fstab.bak` |
| Syntax error | Run `mount -a` to test your `/etc/fstab` file before reboot |
| Want to disable mount temporarily | Add the `noauto` option to the entry |
| Check current mounts | Use `findmnt` or `mount` commands |

---
