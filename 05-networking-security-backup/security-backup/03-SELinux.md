# SELinux (Security-Enhanced Linux)

## Topics Covered
- What is SELinux
- Why SELinux Exists
- Firewall vs SELinux (Conceptual Difference)
- SELinux Modes
- `getenforce`
- `setenforce`
- SELinux Configuration File (`/etc/selinux/config`)
- Real-World SELinux Problem Scenario (Apache + index.html)
- How to Identify SELinux Blocking
- How to Fix SELinux File Context Issues
- Command Summary

---

## What is SELinux

**SELinux (Security-Enhanced Linux)** is an internal security feature in Linux that controls **which applications and processes are allowed to access which files and resources** inside the system.

It adds an extra layer of security on top of normal Linux file permissions by restricting application behavior based on defined rules (policies).

---

## Why SELinux Exists

Traditional Linux permissions (`chmod`, `chown`) control **who** can access a file.  
SELinux controls **what program** is allowed to access a file.

This prevents:
- Compromised applications from accessing sensitive system files
- Services like Apache from reading files outside their allowed scope
- Internal misuse even when file permissions are open

---

## Firewall vs SELinux

- **Firewall** → Controls external access (network traffic entering the server)
- **SELinux** → Controls internal access (what applications can do inside the server)

In simple terms:
- Firewall protects the server from the outside
- SELinux protects the server from the inside

---

## SELinux Modes

SELinux operates in three modes:

| Mode        | Description |
|-------------|-------------|
| Enforcing   | Policies are enforced and unauthorized access is blocked |
| Permissive  | Policy violations are logged but not blocked |
| Disabled    | SELinux is completely turned off |

---

## `getenforce`

**Purpose:** Check the current SELinux mode.

```bash
getenforce
```
**Possible Outputs**

- `Enforcing` → SELinux actively blocks unauthorized access
- `Permissive` → SELinux logs violations only
- `Disabled` → SELinux is turned off

## `setenforce`

**Purpose**: Temporarily change SELinux mode (runtime only).

```bash
# Set SELinux to permissive
sudo setenforce 0

# Set SELinux to enforcing
sudo setenforce 1
```

**Important Notes:**

* Changes apply only until reboot
* After reboot, SELinux follows /etc/selinux/config

---

## SELinux Configuration File

**Purpose**: Permanently configure SELinux mode.

File Location:

```bash
/etc/selinux/config
```
**Example Configuration**

```bash
SELINUX=enforcing
SELINUXTYPE=targeted
```
**Common Settings**

* `SELINUX=enforcing` → Enforced at boot
* `SELINUX=permissive` → Log-only mode
* `SELINUX=disabled` → SELinux completely off

---

## Real-World Problem Scenario (Apache + index.html)

**Scenario**

A static website is hosted using Apache.
An `index.html` file is copied from the home directory to the web root.

```bash
cp /home/ec2-user/index.html /var/www/html/

```

File permissions appear correct:

```bash
-rw-r--r-- index.html

```

Apache service is running, but the browser shows:

```text
403 Forbidden
```
Apache error log shows:

```text
Permission denied: /var/www/html/index.html
```

**Why This Happens (SELinux Context Issue)**

* When files are copied from `/home`, they retain the home directory SELinux context.

* Apache is only allowed to read files labeled as web content.
* Files with a home directory context are blocked by SELinux, even if Linux permissions allow access.

* This is a SELinux policy restriction, not a file permission issue.

---

## How to Identify SELinux Blocking

Check the SELinux context of the file:
```bash
ls -Z /var/www/html/index.html
```

Problematic output example:
```text
user_home_t
```

Correct web content context should be:
```text
httpd_sys_content_t
```
---

## How to Fix SELinux File Context Issues

Restore the correct SELinux context for the web directory:
```bash
sudo restorecon -Rv /var/www/html
```

What This Command Does

* `-R` → Recursively applies to all files and folders

* `-v` → Shows changes being made

* Restores correct SELinux contexts based on system policy

After running the command, Apache can read the files normally.

Command Summary

```bash
# Check current SELinux mode
getenforce

# Temporarily change SELinux mode
sudo setenforce 0   # permissive
sudo setenforce 1   # enforcing

# View SELinux status and policy
sestatus

# Check SELinux file context
ls -Z /var/www/html/index.html

# Restore correct SELinux contexts for web files
sudo restorecon -Rv /var/www/html
```
---

Bottom Line

* Linux permissions control who can access files
* SELinux controls what applications can access files
* Apache requires correct SELinux context to read web files
* `restorecon` is the correct way to fix SELinux context issues
* SELinux should be fixed, not disabled, in real environments

---
