# Threat Intelligence Analysis

## Overview

Threat intelligence was used to validate the reputation of the sender infrastructure and the email attachment.

The investigation focused on identifying known malicious indicators using publicly available intelligence sources.

---

## Infrastructure Analysis

### Originating IP Address

| Indicator | Value |
|-----------|-------|
| IP Address | 192.119.71.157 |
| Registered Owner | HostPapa |

### Findings

The originating IP address was identified from the earliest `Received` header and investigated using a WHOIS lookup to determine ownership.

---

## Email Authentication

### SPF

**Result:** Fail
**Source:** Email header analysis and MXToolbox validation

```
spf=fail smtp.mailfrom=mutawamarine.com
```

SPF validation failed because the sending server was not authorized to send email on behalf of the sender's domain.

---

### DMARC

**Result:** Unknown
**Source:** Email header analysis 
```
dmarc=unknown
```

The email did not contain a confirmed DMARC authentication result. A DMARC policy record existed for the sender domain, but the message could not be validated as passing or failing DMARC.

---

## Malware Analysis

### Attachment

| Artifact | Value |
|----------|-------|
| File Name | SWT_#09674321____PDF__.CAB |
| SHA256 | 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f |

The attachment archive contained a Windows executable file.

---

## VirusTotal Analysis

| Artifact | Value |
|----------|-------|
| Extracted File | SWT_#09674321__PDF.com |
| File Type | Win32 Executable |
| Detection Ratio | 58 / 71 |

VirusTotal identified the extracted executable as malicious based on multiple security vendor detections, confirming that the attachment was unsafe.

---

## Threat Intelligence Summary

The investigation identified multiple indicators consistent with a phishing campaign, including:

- SPF authentication failure for the sending infrastructure
- Suspicious reply-to address
- Malicious executable embedded within the attachment
- High-confidence malware detections from VirusTotal

These findings support the classification of the email as a confirmed phishing attempt.
