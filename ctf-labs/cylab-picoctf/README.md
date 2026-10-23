# 🧪 CTF Labs — CyLab / picoCTF

[← Back to Learning Log](../../README.md)

**Platform:** CyLab Security Academy (formerly picoCTF) — Carnegie Mellon University  
**URL:** https://cylabacademy.org  
**Path:** Beginner's Guide to the Challenge Library  

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
| Section 4 — Python | 6/6 | ✅ Complete |
| Section 5 — Web Exploits | 1/1 | 🔄 In Progress |

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

### Sections

- [CTF Onboarding](00-ctf-onboarding.md)
- [Section 1 — Sanity Checks](01-sanity-checks.md)
- [Section 2 — CyberChef](02-cyberchef.md)
- [Section 3 — General Skills](03-general-skills.md)
- [Section 4 — Python (incl. PW Crack 1–5)](04-python.md)
- [Section 5 — Web Exploits (Enhance!, Big Zip, Vault Door Training)](05-web-exploits.md)
