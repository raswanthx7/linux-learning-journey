# Networking Basics

---

##  Topics Covered

- Network Commands: `ip`, `ifconfig`, `nmcli`
- Viewing Network Info: IP, Gateway, DNS, Routing
- Network Testing Tools: `ping`, `traceroute`, `netstat`, `ss`, `hostname -I`

---

##  Key Notes

- `ip a` or `ip addr` – Shows all IP addresses.
- `ifconfig` – Older command for viewing and configuring network interfaces.
- `nmcli` – Command-line tool to manage NetworkManager (used in RHEL-based distros).
- `ping` – Checks if a host is reachable.
- `traceroute` – Traces the route packets take to a destination.
- `netstat -tuln` – Shows listening ports (deprecated in some distros).
- `ss -tuln` – Modern alternative to `netstat`.
- `hostname -I` – Displays the system's IP address.

---

##  Commands Practiced

```bash
# View IP address (modern)
ip a

# View IP address (legacy)
ifconfig

# View DNS, gateway, etc.
nmcli dev show

# Display only the system IP
hostname -I

# View routing table
ip route

# Ping a domain or IP
ping google.com

# Trace the path to a destination
traceroute google.com

# View all active connections
netstat -tuln

# Modern alternative to netstat
ss -tuln
```

---
