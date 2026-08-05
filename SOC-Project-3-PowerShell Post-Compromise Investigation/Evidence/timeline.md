# PowerShell Post-Compromise Investigation Timeline

## Incident Timeline

This timeline documents attacker activity observed after successful RDP authentication.

The investigation combines Windows Security Logs, PowerShell Script Block Logging (Event ID 4104), and Sysmon Process Creation events (Event ID 1).


| Time | Event ID | Source | Activity |
|---|---|---|---|
| 2026-07-14 08:23:11 | 4624 | Windows Security | Successful RDP authentication |
| 2026-07-15 23:34:20 | 1 | Sysmon | powershell.exe process creation detected |
| 2026-07-15 23:34:21 | 4104 | PowerShell | PowerShell Script Block Logging started |
|2026-07-15 23:38:49 | 4104 | PowerShell | Execution policy bypass attempted|
| 2026-07-15 23:42:00 - 23:42:15 | 4104 | PowerShell | Security and scheduled task discovery |
| 2026-07-15 23:43:00 | 4104 | PowerShell | Execution policy bypass attempted |
| 2026-07-15 23:43:20 | 4104 | PowerShell | Encoded payload decoding and execution |
| 2026-07-15 23:47:49 - 23:48:48 | 4104 | PowerShell | Encoded PowerShell command execution detected |

---

## Timeline Analysis

The investigation began with a successful RDP authentication event (Event ID 4624), which established the initial access context.

Sysmon Event ID 1 then identified PowerShell process creation, followed by PowerShell Script Block Logging (Event ID 4104), which captured reconnaissance commands, security configuration checks, persistence discovery, execution policy bypass attempts, and encoded PowerShell execution.

The sequence of events indicates post-compromise PowerShell activity following successful authentication.

---

## Attack Flow
```
Successful RDP Authentication
          |
          ▼
PowerShell Process Creation
          |
          ▼
Host, User, and Network Discovery
          |
          ▼
Security & Persistence Discovery
          |
          ▼
Execution Policy Bypass
          |
          ▼
Encoded PowerShell Execution
```
