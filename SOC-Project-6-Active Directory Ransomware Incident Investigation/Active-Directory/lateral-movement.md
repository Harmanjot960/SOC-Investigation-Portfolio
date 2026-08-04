# Active Directory Lateral Movement

## Overview

This document contains Active Directory lateral movement activity observed during the ransomware investigation.

The attacker accessed SMB administrative shares and PsExec to support lateral movement and remote execution activity.

---

## SMB Administrative Share Access

### Description

The attacker accessed administrative shares to transfer files and enable remote execution.

### Detection

Windows Security Events:

- Event ID 5140 — Network Share Access
- Event ID 5145 — Detailed File Share Access

### Evidence

Source Address:

```text
10.5.50.12
```

Account:

```text
luke.sullivan
```

Accessed Share:

```text
ADMIN$
```

Target Systems:

```text
THM-DEV-WS

THM-SHR-SRV

THM-SQL-SRV
```

### Findings

The attacker used SMB administrative shares as part of the lateral movement process.

MITRE ATT&CK:

- T1021.002 — SMB/Windows Admin Shares

---

## PsExec Remote Execution

### Description

The attacker used PsExec to execute commands remotely on domain systems.

### Evidence

Source Host:

```text
THM-MKT-WS
```

Target Host:

```text
THM-SQL-SRV
```

User:

```text
luke.sullivan
```

Commands Executed:

```text
hostname & whoami & ipconfig

net localgroup administrators
```

Detection:

- Sysmon Event ID 1 — Process Creation
- Event ID 7045 — Service Installation

Service Created:

```text
PSEXESVC
```

Service Binary:

```text
%SystemRoot%\PSEXESVC.exe
```

### Findings

The attacker used PsExec to establish remote command execution on the target host.

MITRE ATT&CK:

- T1569.002 — Service Execution
