# 🕵️ Shadow Trace — SOC Analyst CTF Writeup

> **Platform:** TryHackMe
> **Difficulty:** Beginner–Intermediate
> **Category:** SOC / Malware Analysis / Threat Hunting

---

## 📖 Scenario

> *It's the middle of the night shift. You're the only analyst in the SOC when a manager calls in urgently: a suspicious file was found on a user's machine and needs immediate review.*
>
> *You open the file and start digging. Something doesn't look normal for a company updater, and at the same time, the EDR throws a couple of alerts.*
>
> **Your task:** Analyse the file, collect anything to identify it, gather any potential IOCs, correlate and analyse the alerts for potential malicious behaviour. It's up to you to piece together what's happening before it spreads further.

---

## 🎯 Learning Objectives

- Extract IOCs from suspicious binaries
- Correlate alerts with malicious activity
- Perform basic SOC triage actions

---

## ✅ Prerequisites

- Introduction to Malware Analysis
- MAL: Malware Introductory
- Malware Classification

---

## 🔍 Part 1 — File Analysis

### Q1: What is the architecture of the binary file `windows-update.exe`?

**Answer:** `64-bit`

> 📌 **How to find it:** Use `file windows-update.exe` in a Linux terminal or open the binary in a tool like **PEStudio**, **Detect-It-Easy (DIE)**, or **CFF Explorer** to inspect the PE header. A 64-bit PE binary will show `PE32+` in the header.

📸 *Screenshot:*

![Q1 - Binary Architecture](screenshots/q1-architecture.png)

---

### Q2: What is the SHA-256 hash of `windows-update.exe`?

**Answer:** `b2a88de3e3bcfae4a4b38fa36e884c586b5cb2c2c283e71fba59efdb9ea64bfc`

> 📌 **How to find it:**
> ```bash
> sha256sum windows-update.exe
> # or on Windows PowerShell:
> Get-FileHash windows-update.exe -Algorithm SHA256
> ```
> You can also submit the hash to **VirusTotal** for further analysis.

📸 *Screenshot:*

![Q2 - SHA256 Hash](screenshots/q2-sha256.png)

---

### Q3: Identify the URL within the file to use it as an IOC

**Answer:** `http://tryhatme.com/update/security-update.exe`

> 📌 **How to find it:** Use **strings** to extract readable content from the binary:
> ```bash
> strings windows-update.exe | grep -i "http"
> ```
> Alternatively, use **FLOSS** (FireEye Labs Obfuscated String Solver) or open the file in **PEStudio** and navigate to the Strings section to spot embedded URLs.

📸 *Screenshot:*

![Q3 - Embedded URL IOC](screenshots/q3-url-ioc.png)

---

### Q4: With the URL identified, can you spot a domain that can be used as an IOC?

**Answer:** `responses.tryhatme.com`

> 📌 **How to find it:** After finding the URL, investigate the base domain `tryhatme.com` further. Using tools like **Wireshark**, **Fakenet-NG**, or running the binary in a sandbox (e.g., **Any.run**, **Joe Sandbox**), you can observe DNS requests or HTTP traffic resolving to subdomains such as `responses.tryhatme.com`.

📸 *Screenshot:*

![Q4 - Domain IOC](screenshots/q4-domain-ioc.png)

---

### Q5: Input the decoded flag from the suspicious domain

**Answer:** `THM{you_g0t_some_IOCs_friend}`

> 📌 **How to find it:** Visiting or querying the suspicious domain (in a safe/sandboxed environment) returns an encoded response. Decoding it (Base64, ROT13, or similar) reveals the hidden flag.

📸 *Screenshot:*

![Q5 - Decoded Flag](screenshots/q5-decoded-flag.png)

---

### Q6: What library related to socket communication is loaded by the binary?

**Answer:** `WS2_32.dll`

> 📌 **How to find it:** Open the binary in **PEStudio** or use the following command to inspect the import table:
> ```bash
> objdump -p windows-update.exe | grep -i "dll"
> # or use:
> dumpbin /imports windows-update.exe
> ```
> `WS2_32.dll` (Windows Socket 2) is a strong indicator of network communication capabilities — a common trait in malware that beacons back to a C2 server.

📸 *Screenshot:*

![Q6 - WS2_32.dll Import](screenshots/q6-ws2-dll.png)

---

## 🚨 Part 2 — Alert Analysis

### Q7: What is the malicious URL triggered by `powershell.exe`?

**Answer:** `https://tryhatme.com/dev/main.exe`

> 📌 **How to find it:** Review the EDR/SIEM alert logs and filter by process name `powershell.exe`. Look for outbound web requests or `Invoke-WebRequest` / `DownloadFile` patterns in the alert details. The URL `https://tryhatme.com/dev/main.exe` represents a PowerShell-based dropper downloading a secondary payload.

📸 *Screenshot:*

![Q7 - PowerShell Malicious URL](screenshots/q7-powershell-url.png)

---

### Q8: What is the malicious URL triggered by `chrome.exe`?

**Answer:** `https://reallysecureupdate.tryhatme.com/update.exe`

> 📌 **How to find it:** Filter the alerts by process `chrome.exe`. Browser-based download alerts often appear as file download events. This URL uses a deceptive subdomain (`reallysecureupdate`) to appear legitimate — a classic social engineering and domain spoofing technique.

📸 *Screenshot:*

![Q8 - Chrome Malicious URL](screenshots/q8-chrome-url.png)

---

### Q9: What is the name of the file saved in the alert triggered by `chrome.exe`?

**Answer:** `test.txt`

> 📌 **How to find it:** In the same chrome.exe alert, inspect the file write/save event details. Despite the URL pointing to a `.exe`, the saved file on disk was named `test.txt` — a common obfuscation technique to bypass filename-based detections.

📸 *Screenshot:*

![Q9 - File Saved Name](screenshots/q9-file-saved.png)

---

## 📊 IOC Summary

| Type | Value |
|------|-------|
| **SHA-256** | `b2a88de3e3bcfae4a4b38fa36e884c586b5cb2c2c283e71fba59efdb9ea64bfc` |
| **URL (Binary)** | `http://tryhatme.com/update/security-update.exe` |
| **Domain** | `responses.tryhatme.com` |
| **URL (PowerShell)** | `https://tryhatme.com/dev/main.exe` |
| **URL (Chrome)** | `https://reallysecureupdate.tryhatme.com/update.exe` |
| **Library** | `WS2_32.dll` |
| **Dropped File** | `test.txt` |

---

## 🧰 Tools Used

| Tool | Purpose |
|------|---------|
| `sha256sum` / PowerShell | File hashing |
| PEStudio / DIE | PE header & string analysis |
| FLOSS | Obfuscated string extraction |
| strings | Embedded string hunting |
| Any.run / Joe Sandbox | Dynamic analysis |
| SIEM / EDR Console | Alert correlation |

---


---

## 💡 Key Takeaways

- **Suspicious file naming** (`windows-update.exe`) is a classic masquerading technique to evade detection by mimicking legitimate OS processes.
- **Embedded URLs** in binaries are a reliable IOC source — always run `strings` or use a tool like PEStudio early in triage.
- **WS2_32.dll** imports indicate network socket capability — a red flag for C2 beaconing malware.
- **Subdomain spoofing** (e.g., `reallysecureupdate.tryhatme.com`) is used to make malicious domains look trustworthy.
- Correlating **multiple alerts across different processes** (PowerShell + Chrome) suggests a multi-stage infection chain.

---

## 🏁 Flag

```
THM{you_g0t_some_IOCs_friend}
```

---

*Writeup by: Khairul Azim*
*Date: May 2026*
