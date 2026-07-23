# MITRE ATT&CK Mapping

| Technique | ID | Evidence |
| ---------- | -- | -------- |
| User Execution | T1204 | User executed a malicious Google Authenticator application |
| PowerShell | T1059.001 | PowerShell payload delivery and execution |
| Ingress Tool Transfer | T1105 | Malware and scripts downloaded from a remote server |
| Obfuscated Files or Information | T1027 | Base64 encoding and Invoke-Expression (`IEX`) usage |
| Application Layer Protocol: Web Protocols | T1071.001 | HTTP communication with malicious infrastructure |
| Boot or Logon Autostart Execution | T1547.001 | Startup shortcut created for persistence |
| Masquerading | T1036 | Fake Google Authenticator and TeamViewer naming |

---

# Attack Chain

```text
Malicious Application Download
        │
        ▼
User Execution
        │
        ▼
PowerShell Loader
        │
        ▼
Payload Download
        │
        ▼
Command-and-Control (C2) Communication
        │
        ▼
Persistence
```
