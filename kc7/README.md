# 🔍 KC7 — Threat Hunting with KQL

[← Back to Learning Log](../README.md)

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
| 1 | [A Rap Beef — Module 1: Enough Beef for a Burger](a-rap-beef-module-1.md) | Jun 2026 | ✅ Complete |
| 2 | A Rap Beef — Module 2: Less Beef, More Phish | Jun 2026 | 🔄 In Progress |
