# 🔐 Cybersecurity Lab Environment Setup

**An isolated VirtualBox lab with Kali Linux, a Windows 10 victim machine and Wireshark traffic analysis, built for ethical hacking and SOC practice**

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000)
![VirtualBox](https://img.shields.io/badge/Hypervisor-VirtualBox%207-0070C0?style=flat-square&labelColor=000000)
![Kali](https://img.shields.io/badge/Kali%20Linux-2026.3-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000)
![Wireshark](https://img.shields.io/badge/Wireshark-SOC%20Lab%203-1679A7?style=flat-square&labelColor=000000)
![Windows](https://img.shields.io/badge/Victim-Windows%2010-0078D6?style=flat-square&labelColor=000000)
![Program](https://img.shields.io/badge/NetworkWalks-Week%2001-C00000?style=flat-square)

---

## 📌 Project Overview

This project documents the setup of a **virtual cybersecurity testing lab** on my own laptop using **VirtualBox** and **Kali Linux**.

The goal is a safe, repeatable and isolated environment where I can practise network scanning, packet analysis, vulnerability assessment and other authorised security-testing activities without touching any real-world system.

The lab runs on a private **NAT Network (`10.0.0.0/24`)**, so the attacker machine (Kali) and target machines (Windows 10 victim, and others added later) can talk to each other while still having outbound Internet access.

In addition to the mandatory task, I completed the **optional Phase 2** (Windows 10 victim VM) and the **optional Wireshark SOC Lab 3**.

---

## 🎯 Objectives

- [x] Use VirtualBox as the virtualisation base
- [x] Set up Kali Linux as the attacker machine
- [x] Create a NAT Network in the `10.0.0.0/24` subnet
- [x] Enable clipboard and drag & drop between host and VM
- [x] Share the host `Downloads` folder with the Kali VM
- [x] Assign Kali the static IP `10.0.0.2/24`
- [x] Give Kali full Internet access
- [x] Take a clean baseline snapshot
- [x] *(Optional)* Install a Windows 10 victim VM
- [x] *(Optional)* Complete the Wireshark SOC Lab 3, "Identify the Attacker's IP"

---

## 🏗️ Lab Architecture

```
                    ┌──────────────────────────────────────────┐
                    │        Host OS (Windows) + VirtualBox     │
                    │                                          │
                    │   NAT Network "NatNetwork"  10.0.0.0/24  │
                    │   Gateway 10.0.0.1  |  DHCP enabled      │
                    │                                          │
                    │   ┌────────────────┐  ┌───────────────┐  │
                    │   │  Kali Linux    │  │  Windows 10   │  │
                    │   │  (Attacker)    │  │  (Victim)     │  │
                    │   │  10.0.0.2/24   │  │  10.0.0.10/24 │  │
                    │   └────────────────┘  └───────────────┘  │
                    └──────────────────────┬───────────────────┘
                                           │ NAT
                                       Internet
```

Additional target VMs can be added to the same network later (Windows 11, Windows 7, Server 2016, Android, CTF machines).

---

## ⚙️ Lab Configuration

| Component | Configuration |
|---|---|
| Host OS | Windows |
| Hypervisor | Oracle VirtualBox 7.x |
| Attacker VM | Kali Linux 2026.3 (VirtualBox image, amd64) |
| Victim VM | Windows 10 (x64) |
| Virtual network | NAT Network, name `NatNetwork` |
| Network address | `10.0.0.0/24` |
| DHCP | Enabled on the NAT Network |
| IPv6 | Disabled |
| Kali IP address | `10.0.0.2/24` |
| Default gateway | `10.0.0.1` |
| DNS server | `8.8.8.8` |
| Adapter type | Intel PRO/1000 MT Desktop (82540EM) |
| Promiscuous mode | Allow All |
| Shared clipboard / drag & drop | Bidirectional |
| Shared folder | Host `Downloads` folder |
| Planned VM range | `10.0.0.2 – 10.0.0.99` |

---

# 🪜 Lab Setup Procedure

## Step 1. Install 7-Zip
Installed 7-Zip to extract compressed VM packages and downloads.

## Step 2. Install VirtualBox
Downloaded and installed the latest recommended VirtualBox release from the official site.

## Step 3. Create the NAT Network
In VirtualBox: **File → Tools → Network → NAT Networks → Create**

| Setting | Value |
|---|---|
| Name | `NatNetwork` |
| IPv4 Prefix | `10.0.0.0/24` |
| Enable DHCP | ✅ |
| Enable IPv6 | ❌ |

![VirtualBox NAT Network](screenshots/01-virtualbox-natnetwork.png)

A **NAT Network** (rather than plain NAT) lets all VMs on the same network reach each other *and* reach the Internet, which is exactly what an attacker/victim lab needs.

## Step 4. Import Kali Linux
Downloaded the official Kali Linux VirtualBox image from kali.org and imported it into VirtualBox.

VM settings:

- **Network → Adapter 1:** attached to *NAT Network*, name `NatNetwork`
- **General → Advanced:** Shared Clipboard and Drag'n'Drop set to *Bidirectional*
- **Shared Folders:** host `Downloads` folder added with auto-mount

![Kali Linux 2026.3 running in VirtualBox](screenshots/02-kali-desktop.png)

## Step 5. Configure the Kali IP address
Set a manual IPv4 configuration in NetworkManager (**Edit Connections → Wired connection 1 → IPv4 Settings**):

```
Method:   Manual
Address:  10.0.0.2
Netmask:  24
Gateway:  10.0.0.1
DNS:      8.8.8.8
```

Verified with `ip a`:

![Kali IP verification](screenshots/03-kali-ip-address.png)

`eth0` is `UP` with `10.0.0.2/24` and broadcast `10.0.0.255`, as required.

## Step 6. Take a clean snapshot
After the network was working, I took a snapshot of the Kali VM as a known-good baseline.

```
Snapshot name: Clean Kali - NatNetwork 10.0.0.2
```

If an experiment breaks the VM, I can roll back to this state in seconds.

---

# 🪟 Phase 2 (Optional): Windows 10 Victim VM

To have a realistic target for later labs, I installed Windows 10 in a second VirtualBox VM named `window 10 victim` and attached it to the same `NatNetwork`.

| Setting | Value |
|---|---|
| VM name | window 10 victim |
| Edition | Windows 10 Home (x64) |
| Network | NAT Network → `NatNetwork` |
| Planned IP | `10.0.0.10/24` |

![Selecting the Windows 10 edition](screenshots/04-win10-select-edition.png)
![Windows 10 installation in progress](screenshots/05-win10-installing.png)

> ⚠️ The victim VM is an intentionally isolated lab target. It is never exposed to the real network.

---

# 🔎 Lab Verification

| Test | Command | Expected result |
|---|---|---|
| Check IP address | `ip a` | `eth0` shows `10.0.0.2/24` |
| Test gateway | `ping -c 4 10.0.0.1` | Replies received |
| Test Internet | `ping -c 4 8.8.8.8` | Replies received |
| Test DNS | `nslookup google.com` | Domain resolves |
| Test lab connectivity | `ping -c 4 10.0.0.10` | Replies from the Windows 10 victim |

---

# 🐞 Problems Encountered & Solutions

## Problem: Kali had no Internet ("Network is unreachable")

After setting the static IP, Kali could not reach the Internet:

```
$ ping -c 4 8.8.8.8
ping: connect: Network is unreachable
```

This is a known issue with VirtualBox 7 and newer Kali releases when a manual IPv4 address is used. The fix is to disable the duplicate address detection delay on the NetworkManager connection and then restart the connection:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

![nmcli fix](screenshots/06-nmcli-fix.png)

After reactivating the connection, the interface came up with `10.0.0.2/24` and connectivity worked.

**Checklist I used while troubleshooting:**

1. NAT Network created correctly (`10.0.0.0/24`)
2. Adapter 1 attached to the right NAT Network
3. No other VM using `10.0.0.2`
4. Gateway set to `10.0.0.1`
5. Connection name matches the real NetworkManager name (`nmcli connection show`)

---

# 🦈 Optional: Wireshark SOC Lab 3, "Identify the Attacker's IP"

This lab simulates a SOC analyst task: open a packet capture, find who is attacking, who is being targeted and what kind of attack it is.

**Result: 70 / 100**

![Wireshark Lab 3 result](screenshots/07-wireshark-lab3-result.png)

| # | Task | Result |
|---|---|---|
| 1 | Capture the traffic | ✅ Correct |
| 2 | Show only SYN packets | ❌ Wrong |
| 3 | Find the attacker | ✅ Correct |
| 4 | Find the target | ✅ Correct |
| 5 | Source port | ❌ Wrong |
| 6 | Ports probed | ✅ Correct |
| 7 | Open ports | ✅ Correct |
| 8 | Reply for closed ports | ✅ Correct |
| 9 | Closed ports | ❌ Wrong |
| 10 | Attack type | ✅ Correct |

### What the traffic showed

The capture was a **TCP SYN (half-open) port scan**, a reconnaissance technique:

| Port state | How the target replies |
|---|---|
| Open | `SYN, ACK` |
| Closed | `RST, ACK` |

The attacker sends a `SYN` to many ports and uses the replies to map which ports are open, without completing the three-way handshake.

### Where I lost marks, and what I learned

- **Show only SYN packets.** A display filter for *initial* SYN packets must also exclude SYN-ACK replies:
  ```
  tcp.flags.syn == 1 && tcp.flags.ack == 0
  ```
  Filtering on `tcp.flags.syn == 1` alone also matches the `SYN, ACK` replies.
- **Source port.** The scanner's source port is a random high (ephemeral) port, not the service port being probed. I need to read the right column, source versus destination.
- **Closed ports.** Closed ports are identified by the `RST, ACK` replies, so I should filter on those (`tcp.flags.reset == 1`) and read the *source* port of each reset.

Reviewing these misses made the difference between "I can open Wireshark" and "I can actually read a scan in a capture".

---

# 💡 What I Learned

1. **NAT vs NAT Network.** Plain NAT isolates each VM; a NAT Network lets VMs on the same network talk to each other and still reach the Internet.
2. **Static IPs in NetworkManager.** Manual addressing needs the correct netmask, gateway and DNS, and some VirtualBox/Kali combinations need the `dad-timeout` fix.
3. **Snapshots.** Take a clean snapshot before experimenting, not after something breaks.
4. **Attacker/victim lab design.** Keeping the lab on its own virtual subnet means scans and tests never leave my machine.
5. **Reading a port scan.** SYN, SYN-ACK and RST-ACK patterns tell you the full story of what an attacker probed.
6. **Documentation.** Writing down errors, fixes and evidence is as important as the setup itself.

---

# 🔐 Security & Ethical Use

This lab is for **education and authorised testing only**. Only scan or attack systems that you own or have explicit written permission to test. All activity in this project took place inside an isolated virtual network on my own machine.

---

# 📁 Repository Structure

```
.
├── README.md
└── screenshots/
    ├── 01-virtualbox-natnetwork.png
    ├── 02-kali-desktop.png
    ├── 03-kali-ip-address.png
    ├── 04-win10-select-edition.png
    ├── 05-win10-installing.png
    ├── 06-nmcli-fix.png
    └── 07-wireshark-lab3-result.png
```

---

# 🔗 Tools & Resources

- **7-Zip:** https://7-zip.org/download.html
- **VirtualBox:** https://virtualbox.org/wiki/Downloads
- **Kali Linux:** https://kali.org/get-kali
- **Wireshark:** https://www.wireshark.org
- **NetworkWalks:** https://www.networkwalks.com

---

# 👤 Author

**Zohaib Khan**
Cybersecurity learner | Network security

LinkedIn: *add your profile link here*

---

## 📌 Project Information

**Program:** Cybersecurity at NetworkWalks | **Week:** 01 | **Project:** WK1-PM1 Cybersecurity Lab Setup (Virtualbox & Kali Linux) | **Date completed:** 30 October 2026
