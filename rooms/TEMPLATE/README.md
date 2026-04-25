<div align="center">

# 🔲 [Room Name] — [Subtitle / Main Vulnerability]

[![Platform](https://img.shields.io/badge/Platform-TryHackMe-red?style=flat-square&logo=tryhackme)](https://tryhackme.com/room/ROOMSLUG)
[![Difficulty](https://img.shields.io/badge/Difficulty-Easy%20%7C%20Medium%20%7C%20Hard-yellow?style=flat-square)]()
[![OS](https://img.shields.io/badge/OS-Linux%20%7C%20Windows-lightgrey?style=flat-square)]()
[![CVE](https://img.shields.io/badge/CVE-XXXX--XXXXX-critical?style=flat-square)](https://nvd.nist.gov/vuln/detail/CVE-XXXX-XXXXX)
[![MITRE](https://img.shields.io/badge/MITRE-TXXXX-blue?style=flat-square)](https://attack.mitre.org/)

</div>

---

## 📋 Room Metadata

| Field | Detail |
|-------|--------|
| **Room URL** | https://tryhackme.com/room/ROOMSLUG |
| **Platform** | TryHackMe |
| **OS / Target** | e.g. Ubuntu 20.04 / Windows 10 |
| **Difficulty** | Easy / Medium / Hard |
| **Category** | e.g. Web, Network, Privilege Escalation |
| **CVE** | CVE-XXXX-XXXXX |
| **Tools Used** | Nmap, Burp Suite, … |
| **Flags** | N flags |

---

## 🎯 Executive Summary

> 2–3 sentences: what was compromised, the root vulnerability, and the impact.

---

## 🔍 Reconnaissance

### 1. Port Scan

```bash
nmap -sV -sC -p- --min-rate 5000 -oN notes/full_scan.txt <TARGET>
```

<details>
<summary>📄 Nmap Output (click to expand)</summary>

```
# paste output here
```

</details>

### Key Findings

| Port | Service | Version | Notes |
|------|---------|---------|-------|
| 80   | HTTP    | Apache  | Web app |

### 2. Additional Enumeration

```bash
# e.g. gobuster, nikto, enum4linux, etc.
```

---

## 💥 Exploitation

### Vulnerability Description

> Brief explanation of the CVE / misconfiguration.

### Steps

```bash
# commands used
```

<details>
<summary>📄 Exploit Output (click to expand)</summary>

```
# paste output here
```

</details>

### Shell Confirmation

```
$ id
uid=www-data(www-data) gid=www-data(www-data) groups=www-data(www-data)
```

---

## 🏴 Post-Exploitation

### Privilege Escalation

```bash
# enumeration commands
```

```bash
# escalation commands
```

### Flag Capture

```bash
cat /root/root.txt
# flag{REDACTED}
```

---

## 🗺️ MITRE ATT&CK Mapping

| Step | Action | Tactic | Technique ID | Technique Name |
|------|--------|--------|-------------|----------------|
| 1 | | | | |
| 2 | | | | |

---

## 🛡️ Defensive Takeaways

| Finding | Blue-Team Control |
|---------|------------------|
| | |

---

## 📚 References

| Resource | Link |
|----------|------|
| NVD — CVE | https://nvd.nist.gov/vuln/detail/CVE-XXXX-XXXXX |

---

<div align="center">

**[⬆ Back to Journal Index](../../README.md)**

</div>
