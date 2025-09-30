# Viewing & Editing Files in Linux

## Topics Covered

- Viewing file content: `cat`, `more`, `less`, `head`, `tail`
- File analysis tools: `file`, `stat`, `wc`, `sort`, `cut`, `grep`
- Basic text editors: `vi`, `nano`
- Wildcard patterns: `*`, `?`, `[a-z]`

---

## Viewing Files

```bash
cat filename.txt          # Show full file content
more filename.txt         # View file page-by-page (forward only)
less filename.txt         # View file (scroll up and down)
head filename.txt         # Show first 10 lines
tail filename.txt         # Show last 10 lines
tail -f filename.txt      # Follow file output live (useful for logs)
```

## File Analysis Commands

```bash
file filename.txt         # Identify file type (text, binary, etc.)
stat filename.txt         # Detailed file metadata
wc filename.txt           # Line, word, and byte count
sort file.txt             # Sort contents alphabetically
cut -d',' -f1 data.csv    # Cut first column (with delimiter)
grep "word" filename.txt  # Search for lines containing 'word'
```
## Text Editors

### vi editor

- Press `i` to enter insert mode  
- Press `Esc` to exit insert mode  
- Type `:wq` to save and quit  
- Type `:q!` to quit without saving  

### nano editor

- `Ctrl + O` → Save  
- `Ctrl + X` → Exit  

## Wildcards

```bash
ls *.txt           # Match all files ending in .txt
ls file?.txt       # Match file1.txt, file2.txt, etc.
ls [a-c]*.txt      # Files starting with a, b, or c
```
----
