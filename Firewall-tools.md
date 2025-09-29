# Firewall Tools in Linux

## Topics Covered
1. What is Firewall?
2. Popular Firewall Tools
3. Open/close Port

---

## 1. What is a Firewall?
A firewall is like a **bodyguard for your Linux system**.  
- It controls **who can enter and leave your server**.  
- Protects against **unauthorized access, hackers, and malicious traffic**.

---

## 2. Popular Firewall Tools

### 2.1 firewalld (CentOS / RHEL / Fedora)
- **Dynamic firewall** → add/remove rules **without restarting**.  
- Uses **zones** to assign security levels to network interfaces.  
- Can use **service names** instead of port numbers.  

**Example commands:**
```bash
sudo firewall-cmd --get-zones
sudo firewall-cmd --zone=public --add-service=ssh --permanent
sudo firewall-cmd --reload
```

### 2.2 UFW (Ubuntu / Debian)

- **User-friendly firewall** for beginners.
- **Dynamic** → add/remove rules without restarting.
- Can use service names (`ssh`, `http`, `https`) instead of numeric ports.

**Example commands:**
```bash
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw status
```

### 2.3 iptables

- **Old low-level firewall** tool.
- Powerful but complex → not beginner-friendly.
- Usually replaced by firewalld / UFW today.

---

## 3. Open/Close Ports
**firewalld**

Add a port:
```bash
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```
Remove a port:
```bash
sudo firewall-cmd --zone=public --remove-port=80/tcp --permanent
sudo firewall-cmd --reload
```

**UFW**

Allow/Deny ports:
```bash
sudo ufw allow 22/tcp
sudo ufw deny 23/tcp
```

---
