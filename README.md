<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

### Building an expanded isolated virtual lab for penetration testing and ethical hacking practice

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-red)
![Ver](https://img.shields.io/badge/VirtualBox-v7.2-blue)
![Kali](https://img.shields.io/badge/Kali%20Linux-2026.2-red)
![Skill](https://img.shields.io/badge/Skill-Networking-black)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-brightgreen)
![Pentest](https://img.shields.io/badge/Penetration%20Testing-red)
![Skill](https://img.shields.io/badge/Skill-Virtualization-red)
![GitHub](https://img.shields.io/badge/GitHub-johnwick--2103-181717?logo=github)
![Windows](https://img.shields.io/badge/Windows-10%2F11%2F7-red)
![NetworkWalks](https://img.shields.io/badge/NetworkWalks-black)
![Ethical Hacking](https://img.shields.io/badge/Ethical%20Hacking-orange)
![Instructor](https://img.shields.io/badge/Waqas%20Karim-CCIE-red)

</div>

---

## 📌 Project Overview

This project focuses on setting up an **expanded virtual cybersecurity and penetration-testing laboratory** using VirtualBox, Kali Linux, and multiple Windows targets.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and Active Directory attack techniques can be practiced safely and repeatedly.

The lab is configured on a private virtual network so that multiple attacker and target machines can communicate with one another for authorized security testing.

---

## 🎯 Objectives

- Install and configure VirtualBox.
- Install/import Kali Linux as a virtual machine.
- Create a private NAT Network for the cybersecurity lab.
- Configure network connectivity for Kali Linux and multiple Windows VMs.
- Assign consistent static IP addresses to each machine.
- Verify network connectivity between all machines (Kali ↔ Windows).
- Take clean VM snapshots for recovery.
- Document the complete setup process, including problems encountered and solutions.
- Prepare the environment for future cybersecurity and Active Directory projects.

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Active Directory attack simulation
- Security-tool experimentation

> ⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
|---|---|
| 🖥️ Host OS | Windows 10/11 |
| 🧰 Hypervisor | VirtualBox 7.x |
| 🐉 Attacker OS | Kali Linux 2026.2 |
| 🌐 Virtual Network | NAT Network |
| 📡 Network Address | 10.0.0.0/24 |
| 🐧 Kali IP Address | 10.0.0.2/24 |
| 🪟 Windows 10 IP Address | 10.0.0.10/24 |
| 🪟 Windows 11 IP Address | 10.0.0.11/24 |
| 🪟 Windows 7 IP Address | 10.0.0.7/24 |
| 🖥️ Windows Server 2016 IP Address | 10.0.0.16/24 |
| 📱 Android 9 IP Address | 10.0.0.9/24 |
| 🚪 Default Gateway | 10.0.0.1 |
| 🌍 DNS Server | 8.8.8.8 |

---

## 🪜 Lab Setup Procedure

**Step 1. Install 7-Zip**
Used to extract the Kali Linux virtual machine package.

**Step 2. Install VirtualBox**
Installed as the hypervisor for the entire lab.

**Step 3. Create the NAT Network**
- Network Name: `NatNetwork`
- IPv4 Prefix: `10.0.0.0/24`
- DHCP: Enabled
- IPv6: Disabled

A NAT Network was used instead of a standard NAT adapter because multiple VMs on the same NAT Network can communicate with each other while still having outbound internet access.

**Step 4. Import Kali Linux and Install Windows VMs**
Kali Linux, Windows 10, Windows 7, Windows Server 2016, and Android 9 were installed and each attached to `NatNetwork`.

**Step 5. Configure Static IP Addressing**
Each machine was assigned a static IP matching the configuration table above.

**Step 6. Verify Cross-VM Connectivity**
Ping tests were run between Kali and each Windows VM to confirm two-way connectivity.

**Step 7. Create Clean VM Snapshots**
A snapshot was taken of each VM to preserve a known-good recovery baseline.

---

## 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
|---|---|---|
| 🌐 Check IP address | `ip a` / `ipconfig` | Correct IP displayed |
| 📡 Test gateway | `ping 10.0.0.1` | Successful replies |
| 🌍 Test Internet connectivity | `ping 8.8.8.8` | Successful replies |
| 🔎 Test DNS resolution | `nslookup networkwalks.com` | Domain resolves |
| 🔄 Test cross-VM connectivity | `ping 10.0.0.x` | Successful replies both directions |
| 🧰 Verify Nmap | `nmap --version` | Nmap version displayed |
| 🔄 Verify snapshot | Restore snapshot → `ip a` | Baseline restored |

---

## 🐞 Problems Encountered & Solutions

### Problem 1 — Kali Slow Boot / Connectivity Issue (Kali 2026.1+)
Static IP configuration triggered IPv4 duplicate address detection (DAD) delays.

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

### Problem 2 — Windows VM Receiving Wrong IP Range (10.0.2.x instead of 10.0.0.x)
The NAT Network's IPv4 prefix had reverted to its default (10.0.2.0/24), and its DHCP config hadn't regenerated after editing in the GUI.

```bash
VBoxManage natnetwork remove --netname NatNetwork
VBoxManage natnetwork add --netname NatNetwork --network "10.0.0.0/24" --enable --dhcp on
```

### Problem 3 — One-Way Ping Failure (Windows Not Responding to Kali)
Windows Defender Firewall blocks inbound ICMPv4 Echo Requests by default. Enabling the built-in rule **"File and Printer Sharing (Echo Request - ICMPv4-In)"** for all profiles resolved this.

---

## 💡 What I Learned

- **NAT vs NAT Network** — a NAT Network lets multiple VMs communicate with each other while keeping outbound internet access, unlike isolated NAT.
- **Virtual network troubleshooting** — VirtualBox's NAT Network DHCP doesn't always regenerate automatically after a prefix change; recreating it via `VBoxManage` is the reliable fix.
- **Windows Firewall behavior** — inbound ICMP is blocked by default, which can look like a network fault even when addressing is correct.
- **Static IP configuration** — across both Linux and Windows systems.
- **VM snapshots** — always take a clean baseline before risky changes.
- **Documentation** — clearly recording commands, problems, and solutions is a core professional skill.

---

## 🔐 Security & Ethical Use

This laboratory is intended strictly for education purposes only.

---

## 🔗 Tools & Resources

- [7-Zip](https://7-zip.org/download.html)
- [VirtualBox](https://virtualbox.org/wiki/Downloads)
- [Kali Linux](https://kali.org/get-kali)

---

## 👤 Author

**Aryan Doshi**
Cybersecurity Intern

[GitHub](https://github.com/johnwick-2103) • [LinkedIn](https://www.linkedin.com/in/aryan-doshi-530500351)

---

📌 **Project Information:** Cybersecurity Internship | Expanded Cybersecurity & Pentesting Lab Setup
