# Section 1 — Sanity Checks

[← Back to Learning Log](../../README.md)

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
