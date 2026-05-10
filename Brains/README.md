Got it — you want your README to match that clean **CTF-style format**. Here’s your **Brains write-up converted into that exact style** 👇

---

# 🧠 CTF Challenge: Brains – TeamCity Exploitation & Log Analysis 🔥

![Category](https://img.shields.io/badge/Category-Web_Exploitation_&_SOC-blue)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green)
![Tools](https://img.shields.io/badge/Tools-Metasploit%20%7C%20Nmap%20%7C%20Splunk-lightgrey)

---

## 📖 Scenario Overview

An engineering server running a CI/CD pipeline was compromised due to a critical vulnerability in a TeamCity instance.

An attacker exploited the system, gained remote access, and established persistence within the server environment.

From a defensive standpoint, system logs were collected to investigate the attacker’s activities and determine how the breach occurred.

**Target IP:** `10.48.181.158`
**Service:** TeamCity (Port 50000)

---

## 🎯 Objective

* Exploit the vulnerable TeamCity server
* Gain remote access to the machine
* Capture the user flag
* Investigate logs to uncover attacker actions
* Identify persistence mechanisms and malicious artifacts

---

## 🛠️ Tools Used

* **Nmap** – Service discovery and enumeration
* **Metasploit Framework** – Exploitation (RCE)
* **Splunk** – Log analysis and threat investigation
* **Linux CLI** – Post-exploitation and enumeration

---

## 📂 Repository Contents

* `challenge` – TryHackMe room: Brains
* `writeup.md` – Full step-by-step technical walkthrough
* `README.md` – This file

---

## 🚀 Solution Summary

### 🔴 Red Team (Exploitation Phase)

1. **Reconnaissance**

   * Performed full port scan using Nmap
   * Identified:

     * SSH (22)
     * HTTP (80)
     * TeamCity (50000)

2. **Enumeration**

   * Accessed TeamCity web interface
   * Version identified: `2023.11.3`
   * Found vulnerability:
     ➤ CVE-2024-27198

3. **Exploitation**

   * Used Metasploit module:

     ```
     exploit/multi/http/jetbrains_teamcity_rce_cve_2024_27198
     ```
   * Payload:

     ```
     java/meterpreter/reverse_tcp
     ```
   * Successfully gained reverse shell

4. **Post-Exploitation**

   * Spawned interactive shell
   * Retrieved flag:

     ```
     THM{faa9bac345709b6620a6200b484c7594}
     ```

---

### 🔵 Blue Team (Investigation Phase)

1. **Backdoor User Detection**

   * Splunk query:

     ```
     index=* sourcetype=auth_logs "new user"
     ```
   * Found attacker-created account:
     ➤ `eviluser`

2. **Malware / Package Analysis**

   * Query:

     ```
     index=* ("apt install" OR "dpkg -i")
     ```
   * Detected suspicious installations:

     * `gcc`
   * Malicious package identified:
     ➤ `datacollector`

3. **Malicious Plugin Discovery**

   * Query:

     ```
     index=* sourcetype=teamcity_activities "plugin"
     ```
   * Found uploaded exploit payload:
     ➤ `AyzzbuXY.zip`

---

## 🧠 Key Learnings

* CI/CD tools like TeamCity are high-value targets
* Authentication bypass vulnerabilities can lead to full system compromise
* Attackers often:

  * Create backdoor users
  * Install malicious packages
  * Upload custom payloads/plugins
* Log analysis is critical for incident response and threat hunting

---

## 🚩 Flag

<details>
  <summary>Click to reveal flag</summary>

```
THM{faa9bac345709b6620a6200b484c7594}
```

</details>

---

## ⚠️ Disclaimer

This project is for **educational purposes only**.
All activities were performed in a controlled lab environment on TryHackMe.

---
