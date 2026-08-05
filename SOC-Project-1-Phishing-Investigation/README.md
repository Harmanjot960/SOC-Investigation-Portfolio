# SOC Project 1: Phishing Email Investigation 

## Overview

This project documents the investigation of a suspicious phishing email escalated to the Security Operations Center (SOC).

The objective was to analyze the email artifact, identify malicious indicators, validate sender authenticity, investigate the attached file, extract indicators of compromise (IOCs), and determine the final incident classification.

---

## Case Scenario

A sales executive at Greenholt PLC reported receiving a suspicious email from a known customer.

The email contained several red flags:

- Generic greeting
- Unexpected money transfer request
- Unsolicited attachment
- Communication style inconsistent with previous interactions

The email was escalated to the SOC for investigation to determine whether it was legitimate or part of a phishing campaign.

---

## Investigation Objectives

- Analyze email headers and message source
- Investigate sender and reply-to information
- Validate SPF and DMARC authentication results
- Identify suspicious infrastructure
- Analyze the email attachment
- Perform threat intelligence checks
- Extract indicators of compromise
- Document findings through an incident report

---

## Investigation Workflow

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

## Case Information

| Field | Details |
|---|---|
| Investigation Type | Phishing Email Analysis |
| Evidence Source | Suspicious email sample (.eml) |
| Email Client | Mozilla Thunderbird |
| Analysis Environment | TryHackMe Virtual Machine |
| Source | TryHackMe SOC Level 1 - Greenholt Phish |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Mozilla Thunderbird | Reviewed email artifact |
| Email Message Source Viewer | Manual header analysis |
| MXToolbox | Email header, SPF, and DMARC analysis |
| WHOIS Lookup | Investigated IP ownership |
| Linux sha256sum | Hash generation |
| VirusTotal | File reputation and malware analysis |


---

## Project Structure

Detailed investigation materials:

```
SOC-Project-1-Phishing-Investigation/
│
├── Screenshots/
│   ├── 01_email_opened_thunderbird.png
│   ├── 02_message_source_headers.png
│   ├── 03_mxtoolbox_header_analysis.png
│   ├── 04_spf_dmarc_results.png
│   ├── 05_whois_lookup.png
│   ├── 06_attachment_details.png
│   ├── 07_sha256_generation.png
│   └── 08_virustotal_analysis.png
│
├── Evidence/
│   ├── artifacts.md
│   ├── iocs.md
│   └── timeline.md
│
├── Threat-Intelligence/
│   └── threat_intelligence.md
│
├── MITRE-ATT&CK/
│   └── mitre_mapping.md
│
└── Incident-Report/
    └── Incident_Report.pdf
```

---

## Key Findings

The investigation identified multiple indicators of a phishing attempt:

- SPF authentication failure for the sender domain
- Suspicious Reply-To address mismatch
- Financial transaction-themed social engineering
- Malicious attachment disguised as a document
- Attachment contained a Windows executable
- VirusTotal identified malicious activity

---

## Attachment Analysis

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
  [- Email artifacts and attachment analysis](Evidence/artifacts.md)
- `Evidence/iocs.md`
[- Indicators of Compromise (IOCs)](Evidence/iocs.md)
- `Threat-Intelligence/virustotal_analysis.md`
[- Threat intelligence results](Threat-Intelligence/threat_intelligence.md)

---

## Screenshots

### 1. Email Opened in Thunderbird
- [01_email_opened_thunderbird.png](Screenshots/01_email_opened_thunderbird.png)

### 2. Email Source Headers
- [02_message_source_headers.png](Screenshots/02_message_source_headers.png)

### 3. MXToolbox Header Analysis
- [03_mxtoolbox_header_analysis.png](Screenshots/03_mxtoolbox_header_analysis.png)

### 4. SPF and DMARC Results
- [04_spf_dmarc_results.png](Screenshots/04_spf_dmarc_results.png)

### 5. WHOIS Lookup
- [05_whois_lookup.png](Screenshots/05_whois_lookup.png)

### 6. Attachment Details
- [06_attachment_details.png](Screenshots/06_attachment_details.png)

### 7. SHA256 Generation
- [07_sha256_generation.png](Screenshots/07_sha256_generation.png)

### 8. VirusTotal Analysis
- [08_virustotal_analysis.png](Screenshots/08_virustotal_analysis.png)

---

## MITRE ATT&CK Mapping

### T1566.001 - Phishing: Spearphishing Attachment

**Evidence:**

A malicious attachment was delivered through email to the targeted user.

---

### T1204.002 - User Execution: Malicious File

**Evidence:**

The attack required the recipient to open the attachment and execute the embedded malicious file.

---

## Incident Classification

**Verdict: True Positive - Phishing Attempt**

The email was classified as malicious based on:

- Email authentication failures
- Sender impersonation indicators
- Suspicious business request
- Malicious attachment behavior
- Threat intelligence detections

---

## Impact Assessment

No evidence of endpoint compromise was available from the provided artifacts.

The investigation confirmed that the email contained a malicious attachment; however, execution on a user system was not observed within the available evidence.

---

## Incident Response Recommendations

- Quarantine the malicious email
- Block identified sender infrastructure
- Block malicious file hashes
- Monitor for similar phishing attempts
- Improve phishing awareness training
- Review email authentication policies

---

## Conclusion

This project demonstrates a SOC analyst workflow for investigating a phishing email, including email header analysis, authentication validation, malicious attachment investigation, IOC extraction, threat intelligence analysis, and incident reporting.
