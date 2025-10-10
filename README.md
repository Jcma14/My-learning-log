# Personal Learning Log: My Tech Journey

This repository documents my personal journey through **Cybersecurity**, **Networking**, and **QA Engineering**.  
It serves as a central place to store my technical notes, code examples, and reflections as I grow in the IT field.  
These notes come from hands-on labs, formal training, and self-study — continuously updated as I learn.

---

## 🧭 Table of Contents

1. [Linux Fundamentals](#linux-fundamentals)  
2. [SSH & Networking](#ssh-and-networking)  
3. [Cybersecurity Basics](#cybersecurity-basics)  
4. [QA Engineering Bootcamp Notes](#qa-engineering-bootcamp-notes)  
5. [Future Sections](#future-sections)

---

<!-- Start: Linux & Command Line Section -->

<details>
  <summary><h2>Linux Fundamentals</h2></summary>
   
### Directory Navigation

| Command | Meaning | Example |
|----------|----------|----------|
| `.` or no special character | Current directory | `cd ./logs` |
| `..` | Parent directory | `cd ..` |
| `/` | Root directory | `cd /` |
| `~` | Home directory | `cd ~` |

### Linux Command Line Basics
Here’s a list of the most commonly used and essential Linux commands for beginners.  
These help you navigate, manage files, and perform basic operations in the terminal.

| Command | Description | Example |
|----------|--------------|----------|
| `pwd` | Prints the current working directory (where you are) | `pwd` |
| `ls` | Lists files and folders in the current directory | `ls -l` |
| `cd` | Changes the current directory | `cd Documents` |
| `mkdir` | Creates a new directory (folder) | `mkdir new_folder` |
| `rmdir` | Deletes an **empty** directory | `rmdir old_folder` |
| `rm` | Removes files | `rm file.txt` |
| `rm -r` | Removes a directory and its contents recursively | `rm -r folder_name` |
| `cp` | Copies files or directories | `cp file.txt /home/user/Desktop` |
| `mv` | Moves or renames files or directories | `mv file.txt newfile.txt` |
| `touch` | Creates a new empty file or updates its timestamp | `touch notes.txt` |
| `cat` | Displays the contents of a file | `cat file.txt` |
| `echo` | Displays text or writes text to a file | `echo "Hello" > greeting.txt` |
| `man` | Displays the manual for a command (help guide) | `man ls` |
| `clear` | Clears the terminal screen | `clear` |
| `history` | Shows a list of recently used commands | `history` |
| `whoami` | Prints your current username | `whoami` |
| `uname -a` | Displays system and kernel information | `uname -a` |
| `df -h` | Shows disk space usage | `df -h` |
| `du -sh` | Displays the size of the current directory | `du -sh` |
| `top` | Shows running processes and system resource usage | `top` |
| `sudo` | Executes a command as a superuser (admin privileges) | `sudo apt update` |
| `apt update` | Updates the list of available packages | `sudo apt update` |
| `apt upgrade` | Installs available package upgrades | `sudo apt upgrade` |
| `exit` | Closes the terminal or SSH session | `exit` |

💡 **Tip:** Use `--help` after any command (for example, `ls --help`) to quickly see its available options and usage examples.

### Linux File Permissions & Ownership

Linux is a multi-user system where every file and directory has permissions that define **who can read, write, or execute** it.  
Understanding and managing permissions is essential for security and proper system administration.

| Command | Description | Example |
|----------|--------------|----------|
| `ls -l` | Lists files in long format, showing permissions and ownership | `ls -l` |
| `chmod` | Changes file or directory permissions (read, write, execute) | `chmod 755 script.sh` |
| `chown` | Changes the ownership of a file or directory | `sudo chown user:group file.txt` |
| `chgrp` | Changes the group ownership of a file or directory | `sudo chgrp developers project/` |
| `umask` | Sets default permission settings for new files | `umask 022` |

### File Search & Inspection Commands

| Command   | Description                                      | Example                               |
| --------- | ------------------------------------------------ | ------------------------------------- |
| `find`    | Search for files and directories                 | `find /home -name notes.txt`          |
| `grep`    | Search for text inside files                     | `grep "error" logfile.txt`            |
| `cat`     | Display file contents                            | `cat notes.txt`                       |
| `less`    | View file contents one screen at a time          | `less notes.txt`                      |
| `head`    | Show the first 10 lines of a file                | `head notes.txt`                      |
| `tail`    | Show the last 10 lines of a file                 | `tail notes.txt`                      |
| `tail -f` | Continuously monitor new lines (useful for logs) | `tail -f /var/log/syslog`             |
| `wc`      | Count lines, words, and characters               | `wc notes.txt`                        |
| `du`      | Show disk usage of files/directories             | `du -h /home/user`                    |
| `df`      | Show available disk space                        | `df -h`                               |
| `stat`    | Display detailed file information                | `stat notes.txt`                      |
| `sort`    | Sort lines of text                               | `sort names.txt`                      |
| `uniq`    | Remove duplicate lines (often used with `sort`)  | `sort names.txt \| uniq`              |
| `cut`     | Extract columns or fields from text              | `cut -d':' -f1 /etc/passwd`           |
| `awk`     | Advanced text processing and reporting           | `awk '{print $1}' data.txt`           |
| `sed`     | Stream editor — modify text in files             | `sed 's/error/warning/g' logfile.txt` |

### Creating Directories

You can create a new directory using the `mkdir` command:

```bash
$ mkdir folder_name
# creates a new directory called folder_name
```

### Removing Empty Directories
Use `rmdir` to delete empty directories:
```bash
$ rmdir Personal
# removes an empty folder named Personal
```
### Removing Files and Directories
To remove files:
```bash
$ rm index.html
# deletes the file index.html
```
To remove a directory (and its contents) recursively:
```bash
$ rm -r Desktop/Personal
# deletes the directory 'Personal' inside 'Desktop' and all its contents
```
⚠️ Important:
When using the command line, double-check your path before deleting files — deletions are permanent (they don’t go to Trash).

### Creating Files
Use the `touch` command to create one or more new files:
```bash
$ touch answer.txt
# creates a new file named answer.txt
```
You can create multiple files at once:
```bash
$ touch style.css main.js
# creates two files: style.css and main.js
```

### Displaying Text in Terminal
The `echo` command prints text in the terminal
```bash
$ echo "Who is Morty?"
Who is Morty?
# prints "Who is Morty?" to the terminal
```
You can also write text to a file:
```bash
$ echo "Who is Morty?" > secrets.txt
# creates a file 'secrets.txt' and writes the line inside it
```
To overwrite or add text to an existing file:
```bash
$ echo "Who is Morty?" > ~/logs/2020/1/secrets.txt
# writes the line to the specified file, overwriting its contents
```

### Redirecting File Output
You can redirect text from one file to another using `cat` and redirection operators.

```<``` → input redirection

```>``` → output redirection

Example:
```bash
$ cat a.txt
AAA
# displays content of a.txt

$ cat b.txt
BBB
# displays content of b.txt

$ cat a.txt > b.txt
# copies content of a.txt into b.txt, overwriting its content
```
After this, both files contain:
```bash
AAA
```

### Appending File Content with `cat` and `>>` Operator
In this example, the content of a.txt is copied to the end of b.txt.
The >> operator is used to append data instead of overwriting it.
```bash
$ cat a.txt >> b.txt
# the content of a.txt was copied to b.txt

$ cat b.txt
BBB
AAA
# a.txt content was appended to b.txt without overwriting existing data
```

### Copying Files and Directories with `cp`
The `cp` (copy-paste) command copies a directory or file. After the command, you must specify:
1. The name and path of the source file.
2. The name and path of the destination file.

If the file is copied from or to the current directory, you can omit the path.

Examples:
``` bash
$ cp brothers.html sisters.html
# copied the file brothers.html and named the new copy sisters.html
# both files are in the current folder

$ cp ../docs/brothers.html sisters.html
# specified the path to the source file brothers.html
# the copy is saved in the current folder as sisters.html

$ cp ../docs/brothers.html ../Documents/
# copied brothers.html to the Documents directory
```
To copy an entire directory, use the `-r` flag.

### Moving and Renaming Files and Directories with `mv`
The `mv` (move) command moves a folder or file and works similarly to the `cp` command: you specify the file name and the destination path. Always specify the path to the new location.

Examples:
```bash
$ mv card.txt ~/
# card.txt moved from the current directory to the home directory

$ mv card.txt /home/logs/2020/
# card.txt moved using an absolute path
```
The `mv` command can also rename a file. To do this, provide the new file name as the second argument.
```bash
$ mv my_app.ssh your_app.ssh
# renamed my_app.ssh to your_app.ssh

$ mv /home/logs/2020/card.txt /home/logs/2020/cards.txt
# moved card.txt and renamed it to cards.txt
# absolute path specified
```
If you are already in the directory where you want to move or copy a file, you can omit the source path and just specify the filename.

### Filtering File Content with `grep`
When you have many log files in a directory and only one of them contains an error, you can use the `grep` command (Global Regular Expression Print) to search for specific text or patterns across files.

'grep' is a file search utility that finds and displays lines matching a given expression.
If no file is specified, it searches across all files in the current directory.

Syntax:
```bash
$ grep [Flag(s)] PATTERN [Address]
```
- `PATTERN` → the text you’re looking for (one or more words).
   - Single word: `Frodo`
   - Multiple words: `"Frodo Baggins"` (use quotes)
   
- `[Address]` → the file or directory path.

- `Flag(s)]` → optional arguments that modify the search behavior (e.g., ignore case, recursive search).

### Common Flags for `grep`
`-R` — Recursive Search:
The `-R` flag tells grep to search through all files and subdirectories.
```bash
$ grep -R DELETE ~/logs/2020/1
```
`-n` — Show Line Numbers:
The `-n` flag displays the line number where each match occurs.
```bash
$ grep -n DELETE apache_2020-01-01.txt
```
#### Example: Search for a String and Display Line Number
You are in your home directory. Go to `~/logs/2020/1`.

Find the line number that contains `"503 3312"` in the file `apache_2020-01-01.txt`.
```bash
$ grep -n "503 3312" apache_2020-01-01.txt
```
#### Output:
```bash
1583
```
✅ The string `"503 3312"` appears on line 1583.
</details>

<!-- End: Linux & Command Line Section -->

---

<!-- Start: Networking Section -->

<details>
  <summary><h2>SSH AND NETWORKING</h2></summary>

### What is SSH?
SSH (Secure Shell) is a network protocol used to securely access another computer remotely.
It encrypts all communication between client and server.

### Main uses:

Remote access to servers or devices.
Executing commands securely.
Transferring files (`scp`, `sftp`).
Tunneling other network traffic.
### Connect to a server:
```bash
ssh user@server_ip
```
### SSH Keys:
* Private key stays on the client.
* Public key goes on the server.

They provide secure passwordless authentication.
### Is SSH Always Secure?
SSH is very secure when properly configured, but risks include:

* Weak passwords → use SSH keys instead.
* Outdated SSH versions → keep OpenSSH updated.
* Not verifying host fingerprints → risk of MITM attacks.
* Exposed port 22 → use a non-standard port or firewall.
* Stolen private keys → protect with passphrases.
  
| Setup                      | Security Level |
| -------------------------- | -------------- |
| Password login             | Medium         |
| Key-based authentication   | Strong         |
| Outdated/misconfigured SSH | Weak           |


### SSH Ports

* Default SSH port: TCP 22

#### Risks:
* Brute-force attacks → disable password logins.
* Port scanning → change default port or restrict via firewall.
* Outdated versions → keep updated.

#### Best practices:
* Use key-based authentication.
* Change default port.
* Limit IP access.
* Use tools like Fail2Ban or UFW.

### Ports on the Client Side
#### When connecting via SSH:
* The client does not open permanent ports.
* It uses a temporary ephemeral port for outbound connection (e.g., Client:54321 → Server:22).
* The port closes automatically when the session ends.

#### Exceptions:
Ports open locally only when port forwarding is used.

| Mode | Description        | Open Port                         |
| ---- | ------------------ | --------------------------------- |
| `-L` | Local forwarding   | Opens local port (localhost only) |
| `-R` | Remote forwarding  | Opens port on remote server       |
| `-D` | Dynamic forwarding | Opens local SOCKS proxy port      |

### Disconnecting from SSH
To disconnect cleanly:
``` bash
exit
```
If the session freezes:
```bash
~.
```
If the terminal is closed, the SSH session ends immediately.

**Best practice**: always use `exit`.
</details>

<!-- End: Networking Section -->
---

<!-- Start: Cybersecurity Section -->

<details>
  <summary><h2>Cybersecurity Basics</h2></summary>
   
### Overview
Core cybersecurity principles learned through TryHackMe and self-study:

* Understanding security principles, governance, and regulation.
* The Cyber Kill Chain model for analyzing attack stages.
* Fundamental defensive practices (updates, authentication, network segmentation).
</details>

<!-- End: Cybersecurity Section -->

---

<!-- Start: Bootcamp Section -->

<details>
  <summary><h2>QA Engineering Bootcamp Notes</h2></summary>

**Training program**: TripleTen QA Engineer Bootcamp (LATAM)

Focus areas:

* Manual testing & test case design
* Bug reporting and documentation
* Python-based test automation
* API and UI testing fundamentals
* Practical projects (e.g., Urban Routes, Urban Lunch, Urban Grocers)
</details>

<!-- End: Bootcamp Section -->

---

<!-- Start: Python Section -->

<details>
  <summary><h2>Python</h2></summary>

### 🐍 Python Cheat Sheet (Courtesy of Jack Rhysider)

This Python cheat sheet was created by **Jack Rhysider**, the host of [**Darknet Diaries**](https://darknetdiaries.com/), and is shared here for **educational and personal learning purposes**.

> 📝 **Credit:** Original creator — [Jack Rhysider](https://darknetdiaries.com/)  
> 📄 **Source:** *Python Cheat Sheet (Darknet Diaries)*  
> ⚖️ **Note:** All rights belong to the original author. This resource is included here solely for study and reference.

[📘 View the Python Cheat Sheet (by Jack Rhysider)](./assets/Python-CheatSheet.pdf)
</details>

<!-- End: Python Section -->
---

<!-- Start: Future Sections -->

<details>
  <summary><h2>Future Sections</h2></summary>
   
These placeholders will be filled as I continue learning:

* Python for Automation
* Git & GitHub Workflow
* Networking Deep Dive
* Kali Linux Tools
* Advanced QA Techniques
* Cloud & Virtualization Basics
</details>



