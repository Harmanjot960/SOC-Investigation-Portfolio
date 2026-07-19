# MITRE ATT&CK Mapping

## PowerShell Post-Compromise Investigation

This document maps observed attacker activity to MITRE ATT&CK techniques based on PowerShell Script Block Logging (Event ID 4104) and Sysmon Process Creation events (Event ID 1).

---

| Activity | Technique | ID | Description |
|---|---|---|---|
| PowerShell Execution | Command and Scripting Interpreter: PowerShell | T1059.001 | Attacker used PowerShell for command execution |
| System Information Discovery | System Information Discovery | T1082 | Attacker collected host information |
| Network Discovery | System Network Configuration Discovery | T1016 | Attacker inspected network configuration |
| User Discovery | Account Discovery | T1087 | Attacker enumerated local users and groups |
| Process Discovery | Process Discovery | T1057 | Attacker reviewed running processes |
| Service Discovery | System Service Discovery | T1007 | Attacker enumerated Windows services |
| Security Software Discovery | Security Software Discovery | T1518.001 | Attacker checked Windows Defender configuration |
| Scheduled Task Discovery | Scheduled Task/Job Discovery | T1053 | Attacker reviewed scheduled tasks |
| PowerShell Obfuscation | Obfuscated Files or Information | T1027 | Attacker used encoded PowerShell commands |

---

# Observed Techniques

## T1059.001 — Command and Scripting Interpreter: PowerShell

Evidence:

```
powershell.exe
```

PowerShell Script Block Logging captured executed commands through Event ID 4104.

---

## T1082 — System Information Discovery

Observed commands:

```
Get-ComputerInfo

hostname

$PSVersionTable
```

Purpose:

Collect information about the compromised endpoint.

---

## T1016 — System Network Configuration Discovery

Observed commands:

```
Get-NetIPAddress

Get-NetAdapter

ipconfig
```

Purpose:

Identify network configuration and adapter information.

---

## T1087 — Account Discovery

Observed commands:

```
whoami

Get-LocalUser

Get-LocalGroup
```

Purpose:

Identify user accounts and privilege context.

---

## T1057 — Process Discovery

Observed command:

```
Get-Process
```

Purpose:

Enumerate running processes on the endpoint.

---

## T1007 — System Service Discovery

Observed command:

```
Get-Service
```

Purpose:

Identify running services on the system.

---

## T1518.001 — Security Software Discovery

Observed command:

```
Get-MpPreference
```

Purpose:

Query Windows Defender configuration and security settings.

---

## T1053 — Scheduled Task Discovery

Observed commands:

```
Get-ScheduledTask

Get-ScheduledTask | Where-Object {$_.State -eq 'Ready'}
```

Purpose:

Review scheduled tasks and identify possible persistence locations.

---

## T1027 — Obfuscated Files or Information

Observed behavior:

```
powershell.exe -EncodedCommand

FromBase64String()

Unicode.GetString()

Invoke-Expression
```

Purpose:

Hide PowerShell command execution through encoding and dynamic execution.

---

# Investigation Assessment

The observed activity indicates post-compromise reconnaissance, security discovery, defense evasion attempts, and obfuscated PowerShell execution.

The attacker used PowerShell to gather system information, enumerate users and services, inspect security configuration, review persistence locations, and execute encoded commands.
