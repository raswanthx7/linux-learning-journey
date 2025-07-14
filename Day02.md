# Day 02 – Linux Architecture & Shell

## Topics Covered

- Linux System Architecture Overview
- Role of Kernel, Shell, and Terminal
- User Space vs Kernel Space
- Different types of Shells (bash, sh, zsh, etc.)
- How commands reach the kernel (process flow)
- Interaction between hardware, OS, and users

---

## My Notes

- Linux has a **modular architecture** consisting of:
  - **Hardware**
  - **Kernel**
  - **Shell**
  - **Utilities and Applications**
  - **User Interface (CLI)**

- **Kernel**: The core part of the OS, responsible for managing hardware and system processes (memory, CPU, etc.).

- **Shell**: The interface between the user and the kernel. It interprets and executes user commands. Most commonly used shell is `bash`.

- **Terminal**: The environment where the shell runs. It accepts user input and displays output.

- Linux uses a layered structure:
  - Users interact with the **shell** through the **terminal**
  - The shell sends system calls to the **kernel**
  - The kernel interacts with the **hardware**

---

## Commands Practiced

```bash
echo $SHELL               # Shows the current default shell
cat /etc/shells           # Lists all available shells
ps -p $$                  # Displays the shell process
uname -a                  # Shows complete system architecture

