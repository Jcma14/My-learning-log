# 🗺️ Nmap — Network Scanning

[← Back to Learning Log](../README.md)

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
