# Sysmon Event ID 11 — File Creation

Sysmon Event ID 11 was used to identify malware delivery, staging locations, and persistence artifacts.

---

# Downloaded Payloads

The attacker downloaded:

```text
update.zip

first.exe

ch.exe

spf.exe

final.exe
```

---

# Startup Folder Persistence

The attacker placed a malicious payload in:

```text
C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

Files in this directory execute automatically after user logon.

MITRE ATT&CK:

```text
T1547.001 - Registry Run Keys / Startup Folder
```

---

# Final.exe Investigation

Timeline analysis revealed:

```text
final.exe
```

was not extracted from:

```text
update.zip
```

Instead:

```text
WinRM Session
        |
        ▼
PowerShell
        |
        ▼
Download final.exe
        |
        ▼
Execute with SYSTEM privileges
```

Evidence:

- Sysmon Event ID 11
- Sysmon Event ID 1
- Timeline correlation

---

# Service Persistence Artifact

Created service payload:

```text
C:\ProgramData\final.exe
```

Service:

```text
TempestUpdate2
```

MITRE ATT&CK:

```text
T1543.003 - Windows Service
```
