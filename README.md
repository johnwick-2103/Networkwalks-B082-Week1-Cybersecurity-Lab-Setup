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
<div align="center">

<h1>PENETRATION TESTING REPORT</h1>

<h2>FOOTPRINTING, PASSIVE RECONNAISSANCE & NETWORK SCANNING</h2>

<h3>W2-PM1 | W2-PM4 | W2-PM5 | CYBERSECURITY | NETWORKWALKS</h3>
<h3>W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS</h3>

<br>

<img src="https://img.shields.io/badge/Status-Completed-brightgreen">
<img src="https://img.shields.io/badge/Program-Networkwalks-0099cc">
<img src="https://img.shields.io/badge/Week-02-blue">

<br>

<img src="https://img.shields.io/badge/Modules-3-blue">
<img src="https://img.shields.io/badge/Type-Pentest%20Report-red">

<br>

<img src="https://img.shields.io/badge/Tool-Windows%2010-blue">
<img src="https://img.shields.io/badge/Tool-Zenmap-yellow">
<img src="https://img.shields.io/badge/Tool-Nmap-brightgreen">

<br>

<img src="https://img.shields.io/badge/Format-Markdown-lightgrey">
<img src="https://img.shields.io/badge/Platform-GitHub-black">
<img src="https://img.shields.io/badge/License-Educational%20Use%20Only-purple">

</div>

<table>
  <tr>
    <td align="center"><b>Pentester Name<br>(Cybersecurity Trainee)</b></td>
    <td><b>Aryan</b></td>
  </tr>

  <tr>
    <td align="center"><b>Program/Batch</b></td>
    <td>B082-Networkwalks</td>
  </tr>

  <tr>
    <td align="center"><b>Date</b></td>
    <td>21 August 2026</td>
  </tr>

  <tr>
    <td align="center"><b>Modules Completed</b></td>
    <td>
      W2-PM1 – Footprinting & Reconnaissance with Multiple Kali Tools<br>
      W2-PM4 – Footprinting & Reconnaissance with theHarvester<br>
      W2-PM5 – Network Scanning with Zenmap
    </td>
  </tr>

  <tr>
    <td align="center"><b>Client/Target</b></td>
    <td>
      1. networkwalks.com – Authorized educational target<br>
      2. microsoft.com – Passive reconnaissance using public sources<br>
      3. My own local LAN network – 192.168.24.0/24 (Ethernet)<br>
      4. My own local LAN network – 10.12.74.0/24 (Wi-Fi)
    </td>
  </tr>

  <tr>
    <td align="center"><b>Permission secured?</b></td>
    <td>Yes – All activities were performed within an authorized educational scope or on my own local network.</td>
  </tr>

  <tr>
  <td align="center"><b>Phases Covered</b></td>
  <td>
    <b>Phase 1:</b> Reconnaissance & Footprinting – W2-PM1<br>
    <b>Phase 2:</b> Passive OSINT & Information Gathering with theHarvester – W2-PM4<br>
    <b>Phase 3:</b> Email, Host & Subdomain Harvesting – W2-PM4<br>
    <b>Phase 4:</b> Network Discovery & Scanning with Zenmap – W2-PM5<br>
    <b>Phase 5:</b> In Progress
  </td>
</tr>
</table>

## 1. Liability Disclaimer

All activities documented in this report were performed strictly for **educational and cybersecurity research purposes**.

The **Footprinting & Reconnaissance with Multiple Kali Tools (W2-PM1)** activities were conducted against `networkwalks.com` within the scope of the assigned cybersecurity training environment.

The **Footprinting & Reconnaissance with theHarvester (W2-PM4)** activity was performed against `microsoft.com` using publicly available information sources as part of a **passive reconnaissance exercise**. No attempt was made to gain unauthorized access, bypass security controls, or modify any data.

The **Network Scanning with Zenmap (W2-PM5)** activities were performed only on **my own local LAN network and devices under my control**.

No unauthorized access, exploitation, or modification of systems was performed during these exercises.

> **⚠️ Important:** The tools and techniques demonstrated in this report must only be used for legitimate educational, research, and authorized security testing purposes. Unauthorized access, scanning, exploitation, or misuse may violate applicable laws and regulations.

All activities were completed as part of assigned cybersecurity training exercises. Any misuse of the techniques described in this report is the sole responsibility of the individual performing such actions.

## 2. Introduction

This report documents three practical cybersecurity activities completed as part of the Week 2 project modules: **Footprinting & Reconnaissance with Multiple Kali Tools (W2-PM1)**, **Footprinting & Reconnaissance with theHarvester (W2-PM4)**, and **Network Scanning with Zenmap (W2-PM5)**.

The first activity focused on gathering publicly available information about `networkwalks.com` using **WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, and DNSRecon**. These tools were used to identify domain registration details, web technologies, IP addresses, HTTP response headers, Web Application Firewall information, and DNS records.

