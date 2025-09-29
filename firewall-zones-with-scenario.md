#  Firewalld: Zones & Interfaces

##  Topics Covered
1. What is a Zone?
2. Important Zones (Public, Internal, DMZ)
3. Other Zones (overview)
4. Interfaces in Firewalld
5. Why eth0, eth1, eth2?
6. Stark Enterprises – Real-World Firewall Scenario (with commands)
7. Cloud Engineer Role

---

##  1. What is a Zone?
- A **zone** = a security level applied to a network connection.  
- It decides:
  - Which traffic is trusted.  
  - Which ports/services are allowed.  

 Think of it as **different levels of security fences around Stark Tower**.

---

##  2. Important Zones

###  Public Zone
- Default zone in most cases.  
- For **internet-facing servers** (like web servers).  
- Only specific ports (80, 443, SSH) are open, everything else blocked.  
-  **Strictest, safest for unknown networks**.  

**Story:**  
The **front gate of Stark Tower** → only guests with passes (services you allow) can enter.

---

###  Internal Zone
- For **trusted internal networks** (LAN, corporate private network).  
- More services allowed because the network is trusted.  
- Example: File sharing, printers, internal apps.  

**Story:**  
Stark’s **employee-only entrance** → staff have more freedom inside.

---

###  DMZ (Demilitarized Zone)
- For **servers that must be accessed from both inside and outside**.  
- Example: Web servers, mail servers.  
- Services exposed: HTTP, HTTPS, SMTP.  
- Extra protection: attackers can’t directly access internal network.  

**Story:**  
Stark’s **guest lounge** → outsiders can come here, but can’t enter the restricted labs.

---

##  3. Other Zones 
- **Trusted** → Everything allowed ( risky).  
- **Drop** → Everything dropped silently (no reply).  
- **Block** → Everything blocked, but ICMP unreachable is sent.  
- **Work** → For work laptops (medium trust).  
- **Home** → For home networks (more trusted than public).  

 As a fresher/cloud engineer, focus mainly on:  
 **Public, Internal, DMZ**

---

##  4. Interfaces in Firewalld
- An **interface** = your network card (like `eth0`, `ens33`, `wlan0`).  
- Each interface must be assigned to a zone.  

**Commands:**
```bash
# Show all zones + which interface is assigned
firewall-cmd --get-active-zones
```

**Example Output:**

public
  interfaces: eth0

- If `eth0` is assigned to the public zone, then all public zone rules apply to that interface.

**Example in Cloud:**

- `eth0` (internet-facing) → assign to public zone
- `eth1` (private LAN) → assign to internal zone

---

## 5. Why eth0, eth1, eth2?

In Linux, every network interface (NIC – Network Interface Card) has a name. Examples:

- `eth0` → first Ethernet interface
- `eth1` → second Ethernet interface
- `ens33`, `ens160` → new naming in modern Linux (systemd/udev)
- `wlan0` → wireless interface

**firewalld assigns zones to interfaces**, not to the whole server.

- Each interface → one zone.
- This is how Linux knows which traffic should follow which rules.

---

## 6. Stark Enterprises – Firewall Configuration (with Commands)

**Scenario**

Stark Enterprises has three needs:

1. Tony’s Laptop (Internet-facing) → Public Zone
2. Office LAN (Finance Dept PCs) → Internal Zone
3. Stark Web Server (customer portal) → DMZ Zone


1. **Public Zone – Laptop Facing Internet**

Goal: Strict, only allow SSH (for remote login) and HTTPS (for browsing).

```bash
# Assign laptop interface to public zone
sudo firewall-cmd --zone=public --change-interface=eth0 --permanent

# Allow SSH
sudo firewall-cmd --zone=public --add-service=ssh --permanent

# Allow HTTPS
sudo firewall-cmd --zone=public --add-service=https --permanent

# Reload firewall to apply
sudo firewall-cmd --reload

# Verify
sudo firewall-cmd --zone=public --list-all
```
 Story: Tony’s laptop is exposed to the internet.
Firewall acts like JARVIS, blocking everything except SSH (for admin) and HTTPS (for secure browsing).


2. **Internal Zone – Office LAN (Finance Dept)**

Goal: Employees need access to file sharing, DNS, and database internally.

```bash
# Assign office LAN interface to internal zone
sudo firewall-cmd --zone=internal --change-interface=eth1 --permanent

# Allow DNS
sudo firewall-cmd --zone=internal --add-service=dns --permanent

# Allow Samba (file sharing)
sudo firewall-cmd --zone=internal --add-service=samba --permanent

# Allow MySQL (Finance DB)
sudo firewall-cmd --zone=internal --add-service=mysql --permanent

# Reload firewall
sudo firewall-cmd --reload

# Verify
sudo firewall-cmd --zone=internal --list-all
```
 Story: Finance Dept PCs are inside the office (trusted).
They can use DNS, file sharing, and connect to the Finance Database, but outsiders can’t.

3. **DMZ Zone – Web Server (Stark Portal)**

Goal: Web server must be accessed by outsiders (customers) & insiders (admins).

```bash
# Assign web server interface to DMZ
sudo firewall-cmd --zone=dmz --change-interface=eth2 --permanent

# Allow HTTP & HTTPS (for customers)
sudo firewall-cmd --zone=dmz --add-service=http --permanent
sudo firewall-cmd --zone=dmz --add-service=https --permanent

# Allow SSH (only for admins)
sudo firewall-cmd --zone=dmz --add-service=ssh --permanent

# Reload firewall
sudo firewall-cmd --reload

# Verify
sudo firewall-cmd --zone=dmz --list-all
```
 Story: The Stark Web Portal is in DMZ.
Customers can access the site (HTTP/HTTPS). Admins can SSH in.
If hackers attack the web server, they are stuck in DMZ and can’t reach the Finance Dept.

---

## 7. Cloud Engineer Role

- In the cloud (AWS, Azure, GCP), firewall concepts are applied as:
    - AWS Security Groups (like zones for each instance).
    - AWS NACLs (like firewalld rules at subnet level).
- A Cloud Engineer configures these rules to:
    - Expose only required services (e.g., HTTP/HTTPS on web servers).
    - Restrict internal services to private networks (DB, DNS, file sharing).
    - Place servers in DMZ-like setups for controlled access.

### Summary for Cloud Career:

- Public Zone → Internet-facing devices (strict).
- Internal Zone → Trusted LAN (office systems).
- DMZ Zone → Semi-trusted servers (web, mail).
- Cloud Engineer → Designs & applies these firewall rules on cloud servers.

---

