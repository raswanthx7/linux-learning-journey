# SSH & Remote Access

---

##  Topics Covered

- Connecting to remote systems using `ssh`
- Transferring files securely with `scp` and `sftp`
- Creating SSH key pair with `ssh-keygen`
- Key-based authentication setup

---

##  Key Notes

- `ssh user@host` → Connect to a remote server using SSH
- `scp file user@host:/path` → Securely copy files to remote system
- `sftp user@host` → Interactive file transfer session (secure)
- `ssh-keygen` → Generates SSH key pair (`id_rsa`, `id_rsa.pub`)
- Public key is added to `~/.ssh/authorized_keys` on the server
- Improves security: No password needed after key setup

---

##  Commands Practiced

```bash
# Connect to a remote server
ssh username@192.168.1.10

# Generate SSH key pair
ssh-keygen

# Copy public key to remote server (for password-less login)
ssh-copy-id username@192.168.1.10

# Securely copy a file to remote system
scp myfile.txt username@192.168.1.10:/home/username/

# Securely copy a file from remote to local
scp username@192.168.1.10:/home/username/myfile.txt .

# Start secure file transfer session
sftp username@192.168.1.10
```

---
