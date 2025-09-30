#  Package Management

---

## 🔹 Topics Covered

- Tools: `yum`, `dnf`, `rpm`, `apt`
- Install, update, and remove packages
- List installed packages
- Check software dependencies

---

##  Key Notes

- **Package managers** are tools used to install, update, and manage software in Linux.
- **RHEL-based systems** use:
  - `yum` (older, still used in scripts)
  - `dnf` (modern replacement for yum)
  - `rpm` (used to install individual `.rpm` packages)
- **Debian/Ubuntu-based systems** use:
  - `apt` or `apt-get`
- Always keep the system updated with the latest packages and security patches.
- You can also check dependencies and software info using package commands.

---

##  Commands Practiced

```bash
# YUM/DNF (RHEL, CentOS, Rocky, Fedora)
sudo yum install httpd             # Install Apache (older systems)
sudo dnf install nginx             # Install Nginx (modern systems)
sudo dnf update                    # Update all packages
sudo dnf remove nginx              # Remove a package
dnf list installed                 # List all installed packages
dnf info nginx                     # Show info about a package

# RPM (Low-level tool for .rpm files)
rpm -ivh package.rpm              # Install .rpm
rpm -Uvh package.rpm              # Upgrade
rpm -e package-name               # Erase (remove)
rpm -qa                           # Query all installed packages

# APT (Ubuntu/Debian systems)
sudo apt update                   # Update package list
sudo apt install curl             # Install a package
sudo apt upgrade                  # Upgrade installed packages
sudo apt remove curl              # Remove package
apt list --installed              # List installed packages
```

---
