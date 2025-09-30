# Day 18 – Services & Daemons

##  Topics Covered
- Manage services: `systemctl`, `service`, `chkconfig`
- Enable, disable, start, stop services
- Check service status
- Runlevels and systemd targets (basic)

---

##  Key Notes

###  Managing Services

- `systemctl` is the standard command to manage services in **systemd-based Linux distributions** (e.g., RHEL, CentOS 7+, Fedora, Ubuntu).
- `service` is older but still works in many systems as a wrapper.
- `chkconfig` is used for managing **legacy SysV init scripts** and runlevels.

###  Common service actions with `systemctl`:
- `systemctl start <service>` – Start the service
- `systemctl stop <service>` – Stop the service
- `systemctl restart <service>` – Restart the service
- `systemctl status <service>` – View current status
- `systemctl enable <service>` – Start service at boot
- `systemctl disable <service>` – Don’t start at boot

###  chkconfig usage (for older systems):
- `chkconfig --list` – List services and their runlevels
- `chkconfig <service> on` – Enable service
- `chkconfig <service> off` – Disable service

###  Runlevels vs Targets (basic idea):
- Old systems used **runlevels** (0 to 6):
  - 0 = Halt
  - 1 = Single-user
  - 3 = Multi-user (no GUI)
  - 5 = Multi-user + GUI
  - 6 = Reboot

- Modern `systemd` systems use **targets**:
  - `multi-user.target` → Similar to runlevel 3
  - `graphical.target` → Similar to runlevel 5

Use `runlevel` or `who -r` to check current runlevel on older systems.  
Use `systemctl get-default` to check current default target.

---

##  Commands Practiced

```bash
systemctl start sshd           # Start SSH service
systemctl stop firewalld       # Stop Firewall service
systemctl restart crond        # Restart Cron service
systemctl status httpd         # Check Apache service status
systemctl enable nginx         # Enable Nginx to start on boot
systemctl disable nginx        # Disable Nginx from starting on boot
systemctl is-enabled crond     # Check if crond is enabled
systemctl list-units --type=service    # List active services

chkconfig --list               # List services (legacy)
chkconfig sshd on              # Enable SSH service (legacy)
chkconfig httpd off            # Disable Apache (legacy)

systemctl get-default          # Check default target
runlevel                       # Show current runlevel (if supported)
```
---
