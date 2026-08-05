# Incident Report — Phishing Email Investigation

##  Executive Summary

A suspicious email was reported to the Security Operations Center (SOC) for investigation. The email contained indicators commonly associated with phishing activity, including a suspicious sender, social engineering elements, and an unsolicited attachment.

The attached archive was analyzed and found to contain an executable file. The executable was extracted, hashed, and analyzed using threat intelligence sources. VirusTotal analysis confirmed that the file was malicious based on multiple security vendor detections.

The investigation determined that the phishing attempt was detected before user interaction. No evidence of attachment execution or endpoint compromise was identified during the investigation.

---

## Incident Overview

| Field | Details |
|---|---|
| **Incident Title** | Phishing Email Investigation – Malicious Attachment Analysis |
| **Incident Type** | Phishing / Malicious Attachment |
| **Severity** | Medium |
| **Status** | Closed |
| **Analyst** | HK |
| **Investigation Date** | June 7, 2026 |
| **Alert Source** | Suspicious email reported for SOC analysis |
| **Detection Method** | User-reported suspicious email |
| **Final Classification** | Confirmed Phishing Attempt |

---

## Alert Details

| Field | Details |
|---|---|
| **Sender Address** | info@mutawamarine.com |
| **Recipient Address** | webmaster@redacted.org |
| **Reply-To Address** | info.mutawamarine@mail.com |
| **Subject** | webmaster@redacted.org your: Transfer Reference Number:(09674321) |
| **First Received Timestamp** | Wed, 10 Jun 2020 01:02:04 -0400 |
| **Final Delivery Timestamp** | Wed, 10 Jun 2020 05:58:54 +0000 |

---

## Initial Assessment

The initial triage process included reviewing the email content, sender information, and attachment characteristics to determine whether the message was malicious.

The following suspicious indicators were identified:

- Unexpected email communication
- Suspicious sender information
- Unsolicited attachment
- Executable file contained within the archive
- Malicious file reputation identified through threat intelligence analysis

Based on the initial findings, the email was escalated for further investigation.

---

## Investigation Workflow

The investigation followed a structured phishing analysis workflow:

1. Reviewed email metadata and headers.
2. Analyzed sender information and authentication results.
3. Extracted the attachment contents.
4. Identified the executable file within the archive.
5. Generated a SHA-256 hash for the executable.
6. Submitted the hash to VirusTotal for reputation analysis.
7. Documented indicators of compromise (IOCs).
8. Mapped observed activity to MITRE ATT&CK techniques.

---

## Email Analysis

The email headers and authentication results were analyzed to identify sender legitimacy and phishing indicators.

Findings:

- SPF authentication failure
- Reply-To mismatch
- Suspicious sender infrastructure
- Financial transaction-themed social engineering

---
## Attachment Analysis

The email contained a CAB archive attachment.

| Artifact | Details |
|---|---|
| Filename | SWT_#09674321____PDF__.CAB |
| Container Type | Microsoft Cabinet (CAB) archive |
| Extracted File | SWT_#09674321__PDF.com |
| Actual File Type | Windows executable |

---

## Threat Intelligence Findings

### File Analysis

The extracted file was submitted to VirusTotal for reputation analysis. VirusTotal identified the file as malicious based on multiple security vendor detections.

**Threat Intelligence Results**

| Artifact | Value |
|----------|-------|
| Detection Ratio | 58 / 71 (VirusTotal) |
| SHA256 | 05261f5a64f81a34fdde66cc82b573773e5dfa3bb5c3ccbfe2d0eef0e9d7b6c9 |

### Infrastructure Analysis

Infrastructure indicators associated with the phishing email were investigated using WHOIS lookup and VirusTotal.

The analysis focused on the sender domain and associated IP address to identify ownership information, reputation data, and potential malicious activity.

The identified infrastructure indicators were documented as part of the investigation and included in the IOC list.

---

## Indicators of Compromise (IOC)

| Type | Indicator | Description |
|---|---|---|
| Email Address | info@mutawamarine.com | Suspicious sender |
| Domain | mutawamarine.com | Associated domain |
| File Name | SWT_#09674321____PDF__.CAB | Malicious attachment |
| SHA-256 | 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f | File identifier |
| IP Address | 192.119.71.157 | Infrastructure indicator |

---

## MITRE ATT&CK Mapping

| Technique ID | Technique Name | Description |
|---|---|---|
| T1566.001 | Phishing: Spearphishing Attachment | The attacker used a malicious attachment delivered through email to attempt initial access. |

## Potential Techniques Not Confirmed

| Technique | Name | Reason Not Confirmed |
| --------- | ---- | ------------------- |
| T1204.002 | User Execution: Malicious File | No evidence of user interaction or execution of the malicious attachment was identified during the investigation. |
| T1036 | Masquerading | No sufficient evidence was available to confirm that the file was disguised as a legitimate document or application. |

---

## Impact Assessment

The investigation confirmed that the email was a phishing attempt containing a malicious attachment.

No evidence of attachment execution or endpoint compromise was identified during the investigation. Based on the available evidence, the phishing attempt was detected before user interaction.

---

## Root Cause Analysis

The incident was caused by a phishing email containing a malicious attachment designed to trick the recipient into opening a potentially harmful file.

The attack relied on social engineering techniques rather than exploitation of a confirmed technical vulnerability.

---

## Incident Classification

| Category | Classification |
|---|---|
| Incident Type | Phishing |
| Subcategory | Malicious Attachment |
| Severity | Medium |
| Outcome | Attempted compromise prevented |
| Status | Closed |

---

## Recommendations

- Continue user awareness training focused on phishing identification.
- Block malicious file hashes and associated indicators where applicable.
- Improve email filtering rules for suspicious attachments.
- Verify unexpected business requests through trusted communication channels.
- Maintain monitoring for related indicators of compromise.

---

## Lessons Learned

- Suspicious emails should be analyzed before interacting with attachments.
- File hash analysis provides valuable threat intelligence during malware investigations.
- Combining email analysis and threat intelligence improves phishing detection accuracy.
- Early reporting helps prevent potential endpoint compromise.

---

## Conclusion

The investigation confirmed a phishing attempt involving a malicious attachment.

The malicious file was identified through attachment analysis and threat intelligence validation. No evidence of execution, persistence, or endpoint compromise was identified.

**Final Outcome: Phishing attempt detected and contained before user impact.**
