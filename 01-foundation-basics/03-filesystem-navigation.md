# File System Navigation

## Topics Covered

- file system structure:`/`,`/bin`,`/dev`,etc.
- Understanding absolute and relative paths
- Navigating directories using `cd`
- Viewing current directory using `pwd`
- Listing contents with `ls` and options
- Path structure: `/`, `/home`, `/root`, `/etc`, etc.
- Hidden files and file types

---

## My Notes

- The Linux **file system is hierarchical**, starting from the root `/`.
- Linux File System Structure (Simple Explanation)

   -  `/` → Root directory
The top of the Linux file system. Everything starts from here.

   - `/bin` → Basic user commands
Stores everyday commands like ls, cp, mv, cat.

   - `/sbin` → System commands
Stores admin/system commands like shutdown, reboot, fdisk.

   - `/etc` → Configuration files
Stores system and application settings. Example: network configs, SSH configs.

   - `/home` → User personal space
Stores personal files for each user. Example: /home/john.

   - `/root` → Root user’s home
Stores files for the root (administrator) user.

   - `/var` → Variable files
Stores changing data like logs, mail, cache. Example: /var/log/messages.

   - `/tmp` → Temporary files
Stores short-term files. Cleared after reboot.

   - `/usr` → User programs
Stores installed software, manuals, and libraries for applications.

   - `/usr/bin` → Extra commands
Stores user application commands like vim, python3.

   - `/usr/sbin` → Extra system commands
Stores system tools not needed for boot, like apache2, nginx.

   - `/lib` → Shared libraries (toolbox for programs)
Stores collections of pre-written code (.so files) used by system commands in /bin and /sbin.

   - `/lib64` → 64-bit libraries
Stores 64-bit versions of shared libraries.

   - `/boot` → Boot files
Stores kernel, bootloader (GRUB), and files needed to start the system.

   - `/dev` → Device files (hardware as files)
Stores special files that represent hardware. Example: /dev/sda = hard disk.

   - `/mnt` → Temporary mount point
Stores temporarily mounted file systems (like extra drives).

   - `/media` → Removable devices
Stores USB drives, CDs, DVDs when mounted.

   - `/opt` → Optional software
Stores third-party or optional applications.

   - `/srv` → Service data
Stores data for services like web servers (e.g., /srv/www).

   - `/proc` → Process information (virtual files)
Stores info about running processes and system resources. Example: /proc/cpuinfo.

   - `/sys` → System and device info
Stores kernel info about hardware devices.

   - `/run` → Runtime data
Stores process IDs, sockets, and runtime info after the system boots.
- Every file and folder is part of one big **directory tree**.
- **Absolute path**: starts from the root  
  Example: `/home/raswanth/linux-learning`
- **Relative path**: starts from your current directory  
  Example: `../scripts` or `./Day01.md`

---

## Commonly Used Commands

```bash
pwd                    # Show current working directory
ls                     # List files and folders
ls -F                  #Adds symbols to file types
ls -l                  # Long format (permissions, owner, size)
ls -a                  # Show hidden files (those starting with .)
cd /path/to/folder     # Go to specific directory (absolute)
cd ..                  # Move one directory up
cd                     # Go to home directory
cd -                   # Switch to previous directory

