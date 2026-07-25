# Persistence Evidence

This document contains persistence mechanisms identified during the investigation.

---

# Startup Folder Persistence

The attacker placed a malicious payload inside:

```text
C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

Files placed in this location execute automatically during user logon.

Evidence:

- Sysmon Event ID 11 — File Creation
- Sysmon Event ID 1 — Process Creation

---

# Local Administrator Account Persistence

The attacker created:

```text
shion
```

The account was added to:

```text
Administrators
```

Evidence:

```text
Event ID 4720
User Account Created
```

```text
Event ID 4732
Member Added to Local Security Group
```

---

# Windows Service Persistence

Created service:

```text
TempestUpdate2
```

Payload:

```text
C:\ProgramData\final.exe
```

Purpose:

```text
Automatic execution after system startup
```

Evidence:

- Service creation command
- Sysmon process telemetry

---

# Persistence Summary

Identified persistence mechanisms:

```text
Startup Folder
        |
        ▼
Administrator Account
        |
        ▼
Windows Service
```
