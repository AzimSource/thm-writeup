# TryHackMe - Brains Writeup

> Detailed walkthrough and investigation for the **Brains** room on TryHackMe.  
> This room focuses on exploiting a vulnerable JetBrains TeamCity server and performing Blue Team log analysis using Splunk.

---

## Room Information

- **Platform:** TryHackMe
- **Room:** Brains
- **Difficulty:** Medium
- **Category:** Red Team / Blue Team / DFIR
- **Skills Learned:**
  - Nmap Enumeration
  - TeamCity Exploitation
  - CVE-2024-27198
  - Metasploit Usage
  - Linux Post-Exploitation
  - Splunk Log Investigation
  - Threat Hunting

---

# RED TEAM — Exploit The Server

## Phase 1 — Reconnaissance

Started with a full Nmap scan to identify services and open ports.

```bash
nmap -sC -sV -p- -v 10.48.181.158
