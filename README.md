# Implementation-of-a-Simulated-Security-Operation-Center-for-threat-detection-and-incident-Response
documentation of my 18 week internship project - which is design and Implementation of a Simulated Security Operation Center for threat detection and incident Response.

# 🛡️ Design and Implementation of a Simulated Security Operations Center (SOC) Environment for Threat Detection and Incident Response

![Wazuh](https://img.shields.io/badge/SIEM-Wazuh_v4.7.5-blue)
![MITRE ATT&CK](https://img.shields.io/badge/Framework-MITRE_ATT%26CK-red)
![Platform](https://img.shields.io/badge/Platform-VirtualBox-lightgrey)
![Duration](https://img.shields.io/badge/Duration-18_Weeks-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📋 Overview

This repository documents my **18-week industry internship project** completed at **SS Infotech Pvt. Ltd., Nagpur** as part of the B.Tech Computer Science and Engineering programme at **Symbiosis Institute of Technology, Nagpur** (Symbiosis International Deemed University).

The project involved the end-to-end design, deployment, and validation of a fully functional **Simulated Security Operations Center (SOC)** environment using open-source tools on a consumer virtualisation platform. The lab replicates the core operational cycle of a real enterprise SOC — log ingestion, threat detection, alert triage, automated response, compliance monitoring, and dashboard visualisation — all mapped to the **MITRE ATT&CK Enterprise Framework**.

> **Industry Mentor:** Mr. Viraj Patle, SS Infotech Pvt. Ltd., Nagpur  
> **Institute Mentor:** Dr. Shreyas Hole, Assistant Professor, Dept. of CSE, SIT Nagpur  
> **Internship Period:** January 2026 – May 2026

---

## 🎯 Project Objectives

1. Deploy Wazuh SIEM (Manager + Indexer + Dashboard) on Ubuntu Server 24.04 LTS
2. Integrate a Windows 11 endpoint as a monitored Wazuh Agent
3. Simulate and detect **failed Windows login attempts** (MITRE T1078)
4. Simulate and detect **malware presence** using the EICAR test file (MITRE T1204.002)
5. Validate **File Integrity Monitoring** on a sensitive Windows system directory
6. Execute and detect an **SSH brute force attack** using Hydra v9.5 (MITRE T1110)
7. Author and deploy **custom Wazuh detection rules** with frequency-based correlation logic
8. Implement **automated Active Response** with iptables IP blocking
9. Detect **network reconnaissance** via Nmap TCP Connect scan (MITRE T1046)
10. Perform a **Security Configuration Assessment** against CIS Windows 11 Enterprise Benchmark v1.0.0

---

## 🏗️ Lab Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│              VirtualBox Bridged Adapter — 192.168.1.0/24        │
├─────────────────┬───────────────────────┬───────────────────────┤
│   WAZUH SERVER  │   WINDOWS ENDPOINT    │     ATTACKER VM       │
│                 │                       │                       │
│ Ubuntu 24.04    │ Windows 11 Home       │ Ubuntu 22.04 LTS      │
│ 192.168.1.6     │ 192.168.1.x (DHCP)    │ 192.168.1.7 (DHCP)    │
│ (Static/Netplan)│                       │                       │
│ Wazuh v4.7.5    │ Wazuh Agent v4.7.5    │ Hydra v9.5            │
│ Manager         │ Agent ID: 002         │ Nmap 7.94SVN          │
│ Indexer         │ FIM + SCA + SecEvents │ Medusa v2.2           │
│ Dashboard       │                       │                       │
│ 6GB RAM / 25GB  │ Host Machine          │ 4GB RAM / 20GB        │
└─────────────────┴───────────────────────┴───────────────────────┘
```

**Host Machine:** Windows 11, AMD Ryzen 7 6800H, 16GB RAM, 500GB NVMe SSD  
**Hypervisor:** Oracle VirtualBox 7.x, Bridged Adapter Mode  
**Network:** All VMs on same subnet, direct IP-to-IP communication

---

## 🔬 Attack Simulations Executed

| # | Simulation | MITRE Technique | Primary Rule | Alert Level | Result |
|---|-----------|----------------|-------------|-------------|--------|
| 1 | Failed Windows Login Detection | T1078 – Valid Accounts | Rule 60122 / Event ID 4625 | Level 5 | ✅ Alert |
| 2 | EICAR Malware Detection | T1204.002 – Malicious File | Rule 87105 / Event ID 1116 | Level 12 | ✅ Alert |
| 3 | File Integrity Monitoring | T1565 – Data Manipulation | Rule 554 | Level 5 | ✅ Alert |
| 4 | SSH Brute Force via Hydra | T1110 – Brute Force | Rule 5763 | Level 10 | ✅ Alert |
| 5 | Custom Detection Rule 100002 | T1078 – Valid Accounts | Rule 100002 | Level 10 | ✅ Alert |
| 6 | Automated Active Response – IP Block | T1110 – Brute Force | Rule 5763 + firewall-drop | — | ✅ IP Block |
| 7 | Nmap Port Scan Detection | T1046 – Network Discovery | Rule 100003 / UFW BLOCK | Level 7 | ✅ Syslog |
| 8 | SCA – CIS Windows 11 Benchmark | — | CIS W11 Enterprise v1.0.0 | — | ✅ 32% Score |
| 9 | Custom OpenSearch Dashboard | — | wazuh-alerts-* index | — | ✅ 3 Panels |

---

## 🗺️ MITRE ATT&CK Coverage

| Tactic | Technique | Detection | Coverage |
|--------|-----------|-----------|----------|
| Initial Access | T1078 – Valid Accounts | Rules 60122, 100002 | ✅ Full |
| Credential Access | T1110 – Brute Force | Rule 5763 + Active Response | ✅ Full |
| Execution | T1204.002 – Malicious File | Rule 87105 | ✅ Full |
| Impact | T1565 – Data Manipulation | Rule 554 (FIM) | ✅ Full |
| Discovery | T1046 – Network Service Discovery | Rule 100003 (UFW) | ⚠️ Partial |

---

## 🛠️ Tools and Technologies

| Tool | Version | Purpose |
|------|---------|---------|
| Wazuh Manager | 4.7.5 | SIEM core — log ingestion, rule evaluation, active response |
| Wazuh Indexer (OpenSearch) | 4.7.5 | Distributed alert storage and search |
| Wazuh Dashboard | 4.7.5 | Alert investigation, SCA results, custom dashboards |
| Wazuh Agent | 4.7.5 | Windows 11 endpoint monitoring |
| Oracle VirtualBox | 7.x | Type-2 hypervisor |
| Ubuntu Server | 24.04 LTS | Wazuh server OS |
| Ubuntu Desktop | 22.04 LTS | Attacker VM OS |
| Windows 11 Home | 10.0.26200 | Monitored endpoint |
| Hydra | v9.5 | SSH brute force simulation |
| Nmap | 7.94SVN | TCP Connect port scan simulation |
| Medusa | v2.2 | Backup brute force tool |
| UFW + iptables | Ubuntu native | Firewall and active response blocking |
| Netplan | Ubuntu native | Static IP configuration |
| PowerShell | 5.1+ | Agent management and simulation scripts |

---

## 📅 18-Week Timeline Summary

| Phase | Weeks | Focus |
|-------|-------|-------|
| Foundations | 1–4 | Linux fundamentals, networking, log analysis, SOC theory, MITRE ATT&CK |
| SIEM Deployment | 5–7 | Wazuh architecture study, deployment, agent integration and stabilisation |
| Conference | 8 | Nullcon Goa 2026 — professional development |
| Attack Simulations | 9–13 | EICAR, failed login, FIM, SSH brute force, custom detection rules |
| Advanced Features | 14–17 | Active response, Nmap detection, SCA assessment, custom dashboard |
| Final Demo | 18 | Consolidation, live demonstration, report completion |

---

## ⚙️ Key Technical Implementations

### Static IP Configuration via Netplan
```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.1.6/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```

### Custom Detection Rule 100002 (Frequency-Based Correlation)
```xml
<group name="local,windows,authentication,">
  <rule id="100002" level="10" frequency="5" timeframe="120">
    <if_matched_sid>60122</if_matched_sid>
    <description>Custom Rule: Multiple failed login attempts detected
    on Windows endpoint (Event ID 4625)</description>
    <mitre>
      <id>T1078</id>
    </mitre>
    <group>authentication_failed,windows,</group>
  </rule>
</group>
```

### Active Response Configuration
```xml
<active-response>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5763</rules_id>
  <timeout>180</timeout>
</active-response>
```

### SSH Brute Force Attack (Hydra)
```bash
hydra -l tirthak -P ~/passwords.txt ssh://192.168.1.6 -t 4 -V
```

---

## 📊 Key Results

- **Total Alerts Generated:** 12,605 across all simulations
- **Alert Distribution:** Level 5 (10,800) | Level 7 (580) | Level 10 (780) | Level 12 (25)
- **SCA Compliance Score:** 32% against CIS Windows 11 Enterprise Benchmark v1.0.0 (395 checks)
- **Active Response:** Automated iptables DROP firing within seconds of Rule 5763 trigger, 180s auto-unblock
- **MITRE ATT&CK Tactic Distribution:** Defense Evasion (1,151) | Impact (444) | Privilege Escalation (400) | Initial Access (359)

---

## 📁 Repository Contents

```
├── Tirthak_Likhar_22070521041_A_WEEK_1.pdf    # Linux Fundamentals
├── Tirthak_Likhar_22070521041_A_WEEK_2.pdf    # Networking and Log Analysis
├── Tirthak_Likhar_22070521041_A_WEEK_3.pdf    # Linux Security Fundamentals
├── Tirthak_Likhar_22070521041_A_WEEK_4.pdf    # SOC Fundamentals and MITRE ATT&CK
├── Tirthak_Likhar_22070521041_A_WEEK_5.pdf    # SIEM Architecture and Wazuh Prep
├── Tirthak_Likhar_22070521041_A_WEEK_6.pdf    # Wazuh SIEM Deployment
├── Tirthak_Likhar_22070521041_A_WEEK_7.pdf    # Agent Integration and Stabilisation
├── Tirthak_Likhar_22070521041_A_WEEK_8.pdf    # Nullcon Goa 2026
├── Tirthak_Likhar_22070521041_A_WEEK_9.pdf    # EICAR Malware Detection (Sim 1)
├── Tirthak_Likhar_22070521041_A_WEEK_10.pdf   # Failed Windows Login (Sim 2)
├── Tirthak_Likhar_22070521041_A_WEEK_11.pdf   # Static IP + FIM Simulation (Sim 3)
├── Tirthak_Likhar_22070521041_A_WEEK_12.pdf   # SSH Brute Force via Hydra (Sim 4)
├── Tirthak_Likhar_22070521041_A_WEEK_13.pdf   # Custom Detection Rule 100002 (Sim 5)
├── Tirthak_Likhar_22070521041_A_WEEK_14.pdf   # Active Response – IP Blocking (Sim 6)
├── Tirthak_Likhar_22070521041_A_WEEK_15.pdf   # Nmap Port Scan Detection (Sim 7)
├── Tirthak_Likhar_22070521041_A_WEEK_16.pdf   # SCA CIS Benchmark (Sim 8)
├── Tirthak_Likhar_22070521041_A_WEEK_17.pdf   # Custom Security Dashboard (Sim 9)
└── Tirthak_Likhar_22070521041_A_WEEK_18.pdf   # Final Demo and Consolidation
```

---

## 🔗 Connect

**Tirthak Girish Likhar**  
B.Tech Computer Science and Engineering  
Symbiosis Institute of Technology, Nagpur  
PRN: 22070521041

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://www.linkedin.com/in/tirthak-likhar-8808a8255/)

---

## 📄 License

This repository is for educational and portfolio purposes. All attack simulations were performed in an isolated lab environment with no external systems involved.
