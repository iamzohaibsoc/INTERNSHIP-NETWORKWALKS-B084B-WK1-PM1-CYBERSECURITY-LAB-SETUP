<div align="center">

![Networkwalks x Mediroza — Penetration Testing Report](cover_banner.png)

# 🛡️ Networkwalks Internship — Week 4 Penetration Test
### 🎯 Target: Mediroza General Hospital (`medirozahospital.com`)

[![Engagement](https://img.shields.io/badge/Engagement-Black--Box_Pentest-critical?style=for-the-badge&logo=kalilinux&logoColor=white)]()
[![Authorization](https://img.shields.io/badge/Authorization-Written_Permission-success?style=for-the-badge&logo=checkmarx&logoColor=white)]()
[![Duration](https://img.shields.io/badge/Duration-5_Days-blue?style=for-the-badge&logo=clockify&logoColor=white)]()
[![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)]()

</div>

> ⚠️ **Authorization Notice:** Conducted in a controlled training environment under written client authorization (**Networkwalks Penetration Testing Project, Week 4**). Techniques documented here were performed **only** against the authorized lab target and must never be used against a system without explicit written permission.

---

## 📋 Engagement Overview

| Detail | Specification |
| :--- | :--- |
| 🏥 **Client** | Mediroza General Hospital |
| 🕵️ **Engagement Type** | Black-box penetration test |
| ⏱️ **Duration** | 5 days |
| 🌐 **Target** | `https://medirozahospital.com` |
| 📜 **Authorization** | Written permission granted (Networkwalks M1–M3) |

---

## 🛠️ Tools & Arsenal Used

<p align="left">
  <img src="https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white" alt="Kali Linux" />
  <img src="https://img.shields.io/badge/Whois-Reconnaissance-2C3E50?style=for-the-badge&logo=gnu-bash&logoColor=white" alt="Whois" />
  <img src="https://img.shields.io/badge/WhatWeb-Fingerprinting-1ABC9C?style=for-the-badge&logo=linux&logoColor=white" alt="WhatWeb" />
  <img src="https://img.shields.io/badge/Katana-Web_Crawler-E74C3C?style=for-the-badge&logo=go&logoColor=white" alt="Katana" />
  <img src="https://img.shields.io/badge/SQL_Injection-Manual_Exploit-D35400?style=for-the-badge&logo=mysql&logoColor=white" alt="SQL Injection" />
  <img src="https://img.shields.io/badge/pdf2john-Hash_Extraction-8E44AD?style=for-the-badge&logo=python&logoColor=white" alt="pdf2john" />
  <img src="https://img.shields.io/badge/Hashcat-Password_Cracking-C0392B?style=for-the-badge&logo=gnubash&logoColor=white" alt="Hashcat" />
  <img src="https://img.shields.io/badge/Browser_DevTools-Manual_Inspection-F39C12?style=for-the-badge&logo=googlechrome&logoColor=white" alt="DevTools" />
  <img src="https://img.shields.io/badge/AI_Assisted-Data_Analysis-412991?style=for-the-badge&logo=openai&logoColor=white" alt="AI Analysis" />
</p>

| Category | Tool / Technique | Purpose in Engagement |
| :--- | :--- | :--- |
| 🔍 **Reconnaissance** | `whois`, `whatweb`, `katana` | Domain profiling, tech-stack fingerprinting (LiteSpeed), and endpoint crawling |
| 💉 **Web Exploitation** | Manual SQLi (`admin' --`) | Bypassed Patient Portal authentication (`/patient/login.php`) |
| 🔓 **Cryptography / Cracking** | `pdf2john` + `Hashcat` | Extracted `$pdf$2*3*128*...` hashes and cracked weak dictionary passwords |
| 📂 **Directory Enumeration** | `robots.txt` + Browser | Discovered exposed `/old/` directory listing and SQL database dump |
| 📊 **Data & Reporting** | SQL / AI Analysis + PDF Layout | Parsed 30 HR & 10 shareholder records; compiled branded 10-page report |

---

## 🎯 Objectives & Milestones

Four milestones, completed sequentially:

1. 🚩 **M1 – Initial Access:** Attack the site and retrieve 3 confidential patient PDF lab reports.
2. 🔑 **M2 – Data Extraction:** Crack the encryption on all 3 retrieved files.
3. 💥 **M3 – Critical Exposure:** Use the retrieved data to uncover staff salaries and shareholder details.
4. 📝 **M4 – Reporting:** Write a professional penetration testing report.

---

## 🚨 Summary of Findings

| ID | Vulnerability / Finding | Severity |
| :---: | :--- | :---: |
| **F1** | **SQL Injection** (`admin' --`) bypassed Patient Portal authentication | ![Critical](https://img.shields.io/badge/CRITICAL-B71C1C?style=flat-square) |
| **F2** | **Public Database Backup** (`/old/mediroza_db_backup_2019.sql`) exposed via directory listing | ![Critical](https://img.shields.io/badge/CRITICAL-B71C1C?style=flat-square) |
| **F3** | **Weak PDF Encryption** — All 3 patient reports used dictionary-crackable passwords (`123456`, `password`, …) | ![High](https://img.shields.io/badge/HIGH-E65100?style=flat-square) |
| **F4** | **Sensitive Path Disclosure** — `robots.txt` exposed `/patient/`, `/staff/`, and `/old/` | ![Medium](https://img.shields.io/badge/MEDIUM-FBC02D?style=flat-square&logoColor=black) |
| **F5** | **Server Banner Disclosure** — LiteSpeed version and verbose HTTP headers exposed | ![Low](https://img.shields.io/badge/LOW-0288D1?style=flat-square) |

---

## ⚙️ Methodology

1. **🔍 Reconnaissance** — Executed `whois`, `whatweb`, and `katana` crawling to map hidden endpoints and identify the underlying technology stack.
2. **💉 Exploitation (M1)** — Performed a SQL injection authentication bypass on `/patient/login.php` using `admin' --`, gaining unauthorized access to 3 confidential pathology PDF reports.
3. **🔓 Password Cracking (M2)** — Extracted each PDF's encryption hash (`$pdf$2*3*128*...`) and executed a `Hashcat` dictionary attack (100-word built-in list), recovering every password within 1–2 attempts.
4. **📂 Exposure Discovery (M3)** — Inspected `/old/` referenced inside `robots.txt`; directory indexing was enabled, exposing `mediroza_db_backup_2019.sql` — a complete HR database dump containing 30 staff records (names, emails, phone numbers, national IDs, salaries) and 10 shareholder records (equity stakes, share classes). Leveraged AI-assisted analysis to summarize payroll totals, equity distribution, and anomalies.
5. **📝 Reporting (M4)** — Consolidated all findings into a formal penetration test report featuring an executive summary, methodology, proof-of-concept evidence, risk matrix, and actionable remediation steps.

---

## 📊 Key Data Points *(Exposed Backup)*

* 👥 **Workforce & Equity:** 30 staff records across 17 departments; 10 shareholders holding **1,000,000 shares** total.
* 💰 **Payroll Exposure:** Total monthly payroll of **R2,027,000** (~**R24.3M** annualized).
* 🏆 **Top Earners:**
  * **Dr. Johan van der Merwe** *(Medical Director)* — `R160,000/mo`
  * **Sarah Botha** *(CFO)* — `R152,000/mo`
  * **Dr. Rajesh Naidoo** *(Chief Pathologist)* — `R138,000/mo`
* 🏢 **Ownership Structure:** 8 of 10 shareholders are internal staff members (**74%** of shares); **Cedar Health Holdings** (`15%`) and the **Reddy Family Trust** (`11%`) hold external equity.

---

## 📦 Repository Deliverables

* 📄 **`Networkwalks_Week4_Penetration_Testing_Report_Mediroza.pdf`** — Submission-ready 10-page report styled in Networkwalks/Mediroza branding (cover, milestone-coded finding pages, risk matrix, and remediation guide).
* 🔗 **[Editable Working Doc](https://claude.ai/artifact/PJBJCVfEJJx31D58kFWek2)** — Live working draft for collaborative review and edits prior to final submission.
* 📘 **`README.md`** — Engagement overview and technical documentation (this file).
* 🖼️ **`cover_banner.png`** — Custom branded cover graphic featuring Networkwalks & Mediroza visual identity.
* 📸 **Evidence Archive** — Screenshots documenting every milestone (recon, SQLi bypass, web crawl, hash cracking, decrypted PDFs, open directory listing, and exposed SQL dump).

---

## ⚖️ Disclaimer

> This work was performed strictly within the scope of an authorized educational penetration-testing exercise (**Networkwalks Internship, Week 4**) against a designated lab target. No part of this document or the techniques it describes should be applied to any system without explicit, documented, written permission from its owner.