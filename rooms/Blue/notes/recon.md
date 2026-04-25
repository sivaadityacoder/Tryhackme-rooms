# 📝 Blue — Raw Recon Notes

> These are unedited field notes captured during enumeration.  
> For the polished writeup, see [`../README.md`](../README.md).

---

## Target Info

```
IP      : 10.10.x.x   (replace with actual room IP)
Date    : YYYY-MM-DD
VPN     : TryHackMe OpenVPN — UK/EU server
LHOST   : 10.13.x.x
```

---

## Phase 1 — Ping / Host Check

```bash
ping -c 3 10.10.x.x
# Confirms host is up
```

---

## Phase 2 — Nmap Quick Scan

```bash
nmap -sV -T4 10.10.x.x
```

```
PORT     STATE SERVICE      VERSION
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
3389/tcp open  tcpwrapped
```

---

## Phase 3 — Full TCP Scan

```bash
nmap -sV -sC -p- --min-rate 5000 -oN full_scan.txt 10.10.x.x
```

Notable: `smb-vuln-ms17-010` script fires → **VULNERABLE**

---

## Phase 4 — SMB Specific Enumeration

```bash
# Confirm MS17-010
nmap -p 445 --script smb-vuln-ms17-010 10.10.x.x

# List SMB shares
smbclient -L //10.10.x.x -N
# Result: NT_STATUS_ACCESS_DENIED (anonymous blocked — expected on Win7)

# Enum4linux
enum4linux -a 10.10.x.x
# OS: Windows 7 Professional 7601 Service Pack 1 x64
# Workgroup: WORKGROUP
# Users: Jon, Administrator, Guest
```

---

## Phase 5 — Notes on Exploit Selection

- `ms17_010_eternalblue` — Rank: great, preferred
- `ms17_010_psexec` — Rank: normal, fallback
- Payload: `windows/x64/shell/reverse_tcp` → upgraded to Meterpreter post-shell

---

## Phase 6 — Post-Exploitation Notes

```
Flags found:
  c:\flag1.txt
  c:\Windows\System32\config\flag2.txt
  c:\Users\Jon\Documents\flag3.txt

Hashes (Jon):
  ffb43f0de35be4d9917ac0cc8ad57f8d  → cracked: alqfna22

Persistence: NOT established (lab environment — not required)
```
