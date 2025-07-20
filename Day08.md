#  Day 08 – Users and Groups

## 🔹 Topics Covered

- User Management: `useradd`, `usermod`, `userdel`
- Group Management: `groupadd`, `groupdel`, `gpasswd`
- Important Files: `/etc/passwd`, `/etc/shadow`, `/etc/group`
- Other Commands: `id`, `who`, `groups`, `passwd`

---

## Key Notes

- **`useradd <username>`** – Create a new user  
- **`usermod`** – Modify an existing user (e.g., change shell, group)  
- **`userdel <username>`** – Delete a user  

- **`groupadd <groupname>`** – Create a new group  
- **`groupdel <groupname>`** – Delete a group  
- **`gpasswd -a <user> <group>`** – Add user to a group  

---

## Important Files

| File Path         | Description                                 |
|-------------------|---------------------------------------------|
| `/etc/passwd`     | Stores basic user account info              |
| `/etc/shadow`     | Stores encrypted user passwords             |
| `/etc/group`      | Stores group information                    |

---

## Commands Practiced

```bash
useradd raswanth             # Add a new user
usermod -s /bin/bash raswanth  # Change shell for user
userdel raswanth             # Delete the user

groupadd devteam             # Add new group
gpasswd -a raswanth devteam  # Add user to group
groupdel devteam             # Delete group

id                           # Show current user ID and groups
who                          # Show logged-in users
groups                       # Show groups of current user
passwd                       # Change user password
```

---
