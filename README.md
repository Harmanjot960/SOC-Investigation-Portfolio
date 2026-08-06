# SOC Investigation Portfolio

Hands-on SOC analyst investigations demonstrating practical experience in threat detection, incident response, malware analysis, network threat hunting, endpoint investigation, and Active Directory security analysis.

This portfolio contains security investigations completed through a personal SOC Home Lab, security labs, and publicly available security datasets.

[Projects](#projects) • [Investigation Sources](#investigation-sources--lab-environments) • [Skills](#skills--technologies) • [Reports](#repository-structure)

---

# Skills & Technologies

![SIEM](https://img.shields.io/badge/SIEM-Splunk-blue)
![Endpoint](https://img.shields.io/badge/Endpoint-Sysmon-orange)
![Network](https://img.shields.io/badge/Network-Wireshark-blue)
![IDS](https://img.shields.io/badge/IDS-Suricata-red)
![Threat Intel](https://img.shields.io/badge/Threat%20Intel-VirusTotal-green)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-purple)

---

# Projects Overview

| Project | Investigation Focus | Tools | Status |
|----------|---------------------|-------|--------|
| Project 1 | Phishing Email Investigation | Email Analysis, VirusTotal, Threat Intelligence | Complete |
| Project 2 | RDP Brute Force Detection | Splunk, Windows Security Logs, SPL | Complete |
| Project 3 | PowerShell Post-Compromise Investigation | Splunk, Sysmon, PowerShell Logs | Complete |
| Project 4 | Network Threat Hunting Investigation | Wireshark, Suricata, PCAP Analysis | Complete |
| Project 5 | Windows Endpoint Incident Response | Sysmon, Windows Logs, PowerShell, Wireshark | Complete |
| Project 6 | Active Directory Ransomware Investigation | Splunk, Sysmon, AD Logs, Windows Events | Complete |

---

# Overview

This repository contains a collection of Security Operations Center (SOC) investigations completed through security labs, training platforms, and publicly available security datasets.

The portfolio demonstrates practical experience across:

- Security monitoring
- Alert triage
- Threat hunting
- Endpoint investigation
- Network traffic analysis
- Malware investigation
- Incident response
- IOC extraction and validation
- Threat intelligence analysis
- MITRE ATT&CK mapping
- Technical reporting

Investigations cover multiple attack scenarios:

- Phishing email investigation
- RDP brute-force detection
- PowerShell post-compromise activity
- Malware network threat hunting
- Windows endpoint compromise
- Active Directory ransomware intrusion

---

# Investigation Methodology

Each investigation follows a structured SOC workflow:

```text
Alert / Incident Intake
        |
        ▼
Initial Triage
        |
        ▼
Evidence Collection
        |
        ▼
Threat Investigation
        |
        ▼
IOC Extraction & Validation
        |
        ▼
Threat Intelligence Analysis
        |
        ▼
Security Tool Investigation
        |
        ▼
MITRE ATT&CK Mapping
        |
        ▼
Incident Report Documentation
```

Additional investigation phases are included when applicable:

- Packet analysis
- Endpoint telemetry analysis
- Malware analysis
- IDS alert validation
- Authentication analysis
- Attack timeline reconstruction

Detailed investigation findings, technical analysis, evidence, timelines, and MITRE ATT&CK mappings are documented in each project's **Incident Report repository**.

---

# Investigation Sources & Lab Environments

Investigations were completed using multiple environments and security platforms.

| Source | Projects |
|---------------|----------|
| Personal SOC Home Lab | Project 2, Project 3 |
| Security Labs | Project 1, Project 5, Project 6 |
| Public Malware Traffic Analysis Dataset | Project 4 |

The personal SOC Home Lab was used for:

- Windows security telemetry collection
- PowerShell logging
- Sysmon event analysis
- Splunk investigations
- Detection query development

Project 4 network threat hunting was performed using a publicly available PCAP dataset analyzed with Wireshark and Suricata.

---

# Tools & Technologies

| Category | Tools & Technologies |
|----------|---------------------|
| SIEM & Log Analysis | Splunk Enterprise, SPL Queries, Windows Event Viewer, Windows Security Logs |
| Endpoint Investigation | Sysmon, PowerShell Script Block Logging, Process Analysis, File and Registry Analysis |
| Windows Security Telemetry | Authentication Logs, User/Group Changes, SMB Access Events, Service Creation Events |
| Network Investigation | Wireshark, PCAP Analysis, Suricata IDS, DNS Analysis, HTTP Analysis, TLS Analysis |
| Threat Intelligence & Malware Analysis | VirusTotal, URLScan, MalwareBazaar, Hybrid Analysis, ANY.RUN, WHOIS, IP Reputation Analysis |
| Frameworks | MITRE ATT&CK Framework |

---

# Repository Structure

Each investigation follows a consistent structure while allowing additional repositories depending on investigation requirements.

| Directory | Purpose |
|----------|---------|
| README | Investigation overview, scenario, workflow, findings, timeline, MITRE mapping, screenshots |
| Evidence | Investigation artifacts including IOCs, timelines, technical findings, and supporting evidence |
| Incident Report | Complete investigation documentation including analysis, impact assessment, recommendations, and analyst conclusion |
| MITRE ATT&CK | Mapping of observed attacker behavior to ATT&CK techniques |
| Screenshots | Visual evidence supporting investigation findings |

Additional repositories may include:

- SPL Queries
- PCAP
- Sysmon
- Windows Event Logs
- Suricata
- Threat Intelligence

---

# Windows Security & Sysmon Event Coverage

The investigations include analysis of Windows security telemetry commonly used by SOC analysts.

## Windows Security Events

- Event ID 4624 — Successful Logon
- Event ID 4625 — Failed Logon
- Event ID 4672 — Special Privileges Assigned
- Event ID 4720 — User Account Created
- Event ID 4728 / 4732 — Security Group Membership Changes
- Event ID 5140 — Network Share Access
- Event ID 5145 — Detailed File Share Access
- Event ID 7045 — Service Installation

## PowerShell Logging

- Event ID 4104 — PowerShell Script Block Logging

## Sysmon Events

- Event ID 1 — Process Creation
- Event ID 3 — Network Connection
- Event ID 10 — Process Access
- Event ID 11 — File Creation
- Event ID 13 — Registry Value Set
- Event ID 15 — FileCreateStreamHash
- Event ID 22 — DNS Query