The second activity involved using **theHarvester** against `microsoft.com` to perform passive reconnaissance. Publicly available sources were used to collect information such as email addresses, hosts, and subdomains related to the target organization.

The third activity focused on **local network discovery using Zenmap**, the graphical interface for Nmap. My local IP address and LAN subnet were identified, followed by a Ping Scan of the `192.168.24.0/24` network. The scan identified active hosts, their IP and MAC addresses, and a visual network topology was generated.

All activities were performed within an authorized educational scope. The purpose of these exercises was to understand how reconnaissance and network scanning techniques can be used to gather information about systems and networks before later stages of a penetration testing process.

## 3. Tools Used

The table below lists each tool used in this report and its purpose.

| **Tool** | **Purpose** |
|---|---|
| **Kali Linux** | Operating system used to perform the footprinting and reconnaissance activities. |
| **WHOIS** | Used to obtain domain registration details such as registrar information, registration dates, and name servers. |
| **WhatWeb** | Used to fingerprint web technologies such as the web server, CMS, plugins, frameworks, and IP address. |
| **Nslookup** | Used to resolve a domain name to its corresponding IP address using DNS. |
| **Curl (`curl -I`)** | Used to inspect HTTP response headers and identify technical information exposed by the web server. |
| **Wafw00f** | Used to detect whether the target website is protected by a Web Application Firewall (WAF). |
| **DNSRecon** | Used to enumerate DNS records such as NS, MX, TXT, SPF, SRV, and other DNS-related information. |
| **theHarvester** | Used for passive reconnaissance to collect publicly available information such as email addresses, hosts, and subdomains related to a target organization. |
| **Windows 10** | Operating system used for the local network scanning activity with Zenmap. |
| **Windows CMD** | Used to identify the local IP address, subnet mask, default gateway, and MAC address using commands such as `ipconfig` and `ipconfig /all`. |
| **Zenmap** | Graphical user interface for Nmap used to scan the local LAN subnet, discover active hosts, identify IP and MAC addresses, and generate a network topology. |
| **Nmap** | Network scanning engine used by Zenmap to perform host discovery and Ping Scan operations on the local network. |

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance

I performed footprinting and reconnaissance activities against `networkwalks.com` using multiple Kali Linux tools. The objective of this activity was to collect publicly available technical information about the target and understand its external infrastructure.

The following tools were used:

- **WHOIS** – to collect domain registration information, registrar details, registration dates, and name servers.
- **WhatWeb** – to fingerprint technologies used by the target website.
- **Nslookup** – to resolve the target domain to its IP address.
- **Curl (`curl -I`)** – to inspect HTTP response headers.
- **Wafw00f** – to detect whether the website was protected by a Web Application Firewall.
- **DNSRecon** – to enumerate DNS records and other DNS-related information.

Using `nslookup`, the domain `networkwalks.com` was resolved to:

`192.232.216.135`

The HTTP response headers collected using Curl returned an **HTTP/2 200** response and exposed technical information related to the web server and WordPress application.

Wafw00f identified the following Web Application Firewall:

`ModSecurity (SpiderLabs)`

DNSRecon provided additional information about the target's DNS infrastructure, including name servers, mail-related records, TXT/SPF records, and service records.

These activities demonstrated how different reconnaissance tools can be combined to create a technical profile of a target using publicly available information.

---

### 4.2 Footprinting & Reconnaissance with theHarvester

I performed passive reconnaissance against `microsoft.com` using **theHarvester** in Kali Linux.

The objective of this activity was to gather publicly available information related to the target organization from external data sources.

For the first task, I used **Baidu** as the data source and set the maximum number of results to 1000.

The following command was executed:

`theHarvester -d microsoft.com -l 1000 -b baidu`

In this command:

- `-d` specifies the target domain.
- `-l` specifies the maximum number of results.
- `-b` specifies the data source.

For the second task, I used all supported sources and limited the maximum number of results to 50.

The following command was executed:

`theHarvester -d microsoft.com -l 50 -b all`

theHarvester can collect publicly available information related to a target organization, including:

- Email addresses
- Subdomains
- Hosts
- Employee-related information
- Other publicly exposed infrastructure information

The activity demonstrated how **passive reconnaissance** can be used to gather information from public sources without attempting to gain unauthorized access to the target systems.

The collected information can help cybersecurity professionals understand an organization's publicly exposed information and evaluate its external attack surface.

---

### 4.3 Network Scanning with Zenmap

For the network scanning activity, I used **Zenmap**, the graphical interface for Nmap, to perform host discovery on my own local LAN network.

First, I used the Windows `ipconfig` command to identify my local network configuration.

The following information was identified:

