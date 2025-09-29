# Introduction to Linux

## Topics Covered:
- What is Linux?
- Why Linux is used in the cloud
- Linux vs Windows (basic comparison)
- Linux distributions (RHEL, Ubuntu, Debian, Fedora, etc.)
- Kernel, Shell, Terminal
- What happens when Linux boots?

---

## My Key Notes

- **Linux** is a **free**, **open-source**, and **Unix-based** operating system. It is highly stable and secure.
- Most **cloud platforms** like **AWS**, **Azure**, and **GCP** use Linux as the default OS for their virtual machines.
- Compared to Windows, Linux relies heavily on the **Command Line Interface (CLI)**, which provides better control and automation.
- **Popular distributions**:
  - **RHEL (Red Hat Enterprise Linux)** – used in corporate environments
  - **Ubuntu** – user-friendly, good for beginners and developers
  - **CentOS**, **Rocky**, **AlmaLinux** – compatible with RHEL
- **Kernel** – the core of the OS that communicates with hardware
- **Shell** – the command-line interpreter (e.g., Bash)
- **Terminal** – where users input commands
- **Linux Boot Process** (in order):
  1. **BIOS/UEFI**
  2. **GRUB** (bootloader)
  3. **Kernel** (OS core)
  4. **Init/systemd** (first process)
  5. **Shell** (user interface)

---

## Commands Practiced

```bash
uname -r               # Show the Linux kernel version
cat /etc/os-release    # Display Linux distribution info
echo $SHELL            # Show the current shell

