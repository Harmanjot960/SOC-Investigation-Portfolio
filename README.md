# SOC Investigation Portfolio

This repository contains a collection of Security Operations Center (SOC) investigations completed through a personal SOC Home Lab, security labs, and publicly available security datasets.

The projects demonstrate practical SOC analyst workflows including **alert triage, log analysis, security event investigation, evidence collection, threat hunting, endpoint and network analysis, IOC extraction and validation, threat intelligence research, detection development, incident response, MITRE ATT&CK mapping, and professional incident reporting**.

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

# Investigation Methodology

Each investigation follows a structured SOC workflow appropriate for the incident being analyzed.

```text
Alert / Incident Intake
        │
        ▼
Initial Triage
        │
        ▼
Evidence Collection
        │
        ▼
Threat Investigation
        │
        ▼
IOC Extraction & Validation
        │
        ▼
Threat Intelligence Analysis
        │
        ▼
Security Telemetry Analysis
        │
        ▼
MITRE ATT&CK Mapping
        │
        ▼
Incident Report Documentation
```

Depending on the investigation, additional phases such as packet analysis, endpoint telemetry correlation, malware behavior analysis, IDS alert validation, authentication analysis, and attack timeline reconstruction are incorporated into the workflow. Detailed investigation findings, technical analysis, evidence, timelines, and MITRE ATT&CK mappings are documented in each project's **Incident Report**.

---

# Investigation Sources & Lab Environments

Investigations were completed using multiple environments and security platforms.

| Source | Projects |
|--------|----------|
| Personal SOC Home Lab | Project 2, Project 3, Project 4 |
| TryHackMe Security Labs | Project 1, Project 5, Project 6 |

The personal SOC Home Lab was used for Windows security telemetry collection, PowerShell logging, Sysmon event analysis, Splunk investigations, detection query development, and network threat hunting using Wireshark and Suricata.

Project 4 utilized a publicly available PCAP dataset as the investigation evidence source, which was analyzed within the SOC Home Lab environment using Wireshark and Suricata.

TryHackMe Security Labs provided realistic enterprise attack scenarios involving phishing analysis, Windows endpoint compromise, and Active Directory ransomware investigations.

---

# Tools & Technologies

| Category | Tools & Technologies |
|----------|---------------------|
| SIEM & Log Analysis | Splunk Enterprise, SPL Queries, Windows Event Viewer, Windows Security Logs |
| Endpoint Investigation | Sysmon, PowerShell Script Block Logging, Process Analysis, File and Registry Analysis |
| Windows Security Telemetry | Authentication Logs, Privileged Access Events, User/Group Changes, SMB Access Events, Service Creation Events, Process Execution Events |
| Network Investigation | Wireshark, PCAP Analysis, Suricata IDS, DNS Analysis, HTTP Analysis, TLS Analysis |
| Threat Intelligence & Malware Investigation | VirusTotal, URLScan, MalwareBazaar, Hybrid Analysis, ANY.RUN, WHOIS, IP Reputation Analysis |
| Frameworks | MITRE ATT&CK Framework |

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

---

# Repository Structure

Each investigation follows a consistent structure while allowing additional repositories depending on investigation requirements.

| Directory | Purpose |
|----------|---------|
| README | Investigation overview, scenario, lab architecture, investigation workflow, key findings, timeline, MITRE ATT&CK mapping, screenshots |
| Evidence | Investigation artifacts including IOCs, timelines, technical findings, and supporting evidence |
| Incident Report | Complete investigation documentation including investigation phases, evidence analysis, root cause analysis, impact assessment, recommendations, and analyst conclusion |
| MITRE ATT&CK | Mapping of observed attacker behavior to ATT&CK techniques |
| Screenshots | Visual evidence supporting investigation findings |

Additional repositories may include **SPL Queries**, **PCAP**, **Sysmon**, **Windows Event Logs**, **Suricata**, and **Threat Intelligence**, depending on the investigation requirements.

