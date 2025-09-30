# SELinux (Security-Enhanced Linux)

##  Topics Covered
- SELinux Overview
- `getenforce`
- `setenforce`
- SELinux Config File (`/etc/selinux/config`)
- Beginner-Friendly Command Summary

---

##  SELinux Overview (Quick Recap)

- **Firewall** = outer guard (controls who enters the server).  
- **SELinux** = internal guard (controls what users/processes can do inside the server).  

Think of SELinux as **Stark Tower internal security**:
- Firewall = external guards at the gate  
- SELinux = guards and cameras inside, controlling which rooms people can access  

---

##  1️⃣ getenforce

**Purpose:** Check the current SELinux mode.  

```bash
getenforce
```
##  getenforce Outputs

**Possible outputs:**

| Output     | Meaning                                                            |
|------------|--------------------------------------------------------------------|
| Enforcing  | SELinux is active → policy enforced → unauthorized access blocked  |
| Permissive | SELinux logs violations, but does not block → useful for testing   |
| Disabled   | SELinux is completely off → no internal access control             |

---

**Example Story (Stark Tower Security):**

- **Enforcing** → blocks employees from entering unauthorized rooms.  
- **Permissive** → logs rule violations but still lets them in.  
- **Disabled** → cameras off, no rules applied.  

---

##  setenforce

**Purpose:**  
Temporarily change SELinux mode without reboot.

---

###  Commands

```bash
# Set to enforcing mode
sudo setenforce 1

# Set to permissive mode
sudo setenforce 0
```
Notes:

- Affects only the current runtime.
- After reboot → mode resets to what’s in /etc/selinux/config.

**Example Story:**

Cloud Engineer wants to test an app → switches to permissive, tests, then switches back to enforcing.

---

## 3. SELinux Config File

**Purpose:** Permanently configure SELinux mode (survives reboot).

 File location:

``/etc/selinux/config``


**Example content:**
```conf
# This file controls SELinux mode on boot
SELINUX=enforcing
# SELINUX=permissive
# SELINUX=disabled
SELINUXTYPE=targeted
```

**Key Points:**

- `SELINUX=enforcing` → always active at boot.
- `SELINUX=permissive` → logs only.
- `SELINUX=disabled` → completely off.

Example Story:

Production servers → `enforcing` (strict security).

Dev/test servers → `permissive` (logs issues, doesn’t block devs).

---

## 4. Commands Summary (Beginner-Friendly)

```bash
# Check current SELinux mode
getenforce

# Temporarily change mode
sudo setenforce 0   # permissive
sudo setenforce 1   # enforcing

# Permanently change mode (edit config file)
sudo nano /etc/selinux/config
# Set SELINUX=enforcing / permissive / disabled

# Check full SELinux status and policy
sestatus
```
---

 Bottom Line (Cloud Perspective)

- Firewall → controls who enters the server.
- SELinux → controls what happens inside the server.
- getenforce → check mode.
- setenforce → temporary change.
- /etc/selinux/config → permanent change.

---
