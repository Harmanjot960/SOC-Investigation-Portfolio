# Sysmon Event ID 1 — Process Creation

Sysmon Event ID 1 was used to identify malicious process execution and reconstruct the attacker execution chain.

---

# Initial Exploitation Chain

The malicious Microsoft Word document:

```text
free_magicules.doc
```

triggered the Follina vulnerability.

Observed process chain:

```text
WINWORD.EXE
      |
      ▼
msdt.exe
      |
      ▼
PowerShell.exe
```

Evidence:

- Malicious document execution
- Parent-child process relationship
- Command-line telemetry

---

# PowerShell Execution

The attacker executed:

```text
powershell.exe
```

Observed activity:

- Encoded commands
- Payload retrieval
- Command execution
- Malware staging

MITRE ATT&CK:

```text
T1059.001 - PowerShell
```

---

# Malware Execution

Observed attacker binaries:

```text
first.exe

ch.exe

spf.exe

final.exe
```

Observed malware execution sequence:

```text
PowerShell
      |
      ▼
first.exe
      |
      ▼
ch.exe
      |
      ▼
WinRM Session
      |
      ▼
spf.exe (PrintSpoofer)
      |
      ▼
final.exe
```

---

# WinRM Remote Execution

The attacker accessed the endpoint through WinRM.

Observed process:

```text
wsmprovhost.exe
```

This confirmed remote PowerShell execution.

MITRE ATT&CK:

```text
T1021.006 - Windows Remote Management
```

---

# Privilege Escalation

The attacker executed:

```text
spf.exe
```

identified as:

```text
PrintSpoofer
```

Result:

```text
NT AUTHORITY\SYSTEM
```

---

# Account Creation Activity

The attacker created a new local user account:

```text
shion
```

Observed commands:

```cmd
net user shion <password> /add
```

The attacker added the account to the local administrators group:

```cmd
net localgroup administrators shion /add
```

Purpose:

- Create attacker-controlled account
- Obtain persistent administrative access

Evidence:

- Sysmon Event ID 1 — Process Creation
- Command-line telemetry
- Windows Security Event ID 4720
- Windows Security Event ID 4732

MITRE ATT&CK:

```text
T1136.001 - Create Account: Local Account
```
---

# Persistence Execution

The attacker created a Windows service:

```text
TempestUpdate2
```

Payload:

```text
C:\ProgramData\final.exe
```

Evidence:

- Sysmon Event ID 1
- Command-line analysis
