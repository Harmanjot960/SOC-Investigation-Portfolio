# PowerShell Post-Compromise Investigation Timeline

## Incident Timeline

This timeline documents attacker activity observed after successful RDP authentication.

The investigation combines Windows Security Logs, PowerShell Script Block Logging (Event ID 4104), and Sysmon Process Creation events (Event ID 1).

---

| Time | Event ID | Source | Activity |
|---|---|---|---|
| 2026-07-14 08:23:11 | 4624 | Windows Security | Successful RDP authentication |
| 2026-07-15 23:34:20 | 1 | Sysmon | powershell.exe process creation detected |
| 2026-07-15 23:34:21 | 4104 | PowerShell | PowerShell Script Block Logging started |
| 2026-07-15 23:38:49 | 4104 | PowerShell | Execution policy discovery using Get-ExecutionPolicy |
| 2026-07-15 23:42:00 - 23:42:15 | 4104 | PowerShell | Security and scheduled task discovery |
| 2026-07-15 23:43:00 | 4104 | PowerShell | Execution policy bypass attempted |
| 2026-07-15 23:43:20 | 4104 | PowerShell | Encoded payload decoding and execution |
| 2026-07-15 23:47:49 - 23:48:48 | 4104 | PowerShell | Encoded PowerShell command execution detected |

---

# Investigation Phases

## Phase 1 — Initial Access Context

Event ID 4624 confirmed successful authentication.

```
Event ID:
4624

Activity:
Successful RDP authentication
```

---

## Phase 2 — PowerShell Execution

Sysmon Event ID 1 confirmed PowerShell execution.

Observed:

```
Image:
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

ParentImage:
C:\Windows\explorer.exe
```

---

## Phase 3 — Discovery Activity

The attacker performed reconnaissance:

```
whoami

hostname

Get-ComputerInfo

Get-Process

Get-Service
```

Purpose:

- Identify system information
- Identify user context
- Enumerate running processes and services

---

## Phase 4 — Security Discovery

The attacker checked security configuration:

```
Get-MpPreference
```

Purpose:

- Identify Windows Defender settings
- Gather information before possible defense evasion

---

## Phase 5 — Defense Evasion

The attacker attempted PowerShell restriction bypass:

```
Set-ExecutionPolicy Bypass -Scope Process
```

---

## Phase 6 — Encoded PowerShell Execution

Encoded PowerShell activity was detected:

```
powershell.exe -EncodedCommand
```

Observed decoding behavior:

```
FromBase64String()

Unicode.GetString()

Invoke-Expression
```

This indicates PowerShell command obfuscation and possible defense evasion.
