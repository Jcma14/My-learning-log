# Honeypots — Cowrie (CSI6230)

[← Back to Learning Log](../../README.md)

**Platform:** ECU — CSI6230 Computer Networks and Security
**Module:** Honeypots lecture + Workshop: Installing and Testing a Cowrie Honeypot
**Date:** September 2026
**Skills demonstrated:** Honeypot deployment, Linux service management, network socket analysis, log analysis, SSH configuration

##### Overview

A honeypot is a decoy system deliberately exposed to attackers to study their behaviour, divert them from real assets, or both — depending on whether it's deployed for research or protection. This module covered the theory (interaction levels, deployment planning, legal considerations) alongside a hands-on lab installing and testing **Cowrie**, a medium-interaction SSH/Telnet honeypot, on an Ubuntu VM with a Kali VM acting as the attacker.

##### Key Concepts

**Interaction levels** — honeypots range from low interaction (easy to deploy, limited data, e.g. Honeyd) to high interaction (a real, fully compromisable system, e.g. Sebek). Cowrie sits in the middle: it emulates a convincing shell and filesystem without being a real OS underneath, trading some realism for much lower risk.

**Research vs protection** — organisations deploy honeypots either to study attacker tools and techniques (universities, ISPs, government research), or to deter and detect attackers before they reach production assets (enterprises, government). The same technology serves both goals depending on placement and intent.

**Honeypot ecosystem** — the module also covered Conpot (a low-interaction ICS/SCADA honeypot), Cuckoo Sandbox (automated malware analysis), and T-Pot (a multi-honeypot platform built on the ELK stack, notably used by PWC Malta in 2024).

**Fingerprinting** — a honeypot that isn't credible defeats its own purpose. Research by Vetterl & Clayton (2018) showed low- and medium-interaction honeypots can be fingerprinted systematically at internet scale, which is exactly why realism (populating the fake filesystem, matching OS signatures, believable credentials) matters in deployment, not just installation.

##### Practical Application

Installed Cowrie from source into a Python virtual environment, then verified it was running and correctly bound with `ss -tlnp`:

```bash
sudo su - cowrie
mkdir new-cowrie && git clone http://github.com/cowrie/cowrie new-cowrie
cd new-cowrie && git reset --hard 0001168
virtualenv --python=python3 cowrie-env && source cowrie-env/bin/activate
pip install --upgrade pip && pip install -r requirements.txt
bin/cowrie start
```

Reading the `ss -tlnp` output was itself a useful exercise — the same host was running Cowrie (port 2222, process `twistd` — the Twisted framework daemon Cowrie runs inside), the *real* SSH daemon (port 22), a local DNS resolver, and CUPS printing, all visible in one listener table. Confirming Cowrie's process by matching its PID against `bin/cowrie status` reinforced how to verify a service is actually the one you think it is, not just that *something* is listening on the expected port.

After enabling Telnet and adding a test user in `cowrie.cfg` / `etc/userdb.txt` and restarting the service, connected from the Kali VM over SSH on port 2222 and ran basic shell commands. Reviewing `var/log/cowrie/cowrie.log` on the Ubuntu side confirmed every command from the attacking machine — including its IP — was captured in full.

Exploring the fake filesystem surfaced a good example of how these honeypots actually work under the hood: Cowrie doesn't give an attacker a real filesystem, it simulates one from a structure snapshot (`fs.pickle`), with real file *content* only for files explicitly populated in `honeyfs/`. Anything an attacker creates mid-session only exists in memory for that session.

##### Takeaways

- Medium-interaction honeypots like Cowrie are a deliberate middle ground: convincing enough to gather real attacker behaviour, without the risk of a genuinely compromisable system.
- A honeypot's value is directly tied to its believability — a stock, default deployment is fingerprintable and gets abandoned by an attacker in seconds; populating the fake filesystem, matching realistic OS signatures, and planning credible network topology are what make the data collected actually useful.
- Being able to read `ss`/`netstat` output and correctly attribute a listening port to its owning process is a foundational skill for both deploying deceptive infrastructure and, defensively, for spotting something that shouldn't be listening on a production box.
- Log review is where the actual value of a honeypot shows up — the install is just the setup; the log is the deliverable.
