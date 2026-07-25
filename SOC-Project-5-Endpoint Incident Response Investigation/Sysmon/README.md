# Sysmon Evidence Analysis

This directory contains Sysmon telemetry collected during the Windows endpoint incident response investigation.

Sysmon was used to monitor:

- Process execution
- Network connections
- File creation activity
- DNS queries

The collected telemetry was correlated with Windows Event Logs and Wireshark traffic analysis to reconstruct the attacker lifecycle.

---

# Sysmon Events Investigated

| Event ID | Description | Investigation Purpose |
|----------|-------------|----------------------|
| Event ID 1 | Process Creation | Identify attacker execution chain and malicious processes |
| Event ID 3 | Network Connection | Identify external communication and C2 activity |
| Event ID 11 | File Creation | Identify malware delivery and persistence artifacts |
| Event ID 22 | DNS Query | Identify attacker-controlled domain resolution |

---

# Investigation Flow

```text
Malicious Document
        |
        ▼
Process Creation
(Sysmon Event ID 1)
        |
        ▼
Network Communication
(Sysmon Event ID 3)
        |
        ▼
File Creation
(Sysmon Event ID 11)
        |
        ▼
DNS Resolution
(Sysmon Event ID 22)
        |
        ▼
Attack Timeline Reconstruction
```

---

# Evidence Sources

Sysmon telemetry was correlated with:

- Windows Security Event Logs
- Wireshark network analysis
- VirusTotal malware analysis
- MITRE ATT&CK techniques

---

# Related Evidence

- `process-events.md`
- `network-events.md`
- `file-events.md`
- `dns-events.md`
