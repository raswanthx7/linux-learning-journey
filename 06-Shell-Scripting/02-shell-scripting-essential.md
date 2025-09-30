# Shell Scripting Essentials


##  Topics Covered

- Start a script: `#!/bin/bash`
- Variables and Quotes
- `read`, `select` for user input
- Conditional statements: `if`, `else`, `elif`
- Test expressions: `[ ]`, `[[ ]]`, `test` command

---

##  Key Notes

- **`#!/bin/bash`** (called shebang) tells the system to use the bash shell to execute the script.
- **Variables** are assigned using `name=value` (no spaces). You access them with `$name`.
- **Quotes**:
  - Double quotes `" "` allow variable expansion.
  - Single quotes `' '` treat everything literally.
- **`read`** gets user input.
- **`select`** creates simple menus in bash scripts.
- **`[ ]`, `[[ ]]`, and `test`** are used to evaluate conditions:
  - `[ $a -eq $b ]`, `[[ $a == $b ]]`, or `test $a -eq $b`

---

##  Commands Practiced

```bash
# Start a basic script
#!/bin/bash
echo "Hello, Linux!"

# Set and print a variable
username="stark"
echo "Hi, $username"

# User input using read
read -p "Enter your age: " age
echo "You are $age years old"

# Conditional if-else
if [ $age -ge 18 ]; then
  echo "Adult"
elif [ $age -eq 17 ]; then
  echo "Almost there!"
else
  echo "Minor"
fi

# Using test command
if test $age -gt 0; then
  echo "Valid age"
fi

# Using select to create a simple menu
select option in Start Stop Exit; do
  echo "You selected $option"
  break
done
```

---
