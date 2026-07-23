# Suricata Alert Analysis

## Alert 1

**Signature:**
- ET MALWARE Fake Microsoft Teams VBS Payload Inbound

**Meaning:**

Initial malware payload delivery detected.

---

## Alert 2

**Signature:**
- ET MALWARE Fake Microsoft Teams CnC Payload Request (GET)

**Meaning:**

Host communicated with malware infrastructure.

---

## Alert 3

**Signature:**
- ET INFO PS1 Powershell File Request

**Meaning:**

PowerShell payload retrieval detected.

---

## Alert 4

**Signatures:**
- ET HUNTING Generic Powershell DownloadString Command
- ET HUNTING Generic Powershell DownloadFile Command

**Meaning:**

PowerShell downloader behavior detected.

---

## Alert 5

**Signature:**
- ET INFO PE EXE or DLL Windows File Download

**Meaning:**

Executable payload transfer detected.

---

## Analyst Conclusion

Suricata alerts confirm malicious network behavior involving:

- Malware delivery
- PowerShell execution
- Payload retrieval
- Command-and-control (C2) communication
