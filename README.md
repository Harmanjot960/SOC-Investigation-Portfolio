# SOC-Investigation-Portfolio

## Overview

This repository contains documented cybersecurity investigations completed through hands-on labs, security training platforms, and publicly available security datasets.

Each investigation follows a structured SOC analyst workflow, including alert analysis, evidence collection, IOC extraction, timeline creation, threat intelligence analysis, SIEM investigation, MITRE ATT&CK mapping, and incident reporting.

The goal of this portfolio is to demonstrate practical experience with security monitoring, threat detection, incident investigation, and security operations workflows.

---

# Investigation Workflow

Each project follows a standard SOC investigation process:

```
Alert Detection
      |
      ▼
Initial Triage
      |
      ▼
Evidence Collection
      |
      ▼
IOC Extraction
      |
      ▼
Threat Intelligence Analysis
      |
      ▼
SIEM Investigation
      |
      ▼
MITRE ATT&CK Mapping
      |
      ▼
Incident Report
```
---

# Tools & Technologies

## SIEM / Log Analysis
* Splunk Enterprise
* SPL (Search Processing Language)
* Windows Event Logs
* Security Event Analysis

## Endpoint Investigation
* Sysmon
* Windows Security Logs
* PowerShell Logging
* Process and Command-Line Analysis

## Threat Intelligence & Malware Analysis
* VirusTotal
* URLScan
* WHOIS
* IP Reputation Analysis
* Hybrid Analysis
* ANY.RUN Sandbox
* MalwareBazaar (where applicable)

## Network Analysis
* Wireshark
* PCAP Analysis
* Network Traffic Investigation

## Frameworks
* MITRE ATT&CK
---

# Projects

# Project 1 - Phishing Investigation

**Status:** In Progress

**Environment:**

* TryHackMe
* SOC Simulation Labs
* Public security datasets (where applicable)

**Focus Areas:**

* Email source analysis
* Email header investigation
* Sender and domain reputation analysis
* Attachment and URL analysis
* Malware sandbox analysis
* Threat intelligence research
* IOC extraction
* SIEM investigation
* MITRE ATT&CK mapping
* Incident reporting
  
---

## Project 2 - PowerShell Attack Investigation

**Focus:**

* Suspicious PowerShell activity
* Process execution analysis
* Sysmon investigation
* Command-line analysis
* Script block logging analysis
* Detection query development

---

## Project 3 - Brute Force Detection

**Focus:**

* Authentication log analysis
* Failed login investigation
* Account activity monitoring
* Suspicious login detection
* SIEM detection queries

---

## Project 4 - Malware Investigation

**Focus:**

* Malware behavior analysis
* Static and dynamic analysis
* Sandbox investigation
* Hybrid Analysis and ANY.RUN reports
* IOC extraction
* Endpoint investigation
* Threat intelligence correlation
* MITRE ATT&CK mapping

---

## Project 5 - Windows Endpoint Investigation

**Focus:**

* Windows Event Log analysis
* Persistence technique detection
* Suspicious process investigation
* Registry and scheduled task analysis
* Incident response workflow

---

# Documentation Structure

Each project contains:

```
Project/
│
├── README.md
├── Screenshots/
├── Evidence/
├── SPL-Queries/
├── MITRE-ATT&CK/
└── Incident-Report/
```

---

# About This Portfolio

These investigations are performed in controlled lab environments and training platforms, along with analysis of publicly available security datasets and log artifacts.

Each project documents:

* Investigation environment
* Alert details
* Investigation methodology
* Evidence collected
* IOC analysis
* Detection queries
* MITRE ATT&CK mapping
* Findings
* Final incident report

This portfolio demonstrates practical experience with SOC monitoring, threat detection, SIEM investigation, endpoint analysis, and incident response processes.
