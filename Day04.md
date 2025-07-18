# Day 04 – File & Folder Operations

## Topics Covered

- Advanced `ls` command options
- Directory navigation with `cd`
- Creating and removing files/folders
- Using `touch`, `mkdir`, `rmdir`, `echo`
- File redirection (`>`, `>>`)
- Creating multiple files/folders
- `touch` and `echo` options

---

## My Notes

### `ls` Command Options

```bash
ls -l        # Long listing (permissions, owner, size, date)
ls -a        # Show hidden files (those starting with .)
ls -lh       # Human-readable file sizes (KB, MB)
ls --color   # Adds color to distinguish files/folders
ls -ld dir   # Show directory info (not contents)
ls -ltr      # Sort by modification time, reverse order
```

### `cd` Variations

```bash
cd           # Go to home directory
cd ..        # Go one directory up
cd ~         # Go to home directory (same as cd)
cd -         # Switch to previous directory
cd /folder   # Go to specific folder (absolute path)
```

### Creating & Removing Files and Folders

```bash
mkdir foldername                 # Create a folder
mkdir folder1 folder2 folder3   # Create multiple folders

rmdir foldername                # Remove empty folder

touch file1.txt                 # Create a file
touch file1.txt file2.txt       # Create multiple files
```

### `touch` Command Options

```bash
touch -c file.txt   # Don’t create the file if it doesn’t exist
touch -t 202507161200 file.txt   # Set custom timestamp (YYYYMMDDhhmm)
touch -a file.txt   # Update only access time
touch -m file.txt   # Update only modification time
```

### `echo` Usage and Redirection

```bash
echo Hello World              # Print text to terminal
echo -n Hello                 # No newline after output
echo -e "Line1\nLine2"        # Enable escape characters

echo "Linux is cool" > file.txt      # Write (overwrite) to file
echo "Add this line" >> file.txt     # Append to file
```

---
