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
| Section 4 — Python | 1/6 | 🔄 In Progress |

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
