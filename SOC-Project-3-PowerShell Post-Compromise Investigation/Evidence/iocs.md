# Indicators of Compromise (IOCs)

## Incident

PowerShell Post-Compromise Investigation

---

# Host Information

| Type | Value |
|---|---|
| Hostname | DESKTOP-SHNKQPV |
| Operating System | Windows |
| Investigation Platform | Splunk |

---

# User Information

| Type | Value |
|---|---|
| User Account | Analyst |
| Context | Interactive PowerShell activity |

---

# Observed Processes

## PowerShell Execution

| Field | Value |
|---|---|
| Process | powershell.exe |
| Parent Process | explorer.exe |
| Detection Source | Sysmon Event ID 1 |

---

# Suspicious Commands Observed

## Discovery Commands

```
whoami

hostname

Get-ComputerInfo

Get-Location

Get-Date
```

---

## User Discovery

```
Get-LocalUser

Get-LocalGroup

$env:USERNAME
```

---

## Network Discovery

```
Get-NetIPAddress

Get-NetAdapter

Get-NetRoute

ipconfig

Test-NetConnection
```

---

## Security Reconnaissance

```
Get-MpPreference
```

---

## Persistence Discovery

```
Get-ScheduledTask

HKLM:\Software\Microsoft\Windows\CurrentVersion\Run
```

---

## Defense Evasion

```
Set-ExecutionPolicy Bypass -Scope Process
```

---

## Obfuscated PowerShell

Observed indicators:

```
-EncodedCommand

FromBase64String

Unicode.GetString

Invoke-Expression
```

---

# MITRE ATT&CK Relevant Indicators

# MITRE ATT&CK Relevant Indicators

| Activity | Technique | MITRE ATT&CK ID |
|---|---|---|
| PowerShell Execution | Command and Scripting Interpreter: PowerShell | T1059.001 |
| System Discovery | System Information Discovery | T1082 |
| Network Discovery | System Network Configuration Discovery | T1016 |
| PowerShell Obfuscation | Obfuscated Files or Information | T1027 |
| Persistence Discovery | Scheduled Task Discovery | T1053 |
