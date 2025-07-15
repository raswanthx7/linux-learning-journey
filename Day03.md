# Day 03 – File System Navigation

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

