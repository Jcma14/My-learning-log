# Personal Learning Log

This repository is my **personal learning log** documenting my journey through **Cybersecurity**, **Networking** and **Quality Assurance (QA) Engineering**.

I use it to track what I learn, reinforce key technical concepts, and organize hands-on knowledge from platforms like **TryHackMe**, **TripleTen**, and self-directed projects. Each section includes summarized explanations, commands, and examples essential for my growth in cybersecurity operations and software quality assurance.

---

## Table of Contents
1. [Linux Basics](#linux-basics)
   - [File and Directory Management](#file-and-directory-management)
   - [Terminal Output and Redirection](#terminal-output-and-redirection)
2. [SSH and Networking](#ssh-and-networking)
3. [Cybersecurity Fundamentals](#cybersecurity-fundamentals)
4. [QA Engineering Notes](#qa-engineering-notes)
5. [Tools and Workflow](#tools-and-workflow)

---

## Linux Basics

### File and Directory Management
```bash
# Create a new directory
$ mkdir folder_name

# Remove an empty directory
$ rmdir Personal

# Remove a file
$ rm index.html

# Remove a directory and its contents (recursive)
$ rm -r Desktop/Personal

# Create new files
$ touch answer.txt
$ touch style.css main.js
```

*Notes:*
Use ```-r``` to remove directories recursively.
Files deleted with ```rm``` do not go to the trash — deletion is permanent.
You cannot delete a directory while you are inside it; move to its parent first.

### Terminal Output and Redirection
```bash
# Display text in the terminal
$ echo "Who is Morty?"

# Write text into a new file
$ echo "Who is Morty?" > secrets.txt

# Overwrite or append text in an existing file
$ echo "Who is Morty?" > ~/logs/2020/1/secrets.txt
```
### Redirecting text between files with cat
```bash
# Display file content
$ cat a.txt
AAA

$ cat b.txt
BBB

# Copy content from one file to another (overwrite)
$ cat a.txt > b.txt
```
After the last command, both ```a.txt``` and ```b.txt``` will contain the same text (```AAA```).

---

## SSH and Networking

### What is SSH
SSH (Secure Shell) is a protocol for secure remote access to another system. It encrypts all communication, protecting passwords, commands, and data transfers.
Common uses:
Remote server access
Secure file transfer (```scp```, ```sftp```)
Port forwarding or tunneling
```bash
# Basic connection syntax
$ ssh user@server_ip
```
### Is SSH Always Secure?
SSH is highly secure when configured properly, but vulnerabilities arise from:
* Weak passwords → use SSH keys.
* Outdated versions → keep OpenSSH updated.
* Not verifying host fingerprints → risk of MITM attacks.
* Default port 22 exposure → change port or limit via firewall.
* Unprotected private keys → use passphrases and permissions.
  
| Setup | Security | Level |
|-------|----------|-------|
| Password | login | Medium |
| Key-based | authentication | Strong |
| Outdated/misconfigured | setup | Weak |

### SSH Ports
* Default SSH port: 22 (TCP)
* Risks: brute-force attacks, scanning
* Best practices:
 * Use key-based authentication
 * Change default port
 * Limit access by IP
 * Use tools like UFW or Fail2Ban
 * Keep SSH updated
