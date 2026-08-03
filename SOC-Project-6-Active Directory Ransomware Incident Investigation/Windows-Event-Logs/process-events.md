# Windows Process Events

## Overview

This document contains Windows process execution evidence observed during the ransomware investigation.

Process events were correlated with Sysmon telemetry to identify attacker execution chains and malicious activity.

---

# Remote Command Execution

## Description

The attacker executed commands remotely using PsExec.

## Evidence

Observed Commands:

```text
C:\Tools\PsExec.exe \\THM-SQL-SRV cmd /c "hostname & whoami & ipconfig"

C:\Tools\PsExec.exe \\THM-SQL-SRV cmd /c "net localgroup administrators"
```

Detection:

- Sysmon Event ID 1 — Process Creation

## Findings

PsExec execution originated from:

```text
Source:

THM-MKT-WS


User:

luke.sullivan


Target:

THM-SQL-SRV
```

MITRE ATT&CK:

- T1569.002 — Service Execution

---

# Ransomware Preparation Commands

## Description

The attacker executed commands to reduce recovery capability and remove forensic evidence.

## Evidence

Observed Commands:

```text
vssadmin delete shadows /all /quiet

wevtutil cl Security
```

Detection:

- Sysmon Event ID 1 — Process Creation

## Findings

The commands indicate preparation activity before ransomware deployment.

MITRE ATT&CK:

- T1490 — Inhibit System Recovery
- T1070.001 — Clear Windows Event Logs

---

# WMIC Payload Deployment

## Description

The attacker used WMIC to remotely execute the ransomware payload.

## Evidence

Observed Command:

```text
wmic /node:<target-host> /user:maria.garcia /password:<password> process call create C:\Windows\fixer.exe
```

Detection:

- Sysmon Event ID 1 — Process Creation

## Findings

WMIC was used to deploy `fixer.exe` across multiple systems.

MITRE ATT&CK:

- T1047 — Windows Management Instrumentation
