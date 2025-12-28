# Linux PATH 

## Topics Covered

1. What PATH is
2. Why PATH exists
3. PATH order (left → right)
4. Why the current directory (.) is NOT in PATH
5. Why we use `./` to run a script
6. Meaning of each PATH directory
7. User vs system scope
8. Shell built-in commands
9. Commonly used `/usr/bin` commands
10. Commonly used `/usr/sbin` commands
11. How Linux decides what to execute (execution flow)
12. summary

---

## 1. What is PATH?

`PATH` is an **environment variable** that contains a **list of directories**.

Linux uses PATH to decide **where to look for executable commands** when you type a command in the terminal.

Example:

```bash
echo $PATH
```

---

## 2. Why PATH exists

Linux does NOT search the entire system to find commands.

Instead:

* PATH acts like a **trusted command lookup list**
* Linux checks only directories listed in PATH
* This improves **security** and **performance**

---

## 3. PATH search order

PATH is checked **from left to right**.

The **first match wins**.

Example PATH:

```bash
[starkdev@ctrl home]$ echo $PATH
/home/starkdev/.local/bin:/home/starkdev/bin:/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin
```

```text
/home/starkdev/.local/bin
/home/starkdev/bin
/usr/local/bin
/usr/local/sbin
/usr/bin
/usr/sbin
```

---
## 4. Why the current directory (.) is NOT in PATH

For security.
Linux does not trust the current directory because it can contain malicious executables that could be run accidentally.

### The real problem Linux is preventing

scenario:

1. You cd /tmp
2. Someone creates a fake executable named ls in /tmp
3. You type:
```bash
ls
```
4. If . were in PATH, Linux would run ./ls, not /usr/bin/ls

**malicious code runs.**

---

## 5. Why we use `./` to run a script

In Linux, **commands and scripts are both executable files**.

When you run a command **without specifying a path**:
```bash
hello.sh
```
Linux will:

- Search only the directories listed in PATH
- If the executable is not found in PATH, it returns:

```
command not found
```
**The current directory (.) is NOT in PATH, so Linux will not look there automatically.**
**By using ./, we explicitly tell Linux to execute the file from the current directory.**

---

## 6. Meaning of each PATH directory

### `/home/starkdev/.local/bin`

* User-specific installed commands
* Created when using:
* .local is hidden(Hidden doesn’t mean unimportant — it means ‘don’t touch unless needed’)
* .local is hidden because it stores user-level software and data managed by tools, not files the user works with directly
* Any file or folder starting with . is hidden(Shown only with ls -a)

  ```bash
  pip install --user <tool>
  ```
* Only **starkdev** can use
* No sudo required

---

### `/home/starkdev/bin`

* Personal scripts created by the user
* Must be created manually
* Scripts placed here can run without `./`
* Only **starkdev** can use

---

### `/usr/local/bin`

* Manually installed system-wide software
* Installed using admin methods (curl, tar, installer scripts)
* Example: AWS CLI v2, Terraform
* All users can run these commands

---

### `/usr/local/sbin`

* Manually installed **admin/system tools**
* Used by system administrators
* Usually requires sudo

---

### `/usr/bin`

* OS-managed normal user commands
* Installed using `yum` / `apt`
* Examples: `ls`, `cat`, `grep`, `curl`
* Available to all users

---

### `/usr/sbin`

* OS-managed system/admin commands
* Usually require sudo
* Examples: `nginx`, `sshd`, `useradd`

---

## 7. User vs system scope

| Location       | Scope              |
| -------------- | ------------------ |
| `/home/*`      | User-specific      |
| `/usr/bin`     | System-wide        |
| `/usr/sbin`    | System-wide admin  |
| `/usr/local/*` | Manual system-wide |

---

## 8. Commonly used shell built-in commands

Shell built-ins are **part of the shell** (not files in `/usr/bin`).

### Frequently used built-ins (cloud / Linux admin)

```text
cd        - change directory
pwd       - print working directory
echo      - print output
export    - set environment variables
unset     - remove variables
history   - command history
alias     - create shortcuts
unalias   - remove shortcuts
source / .- run script in same shell
exit      - exit shell
read      - take user input
jobs      - list background jobs
fg        - foreground job
bg        - background job
```

Check built-in:

```bash
type cd
```

---

## 9. Commonly used `/usr/bin` commands (normal binaries)

### File & directory

```text
ls
cp
mv
rm
mkdir
touch
chmod
chown
```

### File viewing & text

```text
cat
less
head
tail
grep
wc
```

### System info

```text
ps
top
htop
df
du
free
uptime
```

### Networking

```text
ip
ss
ping
curl
wget
```

---

## 10. Commonly used `/usr/sbin` commands (admin binaries)

```text
nginx
sshd
useradd
usermod
userdel
iptables
firewalld
reboot
shutdown
```

---

## 11. How Linux decides what to execute (execution flow)

When you type a command, Bash follows this **exact order**:

1. Alias
2. Shell function
3. Shell built-in
4. External command (PATH search)

Execution stops at the **first match**.

Example:

```bash
type ls
```

---

## 12. summary

* PATH is an ordered list of directories
* Linux searches PATH left → right
* Built-ins are checked before PATH
* `/home/*` paths are user-specific
* `/usr/bin` = OS commands
* `/usr/sbin` = OS admin commands
* `/usr/local/bin` = manual installs
* Current directory is NOT in PATH (security)

---

> PATH is an environment variable that tells the shell where to look for executable commands, searched in order from left to right.

---
