# 🎤 A Rap Beef: An Intro to Security Investigations

[← Back to Learning Log](../README.md)

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
