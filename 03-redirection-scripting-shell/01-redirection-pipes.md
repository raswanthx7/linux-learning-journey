#  Day 10 – Redirection & Pipes

##  Topics Covered:
- Output/input redirection: `>`, `>>`, `<`, `2>`, `&>`, `tee`
- Pipes: `|`
- Command chaining: `;`, `&&`, `||`
- Use cases: logs, backups, chaining scripts

---

##  Key Notes:

- `>` : Redirects **standard output (stdout)** to a file (overwrites).
- `>>` : Appends output to a file **without overwriting**.
- `<` : Takes input from a file.
- `2>` : Redirects **standard error (stderr)** to a file.
- `&>` : Redirects both stdout and stderr to a file.
- `tee` : Displays output and writes it to a file **at the same time**.

- `|` : Takes the output of one command and passes it as input to another (pipe).

- Command chaining:
  - `;` : Run multiple commands **one after another**, regardless of success.
  - `&&` : Run the next command **only if the previous one is successful**.
  - `||` : Run the next command **only if the previous one fails**.

---

##  Commands Practiced:

```bash
# Redirect output to a file (overwrites)
echo "Hello Linux" > file.txt

# Append output to a file
echo "Another line" >> file.txt

# Redirect input from a file
cat < file.txt

# Redirect error to a file
ls wrongfile 2> error.log

# Redirect both output and error
ls /etc /fake  &> all_output.txt

# Use tee to show and write
ls /etc | tee output.txt

# Use pipe to pass output
ps aux | grep ssh

# Command chaining
mkdir test && cd test && touch file.txt
mkdir folder1 || echo "Folder creation failed"
echo "Done"; echo "Next step"
```
___