- **Local IPv4 Address:** `192.168.24.70`
- **Subnet Mask:** `255.255.255.0`
- **LAN Subnet:** `192.168.24.0/24`
- **Default Gateway:** `192.168.24.11`

**Additional Network (Wi-Fi):**
- **Wi-Fi IPv4 Address:** `10.12.74.54`
- **Subnet Mask:** `255.255.255.0`
- **Wi-Fi Subnet:** `10.12.74.0/24`
- **Wi-Fi Gateway:** `10.12.74.168`

The following subnet was entered as the target in Zenmap:

`192.168.24.0/24`

A Ping Scan was performed using:

`nmap -sn 192.168.24.0/24`

The scan identified **1 live host**:

- `192.168.24.70`

The following MAC address was identified:

| **IP Address** | **MAC Address** | **Adapter** |
|---|---|---|
| `192.168.24.70` | `0A-00-27-00-00-0F` | VirtualBox Host-Only Ethernet Adapter |

The MAC address of my own computer was verified separately using:

`ipconfig /all`

**Additional MAC Addresses from my system:**

| **Adapter** | **IP Address** | **MAC Address** |
|---|---|---|
| Ethernet (Realtek) | Media Disconnected | BC-FC-E7-D1-3E-6D |
| Ethernet 2 (VirtualBox) | 192.168.24.70 | 0A-00-27-00-00-0F |
| Wi-Fi | 10.12.74.54 | F8-3D-C6-86-3D-78 |

After completing the scan, I opened the **Topology** section in Zenmap to visually display the discovered devices and saved the topology in PDF format.

**Note:** The scan only showed 1 host because:
1. My Ethernet adapter is a VirtualBox Host-Only adapter
2. Other devices may be on the Wi-Fi network (10.12.74.0/24)
3. Firewalls on other devices may block ping requests

This activity demonstrated how network scanning can be used to identify active hosts, IP addresses, MAC addresses, and the basic topology of a local network.

---

## 5. Risk Analysis / Impact

Based on the information collected during the **footprinting, passive reconnaissance, and network scanning activities**, the following potential security risks and observations were identified.

| # | **Risk / Finding** | **Evidence / Observation** | **Potential Impact** | **Risk Level** |
|---|-------------------|---------------------------|---------------------|----------------|
| 1 | Public domain and infrastructure information exposed | WHOIS revealed registrar details, registration dates, and HostGator name servers for `networkwalks.com`. | Public infrastructure information can assist attackers in building a detailed profile of the target during reconnaissance. | 🟡 Low |
| 2 | Server IP address identifiable | Nslookup resolved `networkwalks.com` to `192.232.216.135`. | The public server IP can be used to map the target's infrastructure and support further authorized enumeration. | 🟡 Low |
| 3 | HTTP technical information exposed | `curl -I` returned HTTP headers showing **Apache**, WordPress-related information, cookies, and the `/wp-json/` API endpoint. | Technical information may help an attacker fingerprint the web application and identify additional areas for investigation. | 🟡 Low |
| 4 | WAF technology identifiable | Wafw00f identified **ModSecurity (SpiderLabs)** protecting `networkwalks.com`. | Identifying the WAF reveals information about the website's defensive architecture and may help an attacker adapt later reconnaissance attempts. | 🟡 Low |
| 5 | DNS infrastructure information exposed | DNSRecon identified name servers, an A record, MX records, TXT/SPF records, SRV records, and other DNS-related information. | DNS information can help build a broader picture of the organization's externally exposed infrastructure and services. | 🟠 Medium |
| 6 | Active host discovered on the local network | Zenmap identified **1 live host**: `192.168.24.70`. | Network discovery can reveal active devices and network structure. Unexpected devices should be verified by the network owner. | 🟠 Medium |
| 7 | MAC address information discoverable | Zenmap and `ipconfig /all` identified MAC addresses associated with devices on the local LAN. | MAC address information can help identify devices and network hardware during internal reconnaissance. | 🟡 Low |
| 8 | Multiple network interfaces active | Two active connections: Ethernet (192.168.24.70) and Wi-Fi (10.12.74.54). | Multiple active interfaces may increase the attack surface and create additional paths for network access. | 🟠 Medium |
| 9 | VirtualBox adapter present | Ethernet 2 is a VirtualBox Host-Only adapter with MAC `0A-00-27-00-00-0F`. | Virtual network adapters may indicate the presence of virtual machines that could be further investigated. | 🟡 Low |
| 10 | Public email addresses discoverable | theHarvester using Baidu identified **6 email addresses** related to `microsoft.com`. | Publicly available email addresses may be used in phishing, social engineering, or credential-targeting campaigns. | 🟠 Medium |
| 11 | Public hosts and subdomains discoverable | theHarvester identified **15 hosts** related to `microsoft.com` during passive reconnaissance. | Publicly exposed hosts and subdomains increase the visible attack surface and may provide additional targets for further authorized security assessment. | 🟠 Medium |

