# Sysmon Network Events

## Overview

This document contains network-related activity observed during the ransomware investigation.

Network evidence was analyzed to identify remote access, lateral movement, SMB activity, and attacker communication between systems.

---

## SMB Administrative Share Access

### Description

The attacker used SMB administrative shares to access remote systems and support lateral movement activity.

### Detection

Windows Security Events were used to identify SMB access activity.

### Evidence

Source Address:

```text
10.5.50.12
```

Account:

```text
luke.sullivan
```

Accessed Shares:

```text
ADMIN$
C$
```

Target Systems:

```text
THM-DEV-WS

THM-SHR-SRV

THM-SQL-SRV
```

Related Events:

- Event ID 5140 — Network Share Access
- Event ID 5145 — Detailed File Share Access
- Event ID 4624 — Logon Events

### Findings

The attacker used SMB administrative shares to move files and support remote execution.

MITRE ATT&CK:

- T1021.002 — SMB/Windows Admin Shares

---

## PsExec Remote Communication

### Description

The attacker used PsExec to execute commands on remote systems.

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
tryhatmestudios\luke.sullivan
```

Remote Commands:

```text
hostname & whoami & ipconfig

net localgroup administrators
```

Related Events:

- Event ID 4624 — Remote Logon
- Event ID 7045 — Service Installation
- Sysmon Event ID 1 — Process Creation

### Findings

The attacker established remote execution capability using PsExec and SMB-based administrative access.

MITRE ATT&CK:

- T1021.002 — SMB/Windows Admin Shares
- T1569.002 — Service Execution

---

## WMIC Remote Deployment Activity

### Description

The attacker used WMIC remote process creation to deploy the ransomware payload across multiple hosts.

### Evidence

Remote Execution Targets:

```text
tsm-prod-01

tsm-prod-02

tsm-prod-03

tsm-prod-04

tsm-prod-05

tsm-prod-06
```

Payload:

```text
C:\Windows\fixer.exe
```

Execution Method:

```text
wmic /node:<target-host> process call create C:\Windows\fixer.exe
```

### Findings

The attacker used remote management functionality to distribute and execute ransomware payloads across domain systems.

MITRE ATT&CK:

- T1047 — Windows Management Instrumentation
