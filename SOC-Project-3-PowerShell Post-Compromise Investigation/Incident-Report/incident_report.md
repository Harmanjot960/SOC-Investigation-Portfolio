# Incident Report — PowerShell Post-Compromise Activity

## Executive Summary

A Windows endpoint was investigated for suspicious PowerShell activity following a successful RDP authentication event.

The investigation identified PowerShell-based reconnaissance, system and user discovery, security configuration checks, persistence discovery, execution policy bypass attempts, and encoded PowerShell execution.

PowerShell Script Block Logging (Event ID 4104), Sysmon Process Creation (Event ID 1), and Splunk correlation searches were used to identify and analyze attacker activity.

---

# Incident Details

| Category | Details |
|---|---|
| Incident Type | PowerShell Post-Compromise Activity |
| Affected Host | DESKTOP-SHNKQPV |
| User Context | Analyst |
| SIEM | Splunk |
| Severity | Medium |

---

# Initial Access Context

The investigation began after a successful RDP authentication event.

Evidence:

```
Event ID:
4624

Activity:
Successful RDP Authentication
```

This event provided the starting point for analyzing post-compromise activity.

---

# Investigation Timeline

| Time | Event | Description |
|---|---|---|
| 2026-07-14 08:23:11 | Event ID 4624 | Successful RDP authentication |
| 2026-07-15 23:34:20 | Sysmon Event ID 1 | PowerShell process creation detected |
| 2026-07-15 23:34:21 - 23:48:48 | PowerShell Event ID 4104 | PowerShell Script Block Logging captured commands |
| 2026-07-15 23:47:49 - 23:48:48 | Event ID 4104 | Encoded PowerShell execution detected |

---

# Phase 1 — PowerShell Execution

Sysmon Event ID 1 identified PowerShell execution.

Observed:

```
Image:
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

ParentImage:
C:\Windows\explorer.exe
```

This confirmed PowerShell execution on the endpoint.

---

# Phase 2 — Host Discovery

The attacker performed system enumeration.

Observed commands:

```
$PSVersionTable

Get-Date

Get-Location

hostname

whoami

Get-ComputerInfo
```

Purpose:

- Identify operating system details
- Determine user context
- Collect endpoint information

MITRE ATT&CK:

```
T1082 - System Information Discovery
```

---

# Phase 3 — User and Network Discovery

The attacker enumerated users and network information.

User discovery:

```
Get-LocalUser

Get-LocalGroup

$env:USERNAME

whoami
```

Network discovery:

```
Get-NetIPAddress

Get-NetAdapter

Get-NetRoute

ipconfig

Test-NetConnection
```

Purpose:

Understand available users, privileges, and network configuration.

MITRE ATT&CK:

```
T1087 - Account Discovery

T1016 - System Network Configuration Discovery
```

---

# Phase 4 — Process and Service Discovery

Observed commands:

```
Get-Process

Get-Service
```

Purpose:

Identify running applications and Windows services.

MITRE ATT&CK:

```
T1057 - Process Discovery

T1007 - System Service Discovery
```

---

# Phase 5 — Security Reconnaissance

The attacker queried Windows Defender configuration.

Command:

```
Get-MpPreference
```

Purpose:

Identify security configuration and defensive controls.

MITRE ATT&CK:

```
T1518.001 - Security Software Discovery
```

---

# Phase 6 — Persistence Discovery

The attacker reviewed persistence locations.

Observed:

```
Get-ScheduledTask

HKLM:\Software\Microsoft\Windows\CurrentVersion\Run
```

Purpose:

Identify possible persistence mechanisms.

MITRE ATT&CK:

```
T1053 - Scheduled Task/Job Discovery
```

---

# Phase 7 — Defense Evasion

The attacker attempted to bypass PowerShell execution restrictions.

Command:

```
Set-ExecutionPolicy Bypass -Scope Process
```

This behavior is commonly observed during malicious PowerShell activity.

---

# Phase 8 — Encoded PowerShell Execution

Splunk detected encoded PowerShell execution activity.

Observed:

```
powershell.exe -EncodedCommand dwBoAG8AYQBtAGkA
```

Additional decoding behavior:

```
[Convert]::FromBase64String($encoded)

[System.Text.Encoding]::Unicode.GetString(...)

Invoke-Expression
```

Encoded PowerShell commands are commonly used to hide command execution and evade detection.

MITRE ATT&CK:

```
T1027 - Obfuscated Files or Information
```

---

# Detection and Analysis

## PowerShell Script Block Logging

PowerShell Event ID 4104 provided visibility into executed commands.

Detected activity included:

```
Get-MpPreference

Get-ScheduledTask

Invoke-Expression

Set-ExecutionPolicy Bypass

-EncodedCommand
```

---

## Sysmon Process Creation

Sysmon Event ID 1 confirmed:

- PowerShell process execution
- Parent process relationship
- Process activity timeline

---

## Splunk Detection

Splunk searches were used to identify:

### Suspicious PowerShell Commands

Detection keywords:

```
Get-MpPreference

Get-ScheduledTask

ExecutionPolicy

Invoke-Expression
```

### Encoded PowerShell Activity

Detection keywords:

```
EncodedCommand

FromBase64String

Invoke-Expression
```

---

# Impact Assessment

Observed activity allowed the attacker to:

- Gather endpoint information
- Identify users and privileges
- Enumerate processes and services
- Review security configuration
- Discover persistence locations
- Attempt PowerShell restriction bypass
- Execute encoded PowerShell commands

No destructive actions were observed.

---

# Response Recommendations

Recommended SOC actions:

- Review PowerShell Script Block Logs
- Investigate affected user activity
- Review persistence locations
- Monitor for additional encoded commands
- Maintain Sysmon endpoint monitoring
- Restrict unnecessary PowerShell usage
- Investigate possible credential compromise from RDP access

---

# MITRE ATT&CK Mapping

| Activity | Technique | ID |
|---|---|---|
| PowerShell Execution | Command and Scripting Interpreter: PowerShell | T1059.001 |
| System Information Discovery | System Information Discovery | T1082 |
| Network Discovery | System Network Configuration Discovery | T1016 |
| User Discovery | Account Discovery | T1087 |
| Process Discovery | Process Discovery | T1057 |
| Service Discovery | System Service Discovery | T1007 |
| Security Software Discovery | Security Software Discovery | T1518.001 |
| Scheduled Task Discovery | Scheduled Task/Job Discovery | T1053 |
| PowerShell Obfuscation | Obfuscated Files or Information | T1027 |

---

# Final Assessment

The investigation confirmed suspicious PowerShell post-compromise activity on the Windows endpoint.

The attacker performed reconnaissance, security discovery, persistence discovery, attempted execution policy bypass, and encoded PowerShell execution.

Splunk correlation of Windows PowerShell Event ID 4104 and Sysmon Event ID 1 provided visibility into attacker behavior and enabled incident documentation.
