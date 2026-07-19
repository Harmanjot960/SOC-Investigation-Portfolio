### Incident Summary
| Incident Type | Host | User | SIEM | Severity | Status |
|---|---|---|---|---|---|
| PowerShell Post-Compromise Activity | DESKTOP-SHNKQPV | Analyst | Splunk | Medium | CONFIRMED |
---
#### Key Findings

| RDP Login | PowerShell | Sysmon | Discovery | Persistence Check | Encoding |
|---|---|---|---|---|---|
| Event ID 4624 Successful Authentication | Event ID 4104 Script Block Logging | Event ID 1 Process Creation | System & Network Discovery | Scheduled Task Discovery | Encoded PowerShell Detected |
---
#### MITRE ATT&CK Mapping
| T1059.001 | T1082 | T1016 | T1053 | T1027 |
|---|---|---|---|---|
| PowerShell | System Information Discovery | Network Configuration Discovery | Scheduled Task | Obfuscated Files |
