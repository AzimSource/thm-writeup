# CTF Challenge: Unsecured NFS Traffic Analysis 🕵️‍♂️🔍

![Category](https://img.shields.io/badge/Category-Network_Forensics-blue)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green)
![Tools](https://img.shields.io/badge/Tools-Wireshark%20%7C%20Hashcat%20%7C%20unzip-lightgrey)

## 📖 Scenario Overview
An intruder successfully infiltrated the corporate network and targeted an internal Network File System (NFS) server used for storing backup files. During the incident, a classified secret was accessed and exfiltrated. 

The only trace left behind by the attacker is a packet capture (PCAP) file recorded during the breach. 

**Intruder IP:** `172.16.175.128`  
**NFS Server IP:** `10.10.19.157`

## 🎯 Objective
Perform network traffic analysis on the provided PCAP to discover the contents of the stolen data and recover the classified flag.

## 🛠️ Tools Used
*   **Wireshark:** For packet filtering and raw byte extraction.
*   **Hash Cracker:** (e.g., CrackStation, Hashcat, or John the Ripper) to crack the intercepted MD5 hash.
*   **Linux CLI:** `unzip` to extract the recovered archive.

## 📂 Repository Contents
*   `challenge` - Try Hack Me - https://tryhackme.com/room/hfb1stolenmount.
*   `writeup.md` - A step-by-step technical guide on how the challenge was solved.
*   `README.md` - This file.

## 🚀 Solution Summary
1.  **Traffic Analysis:** Filtered NFS traffic between the attacker and the server.
2.  **Credential Harvesting:** Discovered a `READ_PLUS` reply containing a plaintext MD5 hash for an archive password.
3.  **Hash Cracking:** Cracked the MD5 hash (`90eb7723a657b6597100aafef171d9f2`) to reveal the plaintext password.
4.  **Data Extraction:** Identified a separate packet containing the raw bytes of a ZIP archive (`secrets.png` inside), exported the raw packet bytes, and rebuilt the file.
5.  **Decryption:** Unzipped the recovered archive using the cracked password to extract the final image.

For the full, detailed walkthrough, please see the [Write-up](writeup.md).

## 🚩 Flag
<details>
  <summary>Click to reveal flag</summary>
  
  `THM{n0t_s3cur3_f1l3_sh4r1ng}`
</details>
