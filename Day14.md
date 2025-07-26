#  Day 14 – Weekly Review + Hands-on

---

## Topics Covered

- Practice: permissions, file operations, redirection
- Real-world tasks: create users, set permissions, make backups
- Mini script: create user folders with logs

---

##  Key Notes

-  Week 2 focused on:
  - Mastering **file permissions** (`chmod`, `chown`, `umask`)
  - Handling **file operations** (`cp`, `mv`, `rm`, `mkdir`, `touch`)
  - Using **redirection and piping** (`>`, `>>`, `<`, `|`, `tee`, etc.)
  - Exploring **users and groups** management (`useradd`, `groupadd`, `passwd`)
  - Understanding **links**, file types, metadata, and superuser privileges

-  Weekly hands-on practice:
  - Created and deleted users
  - Set group permissions
  - Backed up files from one directory to another
  - Used redirection to create and log outputs

-  Mini script idea:
  - Create user folders with individual logs using shell scripting:

---

## Commands Practiced

```bash
# Create users with a loop and prepare folders
for user in user1 user2 user3; do
  sudo useradd $user
  mkdir -p /home/$user/logs
  touch /home/$user/logs/log.txt
  echo "Folder created for $user" >> /home/$user/logs/log.txt
done

# Set permissions and ownership
chmod -R 750 /home/user*/logs
chown -R $user:$user /home/$user/logs

# Back up logs
cp -r /home/user*/logs /backup/logs/

# Use redirection to save command outputs
ls -l /home/user*/logs > log-report.txt
```

---
