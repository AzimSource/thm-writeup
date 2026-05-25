# CTF Challenge: ItsyBitsy 🕷️🖥️

![Category](https://img.shields.io/badge/Category-SOC_Analysis-blue)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow)
![Tools](https://img.shields.io/badge/Tools-Kibana%20%7C%20IDS%20%7C%20Threat_Hunting-lightgrey)

## 📖 Scenario Overview

During routine SOC monitoring, Analyst John identified a suspicious IDS alert indicating potential Command and Control (C2) communication originating from a user in the HR department.

Further investigation revealed that a suspicious file containing the pattern `THM{_____}` may have been accessed from an external infrastructure. Due to limited forensic artifacts available, analysts only had access to a week's worth of HTTP connection logs ingested into the `connection_logs` index in Kibana.

Your mission is to investigate the logs, identify the compromised host, trace the malware communication, and uncover the hidden secret code used by the attackers.

---

## 🎯 Objectives

Perform threat hunting and log analysis to:

* Identify suspicious hosts and anomalous traffic
* Investigate outbound C2 communication
* Detect Living-Off-The-Land Binaries (LOLBins)
* Trace malicious HTTP requests
* Identify attacker-controlled infrastructure
* Recover the hidden secret flag

---

## 🛠️ Tools Used

* **Kibana** – Log investigation and threat hunting
* **IDS Alerts** – Initial detection and triage
* **HTTP Connection Logs** – Network traffic analysis
* **Threat Hunting Techniques** – IOC correlation and anomaly detection

---

## 📂 Repository Contents

* `challenge` - TryHackMe Room - [https://tryhackme.com/room/itsybitsy](https://tryhackme.com/room/itsybitsy)
* `TryHackMe - ItsyBitsy.pdf` - Full walkthrough and investigation notes
* `README.md` - This file

---

## 🚀 Solution Summary

### 1. Initial Triage & Traffic Analysis

* Investigated the `connection_logs` index in Kibana
* Filtered logs for March 2022 activity
* Identified unusual traffic patterns from a suspicious source IP

### 2. Suspicious Host Identification

* Discovered that the majority of traffic originated from a normal baseline host
* Detected a low-frequency IP generating anomalous traffic
* Marked the system as the primary investigation target

### 3. LOLBin Investigation

* Analyzed HTTP requests associated with the suspicious host
* Identified the use of `bitsadmin`, a legitimate Windows binary commonly abused by attackers
* Confirmed malicious download behavior through HTTP User-Agent analysis

### 4. C2 Infrastructure Discovery

* Investigated outbound connections from the infected host
* Found communication with a public file-sharing platform frequently abused by threat actors
* Extracted the full malicious C2 URL from HTTP request paths

### 5. Payload & Secret Extraction

* Accessed the hosted malicious resource in a safe environment
* Identified the hosted file name
* Retrieved the hidden THM flag embedded in the payload

---

## 🔍 Key Findings

| Question                  | Answer                  |
| ------------------------- | ----------------------- |
| Total Events (March 2022) | `1482`                  |
| Suspicious Source IP      | `192.166.65.54`         |
| LOLBin Used               | `bitsadmin`             |
| File Sharing C2 Site      | `pastebin.com`          |
| Full C2 URL               | `pastebin.com/yTg0Ah6a` |
| Accessed File Name        | `secret.txt`            |
| Final Flag                | `THM{SECRET__CODE}`     |

---

## 🚩 Flag

<details>
  <summary>Click to reveal flag</summary>

`THM{SECRET__CODE}`

</details>

---

## 📚 Skills Learned

* SOC alert triage
* Threat hunting with Kibana
* HTTP log analysis
* IOC investigation
* LOLBin detection
* C2 infrastructure analysis
* Anomaly detection
* Network-based malware investigation

---

## ⚠️ Disclaimer

This repository is created for **educational and cybersecurity training purposes only**.
All activities were performed in a controlled lab environment provided by TryHackMe.

