# 🌐 Network Services — TryHackMe

[← Back to Learning Log](../README.md)

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
