# Threat Intelligence Analysis

## Overview

Threat intelligence was used to validate the reputation of the sender infrastructure and the email attachment.

The investigation focused on identifying known malicious indicators using publicly available intelligence sources.

---

# Infrastructure Analysis

## Originating IP Address

| Indicator | Value |
|-----------|-------|
| IP Address | 192.119.71.157 |
| Registered Owner | HostPapa |

### Findings

The originating IP address was identified from the earliest `Received` header and investigated using a WHOIS lookup to determine ownership.

---

# Email Authentication

## SPF

**Result:** Fail

```
spf=fail smtp.mailfrom=mutawamarine.com
```

SPF validation failed because the sending server was not authorized to send email on behalf of the sender's domain.

---

## DMARC

**Result:** Unknown

```
dmarc=unknown
```

A DMARC policy existed for the domain, but a valid authentication result could not be established for the email.

---

# Malware Analysis

## Attachment

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

VirusTotal identified the extracted executable as malicious, confirming that the attachment posed a significant security risk.

---

# Threat Intelligence Summary

The investigation identified multiple indicators consistent with a phishing campaign, including:

- Unauthorized sending infrastructure
- Failed email authentication
- Suspicious reply-to address
- Malicious executable embedded within the attachment
- High-confidence malware detections from VirusTotal

These findings support the classification of the email as a confirmed phishing attempt.
