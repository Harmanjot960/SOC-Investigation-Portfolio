# Investigation Artifacts

## Overview

This document contains the artifacts collected during the PowerShell post-compromise investigation.

The artifacts were collected from:

- Windows Security Logs
- PowerShell Operational Logs
- Sysmon Logs
- Splunk searches

---

# Log Sources

## PowerShell Script Block Logging

```
WinEventLog:Microsoft-Windows-PowerShell/Operational
```

Event analyzed:

```
Event ID 4104
```

Purpose:

Captured the actual PowerShell commands executed by the attacker.

---

## Sysmon Process Monitoring

```
XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```

Event analyzed:

```
Event ID 1
```

Purpose:

Confirmed PowerShell process execution and process relationships.

---

# Collected Artifacts

## PowerShell Commands

Examples:

```
Get-MpPreference

Get-ScheduledTask

Get-Process

Get-Service

Invoke-Expression

Set-ExecutionPolicy Bypass
```

---

## Process Artifacts

Observed process:

```
powershell.exe
```

Associated information:

```
Image:
powershell.exe

ParentImage:
explorer.exe
```

---

## Encoded Command Artifact

Observed:

```
powershell.exe -EncodedCommand
```

Related decoding functions:

```
FromBase64String()

Unicode.GetString()
```

Execution method:

```
Invoke-Expression
```

---

# Detection Artifacts

Splunk searches used:

EventID=4104   - for PowerShell Script Block Logging analysis.

EventID=1      - for Sysmon Process Creation analysis.

EventCode=4624 - for successful Windows authentication events.

---

# Evidence Files

Collected evidence includes:

```
Screenshots/
│
├── 01_successful_rdp_login_event_4624.png
├── 02_powershell_script_block_event_4104.png
├── 03_suspicious_powershell_commands.png
├── 04_sysmon_process_creation_event_1.png
├── 05_splunk_powershell_activity_detection.png
├── 06_splunk_encoded_command_detection.png
├── 07_attack_timeline.png
└── 08_incident_summary.png
```

---

# Final Assessment

The collected artifacts confirm post-compromise PowerShell activity involving reconnaissance, security discovery, execution policy bypass attempts, and encoded command execution.