---

# Projects

## SOC Project 1 — Phishing Email Investigation

**Focus:**  
Investigation of a suspected phishing email through email artifact analysis, sender authentication review, attachment analysis, threat intelligence enrichment, and IOC extraction.

**Skills Demonstrated:**

- Email header analysis
- SPF / DMARC validation
- Malicious attachment investigation
- Threat intelligence analysis
- IOC extraction
- Phishing incident classification

**Tools:**

Email Analysis • VirusTotal • Threat Intelligence • MITRE ATT&CK

**Repository:**  
[View Investigation →](https://github.com/Harmanjot960/SOC-Investigation-Portfolio/SOC-Project-1-Phishing-Investigation)
[View Investigation →](https://github.com/Harmanjot960/SOC-Investigation-Portfolio/tree/main/SOC-Project-1-Phishing-Investigation)

---

## SOC Project 2 — RDP Brute Force Detection

**Focus:**  
Detection and investigation of a simulated RDP brute-force attack using Windows authentication telemetry and Splunk-based analysis.

**Skills Demonstrated:**

- Security event log analysis
- Authentication investigation
- Failed and successful logon correlation
- Privileged access analysis
- SPL detection query development
- Attack timeline reconstruction

**Tools:**

Splunk • SPL Queries • Windows Security Logs • Event IDs 4624, 4625, 4672

**Repository:**  
[View Investigation →](PROJECT_LINK)

---

## SOC Project 3 — PowerShell Post-Compromise Investigation

**Focus:**  
Investigation of suspicious PowerShell activity following attacker access, using endpoint telemetry and Windows logging sources.

**Skills Demonstrated:**

- PowerShell Script Block analysis
- Process execution investigation
- Encoded command detection
- Command-line analysis
- Sysmon event correlation
- MITRE ATT&CK technique mapping

**Tools:**

Splunk • Sysmon • PowerShell Logging • Windows Event Logs

**Repository:**  
[View Investigation →](PROJECT_LINK)

---

## SOC Project 4 — Network Threat Hunting Investigation

**Focus:**  
Investigation of a malware infection through packet capture analysis, identifying malicious communication, payload delivery, and command-and-control activity.

**Skills Demonstrated:**

- PCAP analysis
- Network traffic investigation
- DNS / HTTP / TLS analysis
- IDS alert investigation
- Malware communication analysis
- IOC validation

**Tools:**

Wireshark • Suricata IDS • VirusTotal • PCAP Analysis

**Repository:**  
[View Investigation →](PROJECT_LINK)

---

## SOC Project 5 — Windows Endpoint Incident Response Investigation

**Focus:**  
Endpoint investigation of a compromised Windows system involving malware execution, persistence, credential access, and attacker activity reconstruction.

**Skills Demonstrated:**

- Endpoint telemetry analysis
- Process investigation
- Persistence detection
- Credential access investigation
- PowerShell analysis
- Network artifact correlation

**Tools:**

Sysmon • Windows Event Logs • PowerShell Logging • Wireshark

**Repository:**  
[View Investigation →](PROJECT_LINK)

---

## SOC Project 6 — Active Directory Ransomware Incident Investigation

**Focus:**  
Investigation of a multi-stage Active Directory ransomware intrusion involving initial access, credential compromise, lateral movement, privilege abuse, and ransomware deployment.

**Skills Demonstrated:**

- Active Directory investigation
- Authentication and privilege analysis
- Lateral movement detection
- SMB and PsExec analysis
- Ransomware attack chain reconstruction
- Incident response documentation

**Tools:**

Splunk • Sysmon • Active Directory Logs • Windows Event Logs • MITRE ATT&CK

**Repository:**  
[View Investigation →](PROJECT_LINK)

---

This portfolio demonstrates a structured, evidence-based approach to security monitoring, threat investigation, and SOC incident response across endpoint, network, and enterprise environments.
