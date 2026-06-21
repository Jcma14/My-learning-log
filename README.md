<div align="center">

# 🛡️ Personal Learning Log: Cybersecurity Journey

<img src="assets/Neon Green and Black Tech Virtual Background.png" width="90%" alt="Tech banner"/>

### Transitioning into Cybersecurity | QA Engineering Background | ECU Master of Cyber Security (2026)

This repository documents my hands-on journey into cybersecurity, built on a foundation of QA engineering and software testing.  
Notes come from **TryHackMe labs**, **CTF platforms**, **home lab work**, and **self-study** — written to be clear, practical, and useful for anyone learning the same material.  
Continuously updated as I progress.

---

### 🧠 Tech Stack & Tools

![Cybersecurity](https://img.shields.io/badge/Cybersecurity-darkred?style=for-the-badge&logo=kalilinux&logoColor=white)
![TryHackMe](https://img.shields.io/badge/TryHackMe-212C42?style=for-the-badge&logo=tryhackme&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-0E83CD?style=for-the-badge&logo=nmap&logoColor=white)
![Python](https://img.shields.io/badge/Python-14354C?style=for-the-badge&logo=python&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![SQL](https://img.shields.io/badge/KQL%20%2F%20SQL-336791?style=for-the-badge&logo=microsoftazure&logoColor=white)
![QA](https://img.shields.io/badge/QA%20Engineering-blueviolet?style=for-the-badge&logo=testcafe&logoColor=white)

</div>

---

## 📚 Contents

| Section | Topics | Source |
|---|---|---|
| [🐧 Linux Fundamentals](#-linux-fundamentals--tryhackme) | Core commands, find, grep, shell operators | TryHackMe |
| [🌐 Network Services](#-network-services--tryhackme) | SMB, Telnet — theory, enumeration, exploitation | TryHackMe |
| [🗺️ Nmap — Network Scanning](#️-nmap--network-scanning) | Scan types, flags, NSE scripts, recipes | TryHackMe + NetworkChuck |
| [🧪 CTF Labs — CyLab / picoCTF](#-ctf-labs--cylab--picoctf) | Flags, CyberChef, Linux tools, Python | CyLab Security Academy |
| [🔍 KC7 — Threat Hunting with KQL ](#-kc7--threat-hunting-with-kql) | KQL queries, SOC investigations — *A Rap Beef: An Intro to Security Investigations* | KC7 / CyLab |
| [🛠️ QA Engineering Reference](#️-qa-engineering-reference) | Linux CLI, Python, Pytest, API testing | TripleTen Bootcamp |

---

<!-- ============================================================ -->
<!-- LINUX FUNDAMENTALS -->
<!-- ============================================================ -->

<details>
  <summary><h2>🐧 Linux Fundamentals — TryHackMe</h2></summary>

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

</details>

---

<!-- ============================================================ -->
<!-- NETWORK SERVICES -->
<!-- ============================================================ -->

<details>
  <summary><h2>🌐 Network Services — TryHackMe</h2></summary>

**Source:** TryHackMe — Network Services  
**Topics:** SMB and Telnet — understanding, enumeration, and exploitation

---

### SMB — Server Message Block

**SMB** is a protocol that lets computers on the same network share resources — files, folders, and printers. It works on a **client-server model** over **TCP port 445**.

Linux machines use **Samba**, an open-source implementation of SMB, to communicate with Windows systems in mixed environments.

**Why SMB matters in security:**
- **EternalBlue (MS17-010)** — the exploit behind the 2017 WannaCry ransomware attack, which spread globally via SMB
- **Misconfigured shares** — servers that allow anonymous access, meaning anyone can connect without a password and browse sensitive files
- In penetration testing, SMB is one of the first services checked — an open share can give an attacker a foothold into an entire network

---

### Enumerating SMB

Enumeration means gathering as much information as possible before attempting any attack.

**Step 1 — Confirm SMB is running:**
```bash
nmap -A -Pn -T4 -p- <IP>
# Look for port 445 or 139 to be open
```

**Step 2 — Enumerate with Enum4Linux:**

Enum4Linux is a tool built to extract information from SMB servers.

```bash
# Full enumeration (recommended starting point)
enum4linux -a <IP>

# Just list shares
enum4linux -S <IP>

# Just list users
enum4linux -U <IP>

# Check password policy
enum4linux -P <IP>
```

**What to look for in the output:**
- Share names — especially `files`, `backup`, `data`, `public`
- Shares showing `Mapping: OK, Listing: OK` — these allow anonymous access
- Usernames — real accounts usable in later attacks
- Password policy weaknesses — no minimum length, no lockout threshold

---

### Exploiting SMB — Anonymous Access

The most common SMB vulnerability isn't a complex exploit — it's misconfiguration. Many servers allow **anonymous access**: connecting without any credentials.

**Connect to a share anonymously using SMBClient:**
```bash
smbclient //<IP>/<SHARENAME> -U Anonymous -p 445
# When prompted for a password → press Enter (leave blank)
```

**Commands inside the SMB shell:**

| Command | What it does |
|---|---|
| `ls` | List files in current directory |
| `cd <folder>` | Move into a folder |
| `get <file>` | Download a file to your local machine |
| `put <file>` | Upload a file |
| `exit` | Disconnect |

```bash
smb: \> ls
smb: \> cd confidential
smb: \> get passwords.txt
```

> 💡 As a Security Engineer, finding an anonymously accessible SMB share is a **critical finding**. It means anyone on the network could read sensitive documents, credentials, or backups — and it's often the first step toward full network compromise.

---

### SSH — Secure Shell & Remote Access

**SSH (Secure Shell)** is the standard protocol for securely connecting to and controlling remote machines over a network. It replaced Telnet by encrypting everything in transit — including credentials. Understanding SSH is foundational to both offensive and defensive security work.

**How it works:**  
SSH uses public-key cryptography for the handshake and can authenticate via password or SSH key pair. A key pair consists of a private key (stays on your machine) and a public key (placed on the server). Key-based authentication is significantly more secure than passwords.

**Connecting via SSH:**
```bash
# Default connection (uses port 22)
ssh username@ip_address

# Connect on a custom port
ssh username@ip_address -p 2222

# Connect using a specific key file
ssh -i ~/.ssh/my_key username@ip_address
```

**Why you sometimes need a port number:**  
SSH defaults to port 22 — you only need to specify `-p` when the server runs SSH on a non-default port. This is a common hardening practice: moving SSH off port 22 reduces automated bot scanning significantly. It's also required when port forwarding maps an external port to an internal machine's port 22.

---

### Private vs Public IP Addresses

One of the most important concepts in network security is understanding which machines are reachable from the internet — and which aren't.

| IP Type | Reachable from internet | Example ranges |
|---|---|---|
| **Private** | ❌ No | `192.168.x.x`, `10.x.x.x`, `172.16–31.x.x` |
| **Public** | ✅ Yes | Assigned by your ISP |

Private IP addresses are reserved ranges the internet is designed to ignore. Routers will never forward them outside a local network. This means a machine on `192.168.x.x` has **no internet attack surface** — bots scanning for open SSH ports will never find it, because there's no path to it from outside.

This is why moving SSH off port 22 matters on a public-facing server, but is unnecessary for a VM on a home lab with a private IP.

---

### Accessing a Private Home Network Remotely

If your machines are on a private network, you can't SSH into them directly from another location. There are three main approaches:

**1. WireGuard VPN (Recommended)**  
A VPN creates an encrypted tunnel between your laptop and your home network. With WireGuard running on a Raspberry Pi (via PiVPN), you connect from anywhere and your device behaves as if it's physically on your home network.

- Only one port exposed to the internet: UDP 51820 (encrypted)
- Once connected, access everything on the home network normally
- WireGuard is modern, fast, and far simpler to configure than older VPNs like OpenVPN
- This is the correct approach for a home lab: minimal exposure, maximum access

**2. Port Forwarding**  
You configure your home router to forward an external port to a specific internal machine. Works, but exposes that port to the internet — meaning bots will find and probe it. Requires proper SSH hardening (key auth, non-default port, fail2ban) to be safe.

**3. Reverse SSH Tunnel**  
The remote machine connects *out* to a cloud server (VPS), creating a tunnel you can reach back through. Useful when you can't control the router. Requires a VPS.

```bash
# On the remote machine — initiate the outbound tunnel
ssh -R 2222:localhost:22 user@your-vps.com

# From anywhere — connect back through the VPS
ssh -p 2222 user@your-vps.com
```

**Why this matters in cybersecurity:**  
SSH is one of the most targeted services on the internet. Credential brute-force attacks against SSH on public port 22 are constant and automated. Understanding private vs public IP exposure, default port risks, and how to securely enable remote access is core knowledge for any security role — both for defending infrastructure and for identifying attack vectors during penetration testing or red team engagements.

---

### Telnet — Enumeration

Telnet is an old remote access protocol that sends all data — including passwords — in **plain text**. It's largely been replaced by SSH, but still appears in legacy systems, CTFs, and misconfigured environments.

**Enumerate with a thorough Nmap scan:**
```bash
nmap -A -Pn -T4 -p- <IP>
```

| Flag | What it does | Why use it |
|---|---|---|
| `-A` | Aggressive — OS detection, version detection, scripts, traceroute | Maximum info in one command |
| `-Pn` | Skip ping — scan even if host appears offline | Hosts that block ICMP will show as "down" without this |
| `-T4` | Timing level 4 — faster scan | Good balance of speed and accuracy |
| `-p-` | All 65,535 ports | Finds services on non-standard ports |

**What to look for:**
- Any open port — Telnet might be on port 23 or something unexpected like 8012
- **Service banners** — text the service sends when you connect, often revealing software name, version, or hints about how to proceed
- OS detection results

```bash
# Once you find an open port, connect directly
telnet <IP> <PORT>

# Or use netcat to grab the banner
nc <IP> <PORT>
```

> 💡 **Banners are valuable.** When a service responds with text upon connection, that banner often tells you exactly what software is running — and sometimes hints at how to proceed. Never ignore it.

</details>

---

<!-- ============================================================ -->
<!-- NMAP -->
<!-- ============================================================ -->

<details>
  <summary><h2>🗺️ Nmap — Network Scanning</h2></summary>

**Source:** TryHackMe — Further Nmap · NetworkChuck Tutorial  
**Topics:** Port scanning, scan types, key flags, NSE scripts

In cybersecurity, **you never attack blind**. Nmap (Network Mapper) is the industry-standard tool for reconnaissance — scanning a target's ports, identifying open services, detecting operating systems, and finding known vulnerabilities.

> Every port is like a door. Nmap walks around the building, knocks on every door, and reports back: "this one's open, this one's locked, this one has a firewall in front of it." That report is your map before the real work starts.

---

### Understanding Ports

Every computer has **65,535 ports**. Each port is a numbered channel where a specific service listens for connections.

| Port | Service |
|---|---|
| 22 | SSH |
| 23 | Telnet |
| 80 | HTTP |
| 443 | HTTPS |
| 139 / 445 | SMB (file sharing) |
| 3389 | RDP (Windows Remote Desktop) |

**Port states Nmap reports:**

| State | Meaning |
|---|---|
| `open` | A service is actively listening |
| `closed` | Port reachable but nothing is listening |
| `filtered` | A firewall is blocking Nmap — can't determine state |

---

### Scan Types

| Flag | Scan Type | Notes |
|---|---|---|
| `-sT` | TCP Connect | Full 3-way handshake. Reliable but logged. No sudo needed. |
| `-sS` | SYN / Stealth | Sends SYN, gets SYN/ACK, sends RST to abort. Never completes handshake. Faster, less likely to be logged. **Requires sudo.** Default when running as root. |
| `-sU` | UDP | Scans UDP ports. Much slower than TCP. |
| `-sN` | TCP Null | No flags set. Evades some firewalls. |
| `-sF` | TCP FIN | FIN flag only. Similar evasion technique. |
| `-sX` | Xmas | FIN + PSH + URG flags. Named for lighting up like a Christmas tree. |

**TCP Connect vs SYN — the key difference:**

```
TCP Connect (-sT):   SYN → SYN/ACK → ACK → RST   (full handshake, appears in logs)
SYN Scan    (-sS):   SYN → SYN/ACK → RST           (half-open, many apps won't log it)
```

> ⚠️ "Stealth" is relative — modern IDS/IPS systems absolutely detect SYN scans. Don't rely on them being invisible.

---

### Detection & Information Flags

| Flag | What it does |
|---|---|
| `-O` | Detect the target's operating system |
| `-sV` | Detect service versions (e.g. Apache 2.4.41, OpenSSH 7.9) |
| `-A` | Aggressive — OS + version detection + script scanning + traceroute |
| `-sC` | Run default NSE scripts (checks for common vulnerabilities) |

### Port Selection

| Flag | What it does |
|---|---|
| `-p 80` | Scan only port 80 |
| `-p 80,443,445` | Scan specific ports |
| `-p 1000-2000` | Scan a range |
| `-p-` | Scan all 65,535 ports |
| `--top-ports 20` | Scan the 20 most commonly used ports |

### Speed & Output

| Flag | What it does |
|---|---|
| `-T0` to `-T5` | Timing: 0 = paranoid/slow, 3 = default, 4 = aggressive, 5 = insane |
| `-v` / `-vv` | Verbose — show results as they come in |
| `-oN file.txt` | Save output in readable format |
| `-oG file.txt` | Save in grepable format |
| `-oA basename` | Save in all three formats at once |

### NSE Scripts

| Flag | What it does |
|---|---|
| `--script=<name>` | Run a specific script |
| `--script=vuln` | Run all vulnerability detection scripts |
| `--script=default` | Same as `-sC` |

---

### Common Command Recipes

```bash
# Quick scan of top 1000 ports
nmap <IP>

# Full scan — all ports, versions, OS, scripts
sudo nmap -A -p- <IP>

# Fast stealth scan, all ports
sudo nmap -sS -T4 -p- <IP>

# Identify services on common ports
nmap -sV -p 22,80,443,445 <IP>

# Save results to file
nmap -oN results.txt <IP>

# Run vulnerability scripts
sudo nmap --script=vuln <IP>

# Find all live hosts on local network
nmap -sP 192.168.1.0/24

# Scan UDP top ports (slow but important)
sudo nmap -sU --top-ports 20 <IP>
```

</details>

---

<!-- ============================================================ -->
<!-- CTF LABS -->
<!-- ============================================================ -->

<details>
  <summary><h2>🧪 CTF Labs — CyLab / picoCTF</h2></summary>

**Platform:** CyLab Security Academy (formerly picoCTF) — Carnegie Mellon University  
**URL:** https://cylabacademy.org  
**Path:** Beginner's Guide to the Challenge Library  
**Progress:** 17/28 modules complete

---

### What is a CTF?

A **Capture the Flag** competition is a cybersecurity challenge where you find hidden strings called **flags** — formatted like `picoCTF{some_text_here}` — by exploiting intentionally vulnerable systems, decoding files, or solving puzzles. CTFs are one of the best hands-on ways to build real security skills.

---

### Progress

| Section | Modules | Status |
|---|---|---|
| CTF Onboarding | 3/3 | ✅ Complete |
| Section 1 — Sanity Checks | 3/3 | ✅ Complete |
| Section 2 — CyberChef | 4/4 | ✅ Complete |
| Section 3 — General Skills | 6/6 | ✅ Complete |
| Section 4 — Python | 2/6 | 🔄 In Progress |

---

### Tools Used

| Tool | What it does | When to use |
|---|---|---|
| `cat` | Print file contents | Read any text file |
| `strings` | Extract readable text from any file | Binary files with hidden flags |
| `grep` | Search for a pattern in output | Filter large outputs, find flags |
| `file` | Identify what type a file actually is | Always run first on unknown files |
| `nc` (netcat) | Connect to a remote server | Challenges with a host + port |
| `ssh` | Log into a remote machine | Challenges with SSH credentials |
| `base64 -d` | Decode Base64 strings | Encoded strings in challenges |
| CyberChef | Browser-based encoder/decoder | ROT13, Base64, hex, and more |
| `unzip` | Extract zip archives | Compressed file challenges |
| `bvi` | Binary file viewer | Inspect `.bin` or binary files in hex |
| `wget` | Download files from a URL | Getting challenge files |

---

### Universal CTF Checklist

When you receive a new challenge file, work through this in order:

```bash
# 1. Find out what type of file it actually is
file <filename>

# 2. Extract readable text and hunt for the flag
strings <filename> | grep -i "pico"

# 3. If encoded — open CyberChef and try Base64, ROT13, hex

# 4. If it's a website — view source, check /robots.txt, check JS files

# 5. If given a host + port
nc <host> <port>

# 6. If given SSH credentials
ssh user@host -p <port>
```

---

## CTF Onboarding

**Platform:** CyLab Security Academy / picoCTF  
**Module:** Beginner's Guide to the Challenge Library — CTF Onboarding  
**Date:** June 2026  
**Skills demonstrated:** Platform orientation, understanding flags, using the webshell

#### Overview

The onboarding section introduces what a CTF is, what a flag looks like, and how to submit one. No technical skills are tested yet — the goal is to get comfortable with the platform and environment before the real challenges begin.

#### Key Concepts

- A **flag** is always formatted as `picoCTF{some_text_here}` — copy it exactly and paste it into the answer box. Flags are case-sensitive.
- The **Challenge Library** (picoGym) is a permanent practice arena — challenges are always available, at your own pace, with hints allowed.
- The **Webshell** is a browser-based Linux terminal provided by picoCTF. It's a real Ubuntu environment, useful if you don't have your own Linux setup.
- Every challenge shows its **category** (General Skills, Cryptography, Web, Forensics, etc.) — use this to know which tools to reach for.

#### Takeaways

- Points don't matter for learning — understanding the *why* behind each solution does.
- The category label is your first clue about which tools and techniques apply.
- Having your own Kali Linux VM is better than the webshell — full tool access, no browser dependency.

---

## Section 1 — Sanity Checks

**Platform:** CyLab Security Academy / picoCTF  
**Module:** Beginner's Guide to the Challenge Library — Section 1  
**Date:** June 2026  
**Skills demonstrated:** Reading files with `cat`, SSH remote login, netcat connections

#### Overview

Three challenges that introduce the most fundamental Linux commands and networking tools: reading files, connecting via SSH, and using netcat. These three tools alone solve a large portion of beginner CTF challenges and are used constantly in real security work.

#### Key Concepts

**Challenge 1 — Obedient Cat:** The flag was hidden in a plain text file. Reading it required one command:

```bash
cat flag
```

**Challenge 2 — Super SSH:** Logging into a remote machine using SSH with credentials provided by the challenge.

```bash
ssh ctf-player@titan.picoctf.net -p 65341
# Accept the fingerprint prompt → type: yes
# Enter the password when asked
```

| Part | Meaning |
|---|---|
| `ctf-player` | Username to log in as |
| `@titan.picoctf.net` | The remote machine's address |
| `-p 65341` | Custom port (default SSH is 22) |

**Challenge 3 — What's a Netcat?:** Connecting to a raw TCP service with no credentials — the server sent the flag immediately on connection.

```bash
nc jupiter.challenges.picoctf.org 64287
```

| Tool | Use when... |
|---|---|
| `ssh` | Challenge gives a **username + password** |
| `nc` | Challenge gives just a **host + port** |

#### Takeaways

- `cat`, `ssh`, and `nc` are three of the most-used tools in CTFs and real security work — get comfortable with all three.
- The SSH fingerprint warning is normal in CTFs; in real environments always verify it before accepting.
- Netcat is called the "Swiss Army knife of networking" — it's also used for reverse shells, file transfers, and port listeners in real engagements.

---

## Section 2 — CyberChef

**Platform:** CyLab Security Academy / picoCTF  
**Module:** Beginner's Guide to the Challenge Library — Section 2  
**Date:** June 2026  
**Skills demonstrated:** ROT13, hex/decimal/binary conversion, Base64 decoding, CyberChef

#### Overview

This section introduces encoding and number systems — the foundation of almost all cryptography challenges. CyberChef is the primary tool: a free browser-based encoder/decoder built by GCHQ. Drag an operation into the Recipe, paste your input, and it transforms it instantly.

**CyberChef:** https://gchq.github.io/CyberChef/

#### Key Concepts

**Challenge 1 — Mod 26 (ROT13):** ROT13 replaces every letter with the one 13 positions away in the alphabet. Applying it twice returns the original — it's its own reverse. Not real encryption, just obfuscation.

```bash
# Terminal method
echo "cvpbPGS{...}" | tr A-Za-z N-ZA-Mn-za-m
```

**Challenge 2 — Warmed Up (Hex → Decimal):** The prefix `0x` signals a hexadecimal number. Converting to decimal:

```
0x3D = (3 × 16) + 13 = 61
```
```bash
echo $((0x3D))    # → 61
```

**Challenge 3 — 2Warm (Decimal → Binary):** Divide by 2 repeatedly, read remainders bottom-up.

```
42 → 101010 (binary)
```
```bash
echo "obase=2; 42" | bc    # → 101010
```

**Challenge 4 — Bases (Base64):** Base64 encodes binary data as readable text. It's used in JWTs, email attachments, and data URLs. It's not encryption — anyone can decode it instantly. Recognise it by the trailing `=` or `==` padding.

```bash
echo "bDNhcm5fdGgzX3IwcDM1" | base64 -d
```

| Base | Name | Prefix | Example |
|---|---|---|---|
| 2 | Binary | none / `0b` | `101010` = 42 |
| 10 | Decimal | none | `42` |
| 16 | Hexadecimal | `0x` | `0x2A` = 42 |

| Encoding | Recognise it by | Decode with |
|---|---|---|
| ROT13 | Scrambled letters | CyberChef ROT13 or `tr` |
| Base64 | Alphanumeric + `+/=`, no spaces | CyberChef or `base64 -d` |
| Hex | `0x` prefix or pairs like `48 65 6c` | CyberChef "From Hex" |

#### Takeaways

- Number system conversions (hex, binary, decimal) appear constantly in CTFs and real security work — memory addresses, file signatures, and colour codes all use hex.
- Base64 is not encryption. Seeing it in a web token or URL parameter is worth investigating.
- CyberChef's "Magic" operation can auto-detect the encoding — useful when you're not sure what you're looking at.

---

## Section 3 — General Skills

**Platform:** CyLab Security Academy / picoCTF  
**Module:** Beginner's Guide to the Challenge Library — Section 3  
**Date:** June 2026  
**Skills demonstrated:** `strings`, `grep`, `unzip`, tab completion, web source inspection, robots.txt

#### Overview

The core toolkit for CTF survival: extracting text from binary files, filtering large outputs, tab completion for deep directory trees, inspecting web page source code, and exploiting `robots.txt`. These techniques appear in almost every CTF category.

#### Key Concepts

**Challenge 1 — Wave a Flag / Challenge 4 — Strings It:** Extract readable text from a binary file and filter for the flag.

```bash
strings <file> | grep -i pico
```

This pattern is so common it should be automatic — run it on any unknown file before anything else.

**Challenge 2 — Tab, Tab, Attack:** Navigate a deeply nested directory tree using tab completion. Instead of typing full folder names, type the first few letters and press **Tab** — the terminal fills in the rest.

```bash
unzip Addadshashanammu.zip
strings Add<TAB>/Alm<TAB>/Ash<TAB>/...  # tab-complete each folder
```

**Challenge 3 — Insp3ct0r:** The flag was split across three files of a web page — the HTML, CSS, and JS — hidden in comments.

- HTML comment: `<!-- hidden here -->`
- CSS/JS comment: `/* hidden here */`
- Steps: View Page Source → find part 1 → open linked `.css` and `.js` files → find parts 2 and 3

**Challenge 5 — First Grep:** Search a large plain text file for the flag without reading every line.

```bash
grep -i pico <file>
```

**Challenge 6 — Where are the Robots?:** `robots.txt` tells search engine crawlers which pages not to index — but those "hidden" URLs are visible to anyone. Always check it on web challenges.

```
https://challenge-url/robots.txt
→ Disallow: /8028f.html
https://challenge-url/8028f.html  → flag is here
```

#### Takeaways

- `strings <file> | grep -i pico` is the first command to run on any unknown file in a CTF.
- Tab completion saves enormous time and prevents typos — use it constantly.
- Always check `/robots.txt` on web challenges. The `Disallow` entries are essentially a list of interesting pages the site owner wanted to hide from Google — but not from you.
- View Page Source and check all linked CSS and JS files; flags and sensitive data are often hidden in comments.

---

## Section 4 — Python

**Platform:** CyLab Security Academy / picoCTF  
**Module:** Beginner's Guide to the Challenge Library — Section 4  
**Date:** June 2026  
**Skills demonstrated:** Running Python scripts, reading source code, hex-to-ASCII conversion, MD5 hashing, dictionary attacks

#### Overview

This section builds Python skills through a series of password-cracking challenges. Each one adds a new layer of complexity — from a hardcoded plain-text password, to hex-encoded characters, to MD5 hashing. Reading the provided source code before doing anything else is the key skill throughout.

---

### Python Wrangling ✅
**Flag:** `picoCTF{4p0110_1n_7h3_h0us3_dbd1bea4}`

The challenge gave a Python script (`ende.py`), an encrypted flag file, and a password file. Reading the script revealed it needed a `-d` flag to decrypt and would prompt for a password.

```bash
cat pw.txt           # read the password
python3 ende.py -d flag.txt.en
# enter password when prompted
```

The script used **Fernet encryption** — a symmetric scheme where the same key encrypts and decrypts. Since the key was in `pw.txt`, decryption was straightforward once the script's usage was understood.

> Always `cat` or `nano` a script before running it — even a quick skim tells you what arguments it expects.

---

### PW Crack 1 ✅
**Flag:** `picoCTF{545h_r1ng1ng_fa343060}`

The password was hardcoded directly in the source code as plain text inside an `if` condition.

```bash
nano level1.py      # find: if user_pw == "1e1a"
python3 level1.py -d level1.flag.txt.enc
# enter: 1e1a
```

Hardcoded credentials are a real-world vulnerability — penetration testers use `grep -r "password" .` to hunt for exactly this in source code and config files.

---

### PW Crack 2 ✅
**Flag:** `picoCTF{tr45h_51ng1ng_502ec42e}`

The password was stored as four hex character codes: `chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65)`. Each `chr()` call converts a number to its ASCII character.

```python
# In Python shell — decode each value:
chr(0x33)  # → '3'
chr(0x39)  # → '9'
chr(0x63)  # → 'c'
chr(0x65)  # → 'e'
# Password: 39ce
```

```bash
python3 level2.py -d level2.flag.txt.enc
# enter: 39ce
```

When you see `chr(0x__)` in Python source code, the script is building a string character by character from hex values — decode each one and join them.

---

### PW Crack 3 ✅
**Flag:** `picoCTF{m45h_fl1ng1ng_cd6ed2eb}`  
**Skills demonstrated:** MD5 hash comparison, dictionary attack, Python scripting, binary file inspection, reading source code

#### Overview

In this challenge, the password was no longer stored as plain text or a simple encoding — it was stored as an **MD5 hash** in a binary file (`level3.hash.bin`). Because MD5 is a one-way function, it can't be reversed to recover the original password. The solution was a **dictionary attack**: hash each candidate password from the provided list using the same MD5 function, and compare the result to the stored hash. When they match, the original password is found.

This mirrors a core real-world technique used by both attackers (cracking leaked password databases) and defenders (understanding why MD5 is no longer safe for password storage).

---

#### Understanding MD5 Hashing

A **hash** is a one-way transformation of data. You put a password in, you get a fixed-length string of bytes out. Three key properties:

- **One-way:** You cannot reverse a hash to recover the original input
- **Deterministic:** The same input always produces the exact same output
- **Fixed length:** MD5 always produces a 16-byte (128-bit) result, regardless of input size

```
Password → MD5 hash function → Hash output
"87ab"   → MD5             → E1 6D 55 A5 5D 80 DD DD 52 A8 3E AB EA 57 2B 7B
```

> 💡 **Why `bvi`?** The hash is stored in binary format (`.bin`), not plain text. Opening it with `cat` produces garbled output. `bvi` is a binary file viewer that shows the raw hex bytes — which is exactly what you see when you inspect `level3.hash.bin`: `E1 6D 55 A5 5D 80 DD DD 52 A8 3E AB EA 57 2B 7B`. That's the MD5 hash of the correct password stored on disk.

---

#### Reading the Script

The relevant parts of `level3.py` provided by the challenge:

```python
import hashlib

flag_enc = open('level3.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level3.hash.bin', 'rb').read()   # the stored hash

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()   # returns the MD5 hash as raw bytes

# 7 possible passwords — only 1 is correct
pos_pw_list = ["f09e", "4dcf", "87ab", "dba8", "752e", "3961", "f159"]
```

The script already has everything needed:
- `hash_pw()` — hashes any password string using MD5
- `correct_pw_hash` — the stored hash to match against
- `pos_pw_list` — the 7 candidates to test

---

#### Step by Step

**1. Inspect the hash file (optional — useful to understand what you're working with):**
```bash
bvi level3.hash.bin
# Shows raw bytes: E1 6D 55 A5 5D 80 DD DD 52 A8 3E AB EA 57 2B 7B
# :q to exit
```

**2. Run a dictionary attack — open the Python3 shell and paste the full script plus this loop:**

```python
import hashlib

flag_enc = open('level3.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level3.hash.bin', 'rb').read()

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

pos_pw_list = ["f09e", "4dcf", "87ab", "dba8", "752e", "3961", "f159"]

for pw in pos_pw_list:
    if hash_pw(pw) == correct_pw_hash:
        print("Found it:", pw)
```

Output: `Found it: 87ab`

**3. Run the challenge script and enter the cracked password:**
```bash
python3 level3.py -d level3.flag.txt.enc
# Please enter correct password for flag: 87ab
# Welcome back... your flag, user:
# picoCTF{m45h_fl1ng1ng_cd6ed2eb}
```

---

#### What's Happening Under the Hood

| Step | What the code does |
|---|---|
| `hash_pw(pw)` | Takes the candidate password, converts it to bytes, runs MD5, returns the hash |
| `hash_pw(pw) == correct_pw_hash` | Compares the freshly computed hash to the one stored in `.hash.bin` |
| Match found | The loop prints the password — you now know which one to enter |
| `str_xor()` | The challenge script uses the password to XOR-decrypt the flag from the `.enc` file |

---

#### Key Concepts Reference

| Concept | What it means |
|---|---|
| MD5 | One-way hash function — same input always gives same output, cannot be reversed |
| Dictionary attack | Hash every candidate and compare — when hashes match, password is found |
| Rainbow tables | Pre-computed hash lookup tables — why storing passwords as plain MD5 is insecure |
| `bvi` | Binary file viewer — needed since `cat` on a `.bin` file produces garbled output |
| `m.digest()` | Returns the MD5 result as raw bytes — must match the binary stored in `.hash.bin` |

---

#### Takeaways

- MD5 is a one-way function — you can't reverse it, but you can replicate it. If you can hash all the candidates and compare, you don't need to reverse anything.
- Dictionary attacks are the standard approach to cracking hashed passwords when a candidate list exists. In the real world, attackers use wordlists with millions of entries (like `rockyou.txt`) and tools like `hashcat` to crack MD5 hashes from breached databases almost instantly.
- Storing passwords as plain MD5 hashes is considered insecure precisely because of dictionary and rainbow table attacks. Modern systems use slow hashing algorithms like **bcrypt** or **Argon2** with salting to make this computationally infeasible.
- Reading source code is always the first step — the script told us exactly which hash function was used (`hashlib.md5`), where the stored hash lived (`level3.hash.bin`), and what the candidate passwords were (`pos_pw_list`).
- Python is case-sensitive: `correct_pw_hash` ≠ `Correct_pw_hash`. A `NameError` almost always means a capitalisation mistake.

---

*Learning journey: CTF Labs (CyLab) → Sections 1–4 → PW Crack 4 (next)*

</details>

---

<!-- ============================================================ -->
<!-- KC7 -->
<!-- ============================================================ -->

<details>
  <summary><h2>🔍 KC7 — Threat Hunting with KQL</h2></summary>

**Platform:** KC7 — CyLab Security Academy  
**URL:** https://kc7cyber.com  
**Role:** SOC Analyst simulation — investigate real-style attacks using log data

---

### What is KC7?

KC7 is a hands-on cybersecurity training platform where you play the role of a **SOC (Security Operations Center) analyst** investigating cyberattacks using log data and KQL queries. Each scenario is a self-contained investigation — you're given access to logs and must figure out what happened.

---

### KQL — Kusto Query Language

KQL is a **read-only query language** used to search and analyse large volumes of log data. Think of it like SQL, but built specifically for security logs and event data. It's used in real-world tools like **Microsoft Sentinel**, **Azure Data Explorer**, and **Defender for Endpoint**.

| Use Case | What analysts do |
|---|---|
| **Threat Hunting** | Proactively search logs for hidden attackers before an alert fires |
| **Incident Investigation** | After a breach, trace exactly what happened — logins, file access, data exfiltration |
| **Anomaly Detection** | Find unusual patterns like a user logging in at 3am from two countries simultaneously |

---

### KQL Basic Structure

```kql
TableName
| operator condition
| operator condition
```

Start with the **table**, use `|` to chain operators (just like piping in Linux), and each line filters or transforms the data further.

---

### Operators

**`where`** — filter rows by a condition (like `grep` in Linux):
```kql
Employees
| where role == "Editorial Director"
```

**`count`** — count how many rows remain:
```kql
Email
| where recipient == "target@example.com"
| count
```

**`distinct`** — return only unique values (like `sort | uniq` in Linux):
```kql
Email
| where sender has "suspicious-domain.com"
| distinct sender
| count
```

**`let`** — store a result as a variable to reuse it:
```kql
let target_ips = Employees
| where name has "Mary"
| distinct ip_addr;
OutboundNetworkEvents
| where src_ip in (target_ips)
| count
```
> The semicolon `;` closes the `let` block. The second query runs immediately after using the stored values.

---

### `has` vs `==` vs `contains`

| Operator | Meaning | Example |
|---|---|---|
| `==` | Exact match (case-sensitive) | `role == "Admin"` |
| `has` | Contains a whole word | `sender has "gmail.com"` |
| `contains` | Contains a substring | `domain contains "hire"` |

---

### Tables Reference

| Table | What it contains |
|---|---|
| `Employees` | Staff info: name, role, email, IP, username |
| `Email` | Email logs: sender, recipient, subject, timestamp |
| `OutboundNetworkEvents` | Web browsing: source IP, URL, timestamp |
| `PassiveDns` | DNS records: domains and the IPs they resolved to |
| `AuthenticationEvents` | Login attempts: username, success/fail, timestamp, IP |

---

### Investigation Log

| # | Scenario | Date | Status |
|---|---|---|---|
| 0 | How to Play KC7 | Jun 2026 | ✅ Complete |
| 1 | A Rap Beef — Module 1: Enough Beef for a Burger | Jun 2026 | ✅ Complete |
| 2 | A Rap Beef — Module 2: Less Beef, More Phish | Jun 2026 | 🔄 In Progress |

---

### 🎤 A Rap Beef: An Intro to Security Investigations

**Platform:** KC7 — CyLab Security Academy  
**Module 1:** Enough Beef for a Burger  
**Date:** June 2026  
**Skills demonstrated:** KQL querying, log analysis, reconnaissance detection, information disclosure vulnerabilities, account takeover investigation

---

#### Overview

In this investigation, I played the role of a SOC analyst at OWL Records — a music label that had been targeted by a threat actor. The attacker's goal was to take over the account of one of the label's artists. The scenario introduced core log analysis skills: querying inbound network traffic, reading browsing activity to establish attacker intent, and tracing an account takeover step by step.

This type of investigation directly mirrors real SOC work: an alert fires, you pull the logs, and you follow the trail from first contact to full compromise.

---

#### Key Concepts

**Tables used:**

| Table | What it contains |
|---|---|
| `Employees` | Staff info: names, roles, email addresses |
| `InboundNetworkEvents` | Web server logs: source IP, URL, timestamp, HTTP method |

**KQL operators introduced:**

| Operator | What it does | Example |
|---|---|---|
| `where` | Filter rows by condition | `where role == "CEO"` |
| `between` | Filter by a time range | `where timestamp between (datetime("...") .. datetime("..."))` |
| `has` | Match a whole word within a field | `where src_ip has "18.66.52.227"` |
| `has_any()` | Match any one of several keywords | `where url has_any("washington", "fluffy")` |

**Vulnerability identified:** Information disclosure via GET request URL parameters. The target's email address and username were exposed directly in a password reset URL — allowing the attacker to harvest credentials passively just by browsing the site.

---

#### Practical Application

The investigation unfolded in stages:

**1. Identifying the target organisation's leadership**
```kql
Employees
| where role == "CEO"
```

**2. Scoping the attacker's activity to a single day**
```kql
InboundNetworkEvents
| where timestamp between (datetime("2024-04-10T00:00:00") .. datetime("2024-04-11T00:00:00"))
| where src_ip has "18.66.52.227"
```
Result: 19 rows — confirming the attacker (IP `18.66.52.227`) was actively browsing the OWL Records website on 10 April 2024.

**3. Reading the browsing trail**  
Reviewing the 19 URLs told the full story. The attacker started with broad searches ("OWL Records rapper contact info"), narrowed to a specific artist (Dwake), and eventually found his email address and username exposed in a password reset URL:
```
https://owl-records.com/account/reset-password?username=dwaubrey&email=dwake_audrey@owl-records.com
```

**4. Catching the account takeover in the logs**
```kql
InboundNetworkEvents
| where timestamp between (datetime("2024-04-10T00:00:00") .. datetime("2024-04-11T00:00:00"))
| where url has_any("washington", "fluffy")
| where src_ip has "18.66.52.227"
```
This surfaced the exact moment the attacker submitted answers to Dwake's security questions — completing the account takeover.

**Full attack chain:**  
Reconnaissance → Information disclosure (URL leak) → Password reset → Security questions bypass → Account takeover

---

#### Takeaways

- Sensitive data (emails, usernames) should never appear in GET request URLs — they end up in server logs, browser history, and referrer headers, making them trivially harvestable from logs alone.
- Security questions are a weak authentication fallback; answers are often publicly available or guessable. Modern systems replace them with MFA/TOTP.
- `has_any()` is a powerful KQL operator for quickly surfacing events matching any one of several known values — useful when you already know what you're looking for.
- Attacker behaviour in logs often tells a story: the progression from broad searches to targeted queries to account actions maps directly to the reconnaissance → exploitation chain seen in real incidents.

---

*Learning journey: Threat Hunting with KQL → A Rap Beef Module 1 → Module 2: Less Beef, More Phish*

</details>

---

<!-- ============================================================ -->
<!-- QA ENGINEERING -->
<!-- ============================================================ -->

<details>
  <summary><h2>🛠️ QA Engineering Reference</h2></summary>

**Source:** TripleTen QA Engineering Bootcamp  
**Topics:** Linux CLI, Python basics, Pytest, API testing

This section documents the technical foundations from QA engineering training — Linux command line, Python, automated testing with Pytest, and HTTP/API testing. These skills directly transfer to cybersecurity work.

---

### Linux Command Line

| Command | Description | Example |
|---|---|---|
| `pwd` | Print current working directory | `pwd` |
| `ls` | List files and folders | `ls -l` |
| `cd` | Change directory | `cd Documents` |
| `mkdir` | Create a new directory | `mkdir new_folder` |
| `rm` | Remove files | `rm file.txt` |
| `rm -r` | Remove directory and contents recursively | `rm -r folder_name` |
| `cp` | Copy files | `cp file.txt /home/user/Desktop` |
| `mv` | Move or rename files | `mv file.txt newfile.txt` |
| `touch` | Create a new empty file | `touch notes.txt` |
| `cat` | Display file contents | `cat file.txt` |
| `grep` | Search for text inside files | `grep "error" logfile.txt` |
| `find` | Search for files | `find /home -name notes.txt` |
| `sudo` | Execute as superuser | `sudo apt update` |
| `chmod` | Change file permissions | `chmod 755 script.sh` |
| `chown` | Change file ownership | `sudo chown user:group file.txt` |

💡 Use `--help` after any command (e.g. `ls --help`) to see available options.

---

### Python Basics

**Variables and Types**
```python
name = "Camilo"       # string
age = 30              # integer
height = 1.80         # float
is_active = True      # boolean
```

**Functions**
```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Camilo"))  # Hello, Camilo!
```

**Lists and Loops**
```python
tools = ["nmap", "wireshark", "burpsuite"]

for tool in tools:
    print(tool)
```

**Conditionals**
```python
port = 80

if port == 80:
    print("HTTP")
elif port == 443:
    print("HTTPS")
else:
    print("Unknown service")
```

---

### Pytest — Automated Testing

Pytest is a testing framework for Python. A test is any function whose name starts with `test_`.

```python
def test_sum():
    assert 2 + 3 == 5

def test_deposit_positive():
    deposit = get_user_deposit()
    assert deposit > 0
```

**Running tests:**
```bash
pytest                   # run all tests in project
pytest calc_test.py      # run tests in a specific file
```

**Three rules for clean tests:**
- ✅ One test → one assertion
- ✅ Tests use independent data — no shared state
- ✅ Each test can run alone, in any order

---

### API Testing with Requests + Pytest

```python
import requests
import configuration

def get_docs():
    return requests.get(configuration.URL_SERVICE + configuration.DOC_PATH)

response = get_docs()
print(response.status_code)  # 200 = OK
```

**Common response attributes:**

| Attribute | Purpose |
|---|---|
| `status_code` | HTTP response code (200, 404, 500...) |
| `text` | Response body as a string |
| `json()` | Response body parsed as a dictionary |
| `headers` | Response headers |
| `ok` | `True` if status code is 2xx |

---

### Python Cheat Sheet

A Python reference by Jack Rhysider (host of [Darknet Diaries](https://darknetdiaries.com/)) is included in this repo for study and reference.

> 📄 **Credit:** Original creator — [Jack Rhysider](https://darknetdiaries.com/) · All rights belong to the original author.

[📘 View the Python Cheat Sheet](./assets/Python-CheatSheet.pdf)

</details>

---

<!-- ============================================================ -->
<!-- COMING SOON -->
<!-- ============================================================ -->

<details>
  <summary><h2>🚧 Coming Soon</h2></summary>

Areas actively in progress — notes will be added as completed:

- **Home Lab Setup** — Kali Linux VM, VPN server (PiVPN + WireGuard), Pi-hole, network segmentation
- **KC7 Investigations** — ongoing SOC simulation writeups
- **picoCTF / CyLab** — Section 4 (Python) and beyond
- **PowerShell for Security** — scripting and automation
- **Networking Deep Dive** — TCP/IP, DNS, HTTP in depth
- **Web Application Security** — OWASP Top 10, Burp Suite

</details>

---

<div align="center">

*Built while studying for a Master of Cyber Security at Edith Cowan University (ECU), Perth — starting 2026.*

</div>
