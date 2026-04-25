<div align="center">

# 🛡️ Offensive Security Research Journal

### by Siva Aditya

[![TryHackMe](https://img.shields.io/badge/TryHackMe-Profile-red?style=for-the-badge&logo=tryhackme&logoColor=white)](https://tryhackme.com/p/sivaadityacoder)
[![GitHub](https://img.shields.io/badge/GitHub-sivaadityacoder-181717?style=for-the-badge&logo=github)](https://github.com/sivaadityacoder)
[![Rooms](https://img.shields.io/badge/Rooms_Completed-growing-brightgreen?style=for-the-badge)]()
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE_ATT%26CK-Referenced-blue?style=for-the-badge)](https://attack.mitre.org/)

> Hands-on offensive security writeups from TryHackMe and beyond.  
> Each room is documented as a full case study — recon → exploitation → post-exploitation → lessons learned.

</div>

---

## 📑 Table of Contents

| # | Room | Difficulty | Category | Techniques | MITRE Tags |
|---|------|-----------|----------|------------|-----------|
| 1 | [Blue](#-blue--eternalblue-ms17-010) | 🟢 Easy | Windows / Network | EternalBlue, Meterpreter | T1190, T1059.001 |

---

## 🔵 Blue — EternalBlue (MS17-010)

> **Path:** [`rooms/Blue/`](rooms/Blue/README.md)

| Field | Detail |
|-------|--------|
| Platform | TryHackMe |
| OS | Windows 7 |
| Difficulty | Easy |
| CVE | CVE-2017-0144 (EternalBlue) |
| Category | Network Exploitation, Privilege Escalation |

A classic Windows exploitation room demonstrating the famous **EternalBlue** (MS17-010) SMB vulnerability weaponised by WannaCry and NotPetya. Covers full attack chain from recon to SYSTEM shell.

➡️ [Read Full Writeup →](rooms/Blue/README.md)

---

## 📁 Repository Layout

```
Tryhackme-rooms/
├── README.md                  ← This file — master journal index
└── rooms/
    ├── TEMPLATE/
    │   └── README.md          ← Blank template for new writeups
    └── Blue/
        ├── README.md          ← Full case study
        ├── notes/
        │   └── recon.md       ← Raw recon / scan notes
        └── scripts/
            └── exploit_helper.py
```

---

## 🧭 Methodology

Every room writeup follows the same battle-tested structure:

1. **Reconnaissance** — passive + active intel gathering  
2. **Scanning & Enumeration** — ports, services, versions  
3. **Exploitation** — vulnerability identification and weaponisation  
4. **Post-Exploitation** — privilege escalation, persistence, flag capture  
5. **MITRE ATT&CK Mapping** — technique-to-tactic alignment  
6. **Defensive Takeaways** — blue-team controls that would have stopped this  

---

## 📜 License

All writeups are for **educational purposes only**.  
Do not use these techniques against systems you don't own or have explicit permission to test.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
