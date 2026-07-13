# Indicators of Compromise (IOCs)

The following indicators were identified during the phishing email investigation.

| Type | Indicator | Description |
|-|-|-|
| Email Address | info@mutawamarine.com | Sender address |
| Email Address | info.mutawamarine@mail.com | Reply-To address |
| Domain | mutawamarine.com | Sender domain |
| IP Address | 192.119.71.157 | Originating mail server IP |
| File Name | SWT_#09674321____PDF__.CAB | Suspicious attachment |
| File Name | SWT_#09674321__PDF.com | Extracted executable |
| SHA256 | 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f | Attachment hash |
| SHA256 | 05261f5a64f81a34fdde66cc82b573773e5dfa3bb5c3ccbfe2d0eef0e9d7b6c9 | Extracted executable hash |

---

# IOC Analysis Summary

## Email Indicators

The sender information contained suspicious characteristics:

- Sender authentication failure
- Reply-To address mismatch
- Financial transaction-themed subject

---

## File Indicators

The attachment was extracted to examine its contents. The archive contained an executable (.exe) file. A SHA-256 hash of the executable was generated and submitted to VirusTotal, where it was identified as malicious by multiple security vendors.

- Archive extracted successfully
- Executable file identified
- SHA-256 hash generated
- VirusTotal detection results confirming the file as malicious

