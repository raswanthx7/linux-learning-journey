# File Permissions Deep Dive

## Topics Covered

- File permissions: r, w, x  
- Permission groups: user, group, others  
- chmod: numeric and symbolic modes  
- chown, chgrp, umask  

---

## File Permission Types

- `r` – read: view the content of a file or list a directory  
- `w` – write: modify a file or add/remove files in a directory  
- `x` – execute: run a file or access a directory  

---

## Permission Groups

Each file or folder has 3 permission levels:

- **user** – file owner  
- **group** – group assigned to file  
- **others** – everyone else  

To view permissions:

```bash
ls -l  #-rwxr-xr--  1 user group  0 Jul 12 14:00 file.txt
```

- first char '-' = regular file
- Next three: 'rwx' for user(owner)
- Next three: 'r-x' for group
- Last three: 'r--' for others

---

## `chmod` (Change Mode)

### Numeric Mode

| Number | Permission |
|--------|------------|
| 7      | rwx        |
| 6      | rw-        |
| 5      | r-x        |
| 4      | r--        |
| 3      | -wx        |
| 2      | -w-        |
| 1      | --x        |
| 0      | ---        |

**Examples**

```bash
chmod 755 file.sh    # rwxr-xr-x  
chmod 644 notes.txt  # rw-r--r--  
chmod 700 secret.sh  # rwx------  
```

### Symbolic Mode

**Example**

```bash
chmod u+x script.sh     # Add execute to user  
chmod g-w data.txt      # Remove write from group  
chmod o=r file.txt      # Set read-only for others  
```
---

## `chown` - Change owner

### Change file owner:

```bash
chown raswanth file.txt
```

### Change owner and group:

```bash
chown raswanth:devs file.txt
```

## `chgrp` - Change Group

### Change group only:

```bash
chgrp devs file.txt
```

## `umask` - sets default permissions for new files and folders.

```bash
umask
```
- Common default: 0022
- New file = 666 - umask = 644
- New folder = 777 - umask = 755

---
