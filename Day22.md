#  Day 22 – Shell Scripting


##  Topics Covered:

- What is a shell script?
- Difference between manual commands and scripts
- Bash shell and scripting use cases
- Shebang (`#!/bin/bash`) meaning
- Make script executable with `chmod +x`
- How to execute `.sh` files
- Variables and user inputs in scripts
- Standard input/output (stdin, stdout)
- if/else conditions with real examples
- Loops and automation use cases

---

##  Key Notes:

- **Shell Script**: A file containing a sequence of commands to automate tasks.
- **Manual vs Script**: Manual = run each command by hand; Script = run once, automates everything.
- **Bash**: Most commonly used shell; default in most Linux systems.
- **Shebang (`#!/bin/bash`)**: Tells system to run the script using bash.
- **Executable script**: Use `chmod +x script.sh` to make the script executable.
- **Run a script**: Use `./script.sh` to execute.
- **Variables**: Store values. Example: `name="Stark"`
- **User Input**: Use `read` to accept input. 
Example:
  ```bash
  read -p "Enter your name: " name
  ```
 ###  Conditions (if/else)

Used to control logic based on conditions:

```bash
age=20
if [ $age -ge 18 ]; then
  echo "Adult"
else
  echo "Minor"
fi
```
### Loops
Used to automate repeated tasks.

```bash
#Example – Restart multiple services:
for service in nginx sshd cron; do
  systemctl restart $service
done
```
## Commands Practiced

```bash
# Make script executable
chmod +x backup.sh

# Run the script
./backup.sh

# Set variable and print
name="Stark"
echo "Hello, $name"

# Accept input from user
read -p "Enter filename: " fname

# If condition example
if [ -f "$fname" ]; then
  echo "File exists"
else
  echo "File not found"
fi

# Loop example
for i in 1 2 3; do
  echo "Number: $i"
done
```
---
