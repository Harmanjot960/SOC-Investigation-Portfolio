# Collected Artifacts

## Case Information

| Field | Details |
|------|---------|
| Investigation Type | Phishing Email Analysis |
| Evidence Source | Suspicious email sample (.eml) |
| Analysis Method | Manual email header and attachment analysis |

---

# Email Artifacts

| Artifact | Value |
|----------|-------|
| Display Name | Mr. James Jackson |
| Sender Address | info@mutawamarine.com |
| Reply-To Address | info.mutawamarine@mail.com |
| Recipient | webmaster@redacted.org |
| Subject | webmaster@redacted.org your: Transfer Reference Number:(09674321) |
| Message ID | 20200609225823.DFAEAAF31A6B7414@mutawamarine.com |

---

# Email Authentication

| Check | Result |
|-------|--------|
| SPF | Fail |
| DMARC | Unknown |

### SPF Record

```
v=spf1 include:spf.protection.outlook.com -all
```

### DMARC Record

```
v=DMARC1; p=quarantine; fo=1
```

---

# Email Infrastructure

| Artifact | Value |
|----------|-------|
| Originating IP | 192.119.71.157 |
| IP Owner | HostPapa |

---

# Attachment Artifacts

| Artifact | Value |
|----------|-------|
| Attachment Name | SWT_#09674321____PDF__.CAB |
| Archive Type | CAB Archive |
| SHA256 | 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f |

### Extracted File

| Artifact | Value |
|----------|-------|
| File Name | SWT_#09674321__PDF.com |
| Actual File Type | Win32 Executable |
| Detection Ratio | 58 / 71 (VirusTotal) |
| SHA256 | 05261f5a64f81a34fdde66cc82b573773e5dfa3bb5c3ccbfe2d0eef0e9d7b6c9 |

---

# Investigation Tools

| Tool | Purpose |
|------|---------|
| Mozilla Thunderbird | Opened and reviewed the email artifact |
| Email Message Source | Examined raw email headers |
| MXToolbox | Analyzed email headers, SPF, and DMARC |
| WHOIS Lookup | Identified the owner of the originating IP |
| Linux sha256sum | Generated the attachment SHA256 hash |
| VirusTotal | Investigated file reputation and malware detections |

