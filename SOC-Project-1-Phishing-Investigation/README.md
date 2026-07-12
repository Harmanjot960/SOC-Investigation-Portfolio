# SOC Project 1: Phishing Email Investigation 

## Overview

This project documents the investigation of a suspicious phishing email escalated to the Security Operations Center (SOC).

The objective was to analyze the email artifact, identify malicious indicators, validate sender authenticity, investigate the attached file, extract indicators of compromise (IOCs), and determine the final incident classification.

---

# Case Information

| Field | Details |
|------|---------|
| Source | TryHackMe SOC Level 1 - Greenholt Phish |
| Investigation Type | Phishing Email Analysis |
| Evidence Source | Provided suspicious email sample (.eml) |
| Email Client | Mozilla Thunderbird |
| Analysis Environment | TryHackMe Virtual Machine |

---

# Case Scenario

A sales executive at Greenholt PLC reported receiving a suspicious email from a known customer.

The email contained several red flags:

- Generic greeting
- Unexpected money transfer request
- Unsolicited attachment
- Communication style inconsistent with previous interactions

The email was escalated to the SOC for investigation to determine whether it was legitimate or part of a phishing campaign.

---

# Investigation Objectives

- Analyze email headers and message source
- Investigate sender and reply-to information
- Validate SPF and DMARC authentication results
- Identify suspicious infrastructure
- Analyze the email attachment
- Perform threat intelligence checks
- Extract indicators of compromise
- Document findings through an incident report

---

# Investigation Workflow

```
Suspicious Email Reported
          |
          ▼
Email Source & Header Analysis
          |
          ▼
Sender Authentication Review
(SPF / DMARC)
          |
          ▼
Infrastructure Investigation
          |
          ▼
Attachment & Hash Analysis
          |
          ▼
Threat Intelligence Review
          |
          ▼
IOC Extraction
          |
          ▼
Incident Classification
```

---

# Tools Used

| Tool | Purpose |
|------|---------|
| Mozilla Thunderbird | Reviewed email artifact |
| Email Message Source Viewer | Manual header analysis |
| MXToolbox | Email header, SPF, and DMARC analysis |
| WHOIS Lookup | Investigated IP ownership |
| Linux sha256sum | Hash generation |
| VirusTotal | File reputation and malware analysis |


---

# Key Findings

The investigation identified multiple indicators of a phishing attempt:

- SPF authentication failure for the sender domain
- Suspicious Reply-To address mismatch
- Financial transaction-themed social engineering
- Malicious attachment disguised as a document
- Attachment contained a Windows executable
- VirusTotal identified malicious activity

---

# Attachment Analysis

The email contained the following attachment:

| Artifact | Details |
|-|-|
| Filename | SWT_#09674321____PDF__.CAB |
| File Type | CAB Archive |
| Extracted File | SWT_#09674321__PDF.com |
| Actual File Type | Win32 EXE |

The attachment was analyzed using VirusTotal after calculating its SHA256 hash.

Detailed file analysis and threat intelligence results are available in:

- `Evidence/artifacts.md`
- `Evidence/iocs.md`
- `Threat-Intelligence/virustotal_analysis.md`

---

# MITRE ATT&CK Mapping

## T1566.001 - Phishing: Spearphishing Attachment

**Evidence:**

A malicious attachment was delivered through email to the targeted user.

---

## T1204.002 - User Execution: Malicious File

**Evidence:**

The attack required the recipient to open the attachment and execute the embedded malicious file.

---

# Incident Classification

## Verdict: True Positive - Phishing Attempt

The email was classified as malicious based on:

- Email authentication failures
- Sender impersonation indicators
- Suspicious business request
- Malicious attachment behavior
- Threat intelligence detections

---

# Impact Assessment

No evidence of endpoint compromise was available from the provided artifacts.

The investigation confirmed that the email contained a malicious attachment; however, execution on a user system was not observed within the available evidence.

---

# Recommended Actions

- Quarantine the malicious email
- Block identified sender infrastructure
- Block malicious file hashes
- Monitor for similar phishing attempts
- Improve phishing awareness training
- Review email authentication policies

---

# Supporting Documentation

Detailed investigation materials:

```
SOC-Project-1-Phishing-Investigation/

├── Screenshots/
│
├── Evidence/
│   ├── artifacts.md
│   ├── iocs.md
│   └── timeline.md
│
├── Threat-Intelligence/
│   ├── virustotal_analysis.md
│   └── infrastructure_analysis.md
│
├── MITRE-ATT&CK/
│   └── mitre_mapping.md
│
└── Incident-Report/
    └── Incident_Report.pdf
```

---

# Final Summary

This project demonstrates a SOC analyst workflow for investigating a phishing email, including email header analysis, authentication validation, malicious attachment investigation, IOC extraction, threat intelligence analysis, and incident reporting.
