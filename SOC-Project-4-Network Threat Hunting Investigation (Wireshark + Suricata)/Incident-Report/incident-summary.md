# Incident Summary

| Field | Details |
|------|---------|
| Incident Type | Malware Infection |
| Affected Host | 10.1.17.215 |
| Domain | BLUEMOONTUESDAY |
| Tools | Wireshark + Suricata |
| Severity | High |
| Status | CONFIRMED |

---

## Key Findings

| Category | Finding |
|---------|---------|
| Initial Access | Fake Google Authenticator Application |
| Malware Delivery | PowerShell Payload |
| C2 Communication | 5.252.153.241 |
| Payloads | TeamViewer Components |
| Persistence | Startup Shortcut |

---

## Network Evidence

- Malicious Domain: authenticatoor.org
- Payload Server: 5.252.153.241
- Suspicious TLS: 45.125.66.32
- Payload Hosting: 82.221.136.26

---

## Final Assessment

Confirmed malware infection involving:

- Malicious application download
- PowerShell payload execution
- C2 communication
- Additional malware retrieval
- Persistence attempt

Evidence collected using Wireshark packet analysis and Suricata IDS detection.
