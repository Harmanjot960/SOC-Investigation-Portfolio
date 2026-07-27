# Windows Security Event Analysis

## Overview

Windows Security Event Logs were analyzed to identify attacker authentication activity and post-compromise actions.

The investigation focused on identifying:

- Successful authentication
- Remote access activity
- Privileged actions
- Security-impacting events

---

## Successful Authentication

### Event ID 4624 — Successful Logon

Event ID 4624 was analyzed to identify successful logon activity after credential discovery activity.

Observed activity:

```text
Remote authentication activity
```

The investigation correlated:

```text
Windows Security Event Logs
        +
Sysmon Process Events
        +
Network Traffic Analysis
```

to confirm attacker access.

---

## WinRM Authentication

The attacker used discovered credentials to access the system through:

Related endpoint activity:

```text
wsmprovhost.exe
```

This confirmed the creation of a remote PowerShell session.

Evidence:

- Windows Security authentication events
- Sysmon Event ID 1 — Process Creation
- Sysmon Event ID 3 — Network Connection

---

## Security Event Correlation

The following evidence sources were correlated:

```text
Authentication Events
        |
        ▼
Remote Session Creation
        |
        ▼
Command Execution
        |
        ▼
Privilege Escalation
```

This timeline confirmed that legitimate credentials were abused for remote access after initial compromise.
