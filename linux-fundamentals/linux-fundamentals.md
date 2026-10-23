# 🐧 Linux Fundamentals — TryHackMe

[← Back to Learning Log](../README.md)

**Source:** TryHackMe — Linux Fundamentals  
**Topics:** Core commands, file searching, shell operators

Linux is the standard operating system in cybersecurity. Kali Linux, most servers, and most CTF environments run on it. These are the commands used every day in the terminal.

---

### Core Commands

| Command | What it does | Example |
|---|---|---|
| `echo` | Print text to the screen | `echo "hello"` |
| `whoami` | Show which user you're logged in as | `whoami` |
| `ls` | List files and folders | `ls -la` |
| `cd` | Move into a folder | `cd Documents` |
| `pwd` | Show full path of current location | `pwd` |
| `cat` | Print a file's contents | `cat passwords.txt` |
| `find` | Search for files by name or type | `find -name passwords.txt` |
| `grep` | Search inside files for text | `grep "admin" access.log` |
| `wc` | Count lines, words, or characters | `wc -l access.log` |
| `mkdir` | Create a new folder | `mkdir my_folder` |
| `rm` | Remove a file | `rm file.txt` |
| `rm -r` | Remove a folder and its contents | `rm -r folder_name` |
| `cp` | Copy a file | `cp file.txt /backup/` |
| `mv` | Move or rename a file | `mv old.txt new.txt` |

---

### Using `find`

`find` searches the filesystem for files. Useful when you know a filename but not where it is.

```bash
# Find a specific file by name
find -name passwords.txt

# Find all files with a certain extension
find -name *.txt

# Search entire system, suppress permission errors
find / -name flag.txt 2>/dev/null
```

> 💡 In CTFs, `find` is essential for hunting flag files hidden deep in a filesystem. The `2>/dev/null` part hides "Permission denied" errors so results are clean.

---

### Using `grep`

`grep` searches **inside** files for a specific word or pattern — much faster than reading files manually.

```bash
# Find a specific IP in a log file
grep "81.143.211.90" access.log

# Case-insensitive search
grep -i "admin" config.txt

# Search recursively through all files in a folder
grep -r "password" /etc/

# Show line numbers with results
grep -n "error" app.log
```

---

### Shell Operators

Operators let you chain commands and control where output goes.

| Operator | What it does | Example |
|---|---|---|
| `\|` | Pipe — sends output of one command into the next | `strings file \| grep -i "pico"` |
| `>` | Redirect output to a file (overwrites) | `echo "hello" > file.txt` |
| `>>` | Redirect output to a file (appends) | `echo "world" >> file.txt` |
| `&&` | Run second command only if first succeeds | `mkdir dir && cd dir` |
| `&` | Run command in the background | `cp large_file /backup/ &` |

> ⚠️ `>` silently overwrites existing files. Always use `>>` if you want to keep existing content.

---

### File Permissions

Every file in Linux has permissions that define who can read, write, or execute it.

```bash
ls -l                          # View permissions
chmod 755 script.sh            # Set rwx for owner, rx for others
chown user:group file          # Change file ownership
```

Permission format: `-rwxr-xr-x`
- First character: file type (`-` = file, `d` = directory)
- Next 3: owner permissions (rwx)
- Next 3: group permissions (r-x)
- Last 3: everyone else (r-x)
