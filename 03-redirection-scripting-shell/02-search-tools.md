# Day 11 – Hostnames, Prompts & Profiles

##  Topics Covered:
- Setting the system hostname using `hostnamectl`
- Customizing the shell prompt using `PS1`
- Making prompt changes permanent via `.bashrc` or `.bash_profile`
- Environment variables: `env`, `printenv`, `export`
- Using `source` to apply changes

---

##  Key Notes:

- `hostnamectl` is used to view and set the hostname of your system.
  - Example: `hostnamectl set-hostname mylinuxvm`
  - Use `hostnamectl` or `hostname` to verify.

- `PS1` is an environment variable that controls the appearance of your shell prompt.
  - Example: `PS1="\u@\h:\w\$ "` → Displays username@hostname:current-directory$

- `.bashrc` and `.bash_profile` are startup configuration files for Bash shell.
  - Changes in `~/.bashrc` affect interactive shell sessions.
  - Use `source ~/.bashrc` to apply the changes immediately.

- `env` and `printenv` display current environment variables.
  - Example: `printenv PATH` shows the path variable.

- `export` is used to define and export environment variables.
  - Example: `export NAME=raswanth`

---

##  Commands Practiced:

```bash
hostnamectl set-hostname mylinuxvm       # Set hostname
hostnamectl                              # Check hostname
echo $PS1                                # View current prompt variable
PS1="\u@\h:\w\$ "                         # Temporary prompt change
source ~/.bashrc                         # Reload bash config
printenv                                 # Show all environment variables
printenv PATH                            # Show value of PATH variable
export MYVAR=Hello                       # Create a new environment variable
env                                      # List all environment variables
```

---
