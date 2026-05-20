# CTF Challenge: Shadow Trace 🕵️‍♂️💻

![Category](https://img.shields.io/badge/Category-Malware_Analysis-red)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green)
![Tools](https://img.shields.io/badge/Tools-PEStudio%20%7C%20CyberChef%20%7C%20SIEM-lightgrey)

## 📖 Scenario Overview
It’s the middle of the night shift, and you're the only analyst in the SOC when an urgent call comes in from management. A suspicious executable file has been discovered on a user's machine, and early EDR alerts suggest potential malicious activity.

The file appears to masquerade as a legitimate Windows updater, but further inspection reveals hidden Indicators of Compromise (IOCs), encoded payloads, suspicious domains, and network-related behavior.

Your task is to investigate the binary, extract useful intelligence, correlate SIEM alerts, and determine the malicious infrastructure involved before the threat spreads further.

## 🎯 Objectives
Perform static analysis and alert investigation to:

- Identify Indicators of Compromise (IOCs)
- Extract suspicious URLs and domains
- Decode obfuscated data
- Correlate SIEM alerts with malicious activity
- Investigate suspicious PowerShell and browser behavior

## 🛠️ Tools Used
* **PEStudio** – Static malware analysis and IOC extraction
* **CyberChef** – Base64 and ASCII decoding
* **SIEM Dashboard** – Alert investigation and correlation
* **Windows Malware Analysis Techniques** – Static inspection of PE files

## 📂 Repository Contents
* `challenge` - TryHackMe Room - https://tryhackme.com/room/shadowtrace
* `Shadow Trace.pdf` - Full step-by-step walkthrough and investigation process
* `README.md` - This file

## 🚀 Solution Summary

1. **Static Malware Analysis**
   - Inspected the suspicious binary (`windows-update.exe`) using PEStudio
   - Identified the binary architecture and SHA-256 hash

2. **IOC Extraction**
   - Discovered suspicious URLs embedded in the executable
   - Extracted malicious domains from Indicators tab

3. **Encoded Data Analysis**
   - Detected Base64-encoded content hidden in network artifacts
   - Decoded the payload using CyberChef to reveal the hidden flag

4. **Library & Capability Analysis**
   - Identified imported networking library (`WS2_32.dll`)
   - Confirmed the malware has socket communication capabilities

5. **Alert Correlation**
   - Investigated SIEM alerts triggered by:
     - `powershell.exe`
     - `chrome.exe`

6. **Payload Investigation**
   - Decoded obfuscated PowerShell payloads
   - Recovered malicious download URLs
   - Identified suspicious downloaded file names

For the full detailed walkthrough, please see the [Write-up](TryHackMe-ShadowTrace.pdf).

---

## 🔍 Key Findings

| Question | Answer |
|---|---|
| Binary Architecture | `64-bit` |
| SHA-256 Hash | `b2a88de3e3bcfae4a4b38fa36e884c586b5cb2c2c283e71fba59efdb9ea64bfc` |
| Suspicious URL | `http://tryhatme.com/update/security-update.exe` |
| Socket Library | `WS2_32.dll` |
| PowerShell URL | `https://tryhatme.com/dev/main.exe` |
| Chrome Alert URL | `https://reallysecureupdate.tryhatme.com/update.exe` |
| Saved File Name | `test.txt` |

---

## 🚩 Flag

<details>
  <summary>Click to reveal flag</summary>

  `THM{you_g0t_some_IOCs_friend}`

</details>

---

## 📚 Skills Learned

- Static malware analysis
- IOC extraction
- Base64 and ASCII decoding
- PE file inspection
- SIEM alert analysis
- Threat correlation
- Malware triage workflow

---

## ⚠️ Disclaimer
This repository is created for **educational and cybersecurity training purposes only**.  
All activities were performed in a controlled lab environment provided by TryHackMe.