**Risk Level Key:** 🔴 Critical | 🟠 Medium | 🟡 Low

> **Note:** The findings above are reconnaissance and network-discovery observations and should not automatically be considered confirmed vulnerabilities. No exploitation or vulnerability validation was performed during these project modules.

The `theHarvester -b all` activity also produced several **missing API key** messages for some external data sources. This is a limitation of the reconnaissance results rather than a vulnerability in the target, because some sources could not be queried successfully.

## 6. Recommendations

Based on the observations from the **footprinting, passive reconnaissance, and network scanning activities**, the following security recommendations are suggested:

1. **Review Publicly Exposed Information**  
   Organizations should regularly review what information about their domains, servers, technologies, and infrastructure is publicly accessible.

2. **Keep Web Technologies Updated**  
   Web servers, CMS platforms, plugins, and other technologies should be regularly updated and checked against current security advisories.

3. **Review HTTP Response Headers**  
   HTTP headers should be reviewed to ensure that unnecessary technical information about the server, framework, or application is not being exposed.

4. **Maintain and Monitor the Web Application Firewall**  
   The WAF should remain properly configured, updated, and monitored to help detect and block malicious web traffic.

5. **Review DNS Records Regularly**  
   DNS records such as NS, MX, TXT, SPF, and SRV records should be periodically reviewed to ensure that only necessary information and services are publicly exposed.

6. **Reduce Unnecessary Public Email Exposure**  
   Organizations should monitor publicly available email addresses because exposed addresses may become targets for phishing and social engineering attacks.

7. **Monitor Public Subdomains and Hosts**  
   Publicly discoverable subdomains and hosts should be regularly inventoried and reviewed. Unused or outdated services should be removed or secured to reduce the external attack surface.

8. **Perform Regular Internal Network Discovery**  
   Organizations should periodically scan their own authorized networks to identify active devices and maintain awareness of systems connected to the network.

9. **Investigate Unknown Devices**  
   Any unexpected or unauthorized device discovered during internal network scanning should be identified and investigated.

10. **Maintain Updated Network Documentation**  
    IP addresses, MAC addresses, devices, network topology, and other infrastructure information should be documented and kept up to date.

11. **Limit Unnecessary Information Exposure**  
    Organizations should minimize publicly available technical information wherever possible because reconnaissance data can assist attackers in planning later stages of an attack.

12. **Review Virtual Network Adapters**  
    Virtual network adapters (such as VirtualBox) should be reviewed and secured to ensure they do not create unintended network exposure.

13. **Monitor Multiple Active Network Connections**  
    Systems with multiple active network connections should be reviewed to ensure they do not create additional security risks.

14. **Perform Security Testing Only with Authorization**  
    Reconnaissance, scanning, and other cybersecurity testing activities should only be performed against systems and networks where proper authorization and an agreed scope have been established.

## 7. Conclusion

During the Week 2 cybersecurity project activities, I completed practical exercises covering **footprinting, reconnaissance, passive information gathering, and local network scanning**.

In the **W2-PM1 Footprinting & Reconnaissance** activity, I used multiple Kali Linux tools including **WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, and DNSRecon** to collect publicly available information about `networkwalks.com`. These tools helped identify domain registration details, web technologies, IP information, HTTP response headers, Web Application Firewall information, and DNS records.

In the **W2-PM4 theHarvester** activity, I performed passive reconnaissance against `microsoft.com` using publicly available data sources. The exercise demonstrated how email addresses, hosts, and subdomains can be discovered without attempting to gain unauthorized access to the target systems.

In the **W2-PM5 Network Scanning with Zenmap** activity, I identified my local network configuration and performed a Ping Scan against the `192.168.24.0/24` subnet. The scan discovered **1 live host** (`192.168.24.70`), and I collected its IP and MAC address information. I also identified that I have a second active network connection (Wi-Fi) on `10.12.74.54/24`. I generated a visual network topology using Zenmap and saved it as a PDF.

These exercises demonstrated that a significant amount of useful security information can be gathered before any exploitation takes place. Reconnaissance and network discovery help security professionals understand the visible attack surface, identify exposed information, and build a clearer picture of an environment.

I also learned the importance of documenting each step clearly, including the tools used, commands executed, results observed, potential risks, and recommended security improvements.

All activities were performed within an authorized educational scope, and no unauthorized exploitation or access was attempted.


# -End-

## 👤 Author

**Aryan**  
Cybersecurity Trainee | B082  
Networkwalks Cybersecurity Program

## 📌 Project Information

**Program Name:** Cybersecurity Program at Networkwalks  
**Week:** 02  
**Modules Completed:** W2-PM1 | W2-PM4 | W2-PM5  
**Repository:** GitHub  
**Project Area:** Footprinting, Reconnaissance & Network Scanning
