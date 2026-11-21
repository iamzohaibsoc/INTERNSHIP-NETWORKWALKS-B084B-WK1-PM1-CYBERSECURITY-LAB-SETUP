<div align="center">

# 🔓 Password Cracking Lab — JTR & NetworkWalks Tools

### Week 3 · Project Modules 1 & 2 — Cybersecurity & Ethical Hacking

![Status](https://img.shields.io/badge/status-complete-c0141c?style=for-the-badge)
![Platform](https://img.shields.io/badge/platform-Kali%20Linux%20%7C%20Windows-222222?style=for-the-badge)
![Tool](https://img.shields.io/badge/tool-John%20the%20Ripper-c0141c?style=for-the-badge)
![Tool](https://img.shields.io/badge/tool-NetworkWalks-c0141c?style=for-the-badge)

*Training lab by [NetworkWalks Academy](https://networkwalks.com) — completed and documented for submission.*

</div>

---

## 📖 Overview

This repository documents the completion of two linked NetworkWalks training labs on **password cracking**:

| Module | Topic | Tools |
|---|---|---|
| **Module 1** | Password Cracking with JTR | `pdf2john`, `john` (CLI), Johnny (GUI) |
| **Module 2** | Password Cracking with NetworkWalks Tools | [Hash Calculator](https://networkwalks.com/hash-calculator/), [Password Cracker](https://networkwalks.com/password-cracker/) |

The goal of both labs is the same: take a password-protected PDF, extract its hash, run a dictionary attack
against that hash using `rockyou.txt` (or an equivalent wordlist), and recover the original password — showing
first-hand why weak or breached passwords provide almost no real protection.

> 📄 A fully formatted PDF write-up with all screenshots is included in this repo:
> **[`Password_Cracking_Lab_Report_NetworkWalks.pdf`](./Password_Cracking_Lab_Report_NetworkWalks.pdf)**

---

## 🎯 Task

> Crack the password of the protected sample PDF(s) using:
> 1. **JTR John** and **JTR Johnny** on a Windows/Kali machine, and
> 2. The **NetworkWalks Hash Calculator** and **Password Cracker** web tools.

As an extension beyond the brief, all three sample files were cracked and cross-verified using both
approaches:

| File | Cracked Password | Method |
|---|---|---|
| `My Locked PDF1.pdf` | `good-luck` | `john` (CLI) + `rockyou.txt` |
| `My Locked PDF2.pdf` | `password1` | `john` (CLI) + `rockyou.txt` |
| `My Locked PDF3.pdf` | `1qaz2wsx` | `john` (CLI), Johnny (GUI) **and** NetworkWalks Password Cracker |

---

## 🧪 Methodology

### Module 1 — John the Ripper (Kali Linux)

1. Download the locked PDF(s) to `~/Downloads`.
2. Extract a crackable hash with `pdf2john`:
   ```bash
   pdf2john "My Locked PDF1.pdf" > pdf_hash.txt
   ```
3. Run a dictionary attack with `john`:
   ```bash
   john --wordlist=/usr/share/wordlists/rockyou.txt pdf_hash.txt
   ```
4. Display the cracked password:
   ```bash
   john --show --format=PDF pdf_hash.txt
   ```
5. Cross-verify using **Johnny**, the GUI front-end for John the Ripper, by opening the same hash file via
   *Open password file → Start new attack*.
6. Open the PDF in a viewer and enter the recovered password to confirm.

### Module 2 — NetworkWalks Hash Calculator & Password Cracker (Browser)

1. Open the [Hash Calculator](https://networkwalks.com/hash-calculator/) and upload the locked PDF.
   The tool parses the file **locally in the browser** (nothing is uploaded) and returns a `$pdf$...` hash.
2. Copy the full hash value.
3. Open the [Password Cracker](https://networkwalks.com/password-cracker/), paste the hash, and start the
   dictionary attack against the built-in wordlist.
4. Once the match is reported, use the recovered password to unlock the PDF.

---

## 📊 Results Summary

```
┌──────────────────────┬────────────────┬───────────────────────────────┬────────────┐
│ File                 │ Password       │ Tool                          │ Time       │
├──────────────────────┼────────────────┼───────────────────────────────┼────────────┤
│ My Locked PDF1.pdf   │ good-luck      │ john (CLI) + rockyou.txt       │ 24 seconds │
│ My Locked PDF2.pdf   │ password1      │ john (CLI) + rockyou.txt       │ < 1 second │
│ My Locked PDF3.pdf   │ 1qaz2wsx       │ john / Johnny / NW Cracker     │ < 1 second │
└──────────────────────┴────────────────┴───────────────────────────────┴────────────┘
```

🏁 **Flag captured (PDF1):** `nw{cybersecurity_flag_captured_2608}`

---

## 🖼️ Evidence (Screenshots)

| | |
|---|---|
| `pdf2john` hash extraction | John cracks PDF1 → `good-luck` |
| John cracks PDF2 → `password1` | John cracks PDF3 → `1qaz2wsx` |
| Johnny GUI — PDF3 cracked (100%) | NetworkWalks Hash Calculator |
| NetworkWalks Password Cracker — match found | PDF unlocked & flag displayed |

*(Full-resolution screenshots are embedded in the PDF report linked above.)*

---

## 💡 Key Takeaways

- **Encryption vs. hashing:** encryption is reversible with the correct key; hashing is one-way and can only be
  attacked by guessing and matching, not by "decrypting."
- **Dictionary attacks are fast against weak passwords.** Every password in this lab was present in
  `rockyou.txt` and was cracked in under 30 seconds.
- **CLI and GUI tools are equivalent.** Johnny simply wraps the same `john` engine in a point-and-click
  interface — the cracking logic and results are identical.
- **Browser-based tools work too.** The NetworkWalks Hash Calculator and Password Cracker reproduce the same
  workflow entirely client-side, with no install and no file upload to a server.
- **Takeaway for defenders:** use passphrases of 12+ characters that do not appear in public breach
  dictionaries, and prefer a password manager over memorable-but-common passwords.

---

## 🛠️ Tools & References

| Tool | Link |
|---|---|
| John the Ripper | https://www.openwall.com/john/ |
| Johnny (GUI) | https://openwall.info/wiki/john/johnny |
| NetworkWalks Hash Calculator | https://networkwalks.com/hash-calculator/ |
| NetworkWalks Password Cracker | https://networkwalks.com/password-cracker/ |
| Lab source page | https://networkwalks.com/project-task-lab-password-cracking-with-networkwalks-tools/ |

---

## 📁 Repository Contents

```
.
├── README.md                                       # This file
├── Password_Cracking_Lab_Report_NetworkWalks.pdf    # Full formatted lab report
└── screenshots/                                     # Evidence screenshots referenced in the report
```

---

## ⚠️ Disclaimer

This work was performed strictly against **sample files provided for an authorized training lab** by
NetworkWalks Academy, for educational purposes only. Password cracking techniques shown here must only be used
on systems and files you own or are explicitly authorized to test. Unauthorized password cracking is illegal.

---

<div align="center">

**NetworkWalks Academy** · Cybersecurity & Ethical Hacking Training
📧 info@networkwalks.com · 🌐 [networkwalks.com](https://networkwalks.com)

</div>
