#  Day 09 – Superuser & sudo

##  Topics Covered

- Root user and privileges
- Switching users: `su`, `sudo`, `exit`
- Granting sudo access
- `wheel` group
- `sudoers` file (basic intro)

---

##  Key Notes

- **Root user** (UID 0) is the most powerful account with unrestricted system access.
- **`su` (Substitute User)** allows you to switch to another user account.  
  Example: `su - root`  
- **`sudo` (Superuser Do)** allows permitted users to run commands as root without switching users.  
  Example: `sudo apt update`

- **`exit`** is used to return to the previous user or session.

- The **`wheel` group** is a special group that controls who can use `sudo` in some Linux distributions (like RHEL-based systems).  
  Add a user to the wheel group:  
  ```bash
  usermod -aG wheel username

- `/etc/sudoers` file defines who can run what as root.
Always edit with:

```bash
visudo
```

## Commands Practiced

```bash
su -                       # Switch to root user (if password is known)
sudo command               # Run a command as root
exit                       # Exit from current user session

usermod -aG wheel raswanth  # Add user to wheel group (for sudo access)

visudo                     # Safely edit sudoers file
```
---
