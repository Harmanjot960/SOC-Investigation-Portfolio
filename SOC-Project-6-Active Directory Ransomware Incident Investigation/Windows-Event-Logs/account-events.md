# Windows Account Events

## Overview

This document contains account-related Windows event evidence observed during the investigation.

Account activity was analyzed to identify compromised credentials, privileged access, and Active Directory discovery.

---

## Compromised Account Usage

### Description

The attacker used the account `luke.sullivan` for remote authentication and administrative activity.

### Evidence

Observed authentication:

```text
User:

luke.sullivan


Source Address:

10.5.50.12
```

Target Systems:

```text
THM-SQL-SRV

THM-DEV-WS

THM-SHR-SRV
```

Authentication Type:

```text
Kerberos
```

### Findings

The account was used to access multiple systems during lateral movement.

MITRE ATT&CK:

- T1078 — Valid Accounts

---

## Active Directory Discovery

### Description

The attacker enumerated domain accounts and privileged groups.

### Commands Observed

```text
net user /domain

net group "Domain Admins" /domain

net group "Enterprise Admins" /domain

Get-ADUser

Get-ADComputer
```

### Detection

- Sysmon Event ID 1 — Process Creation
- PowerShell logging
- Active Directory events

### Findings

The attacker gathered information about domain users, groups, and computers.

MITRE ATT&CK:

- T1087 — Account Discovery
- T1069.002 — Permission Groups Discovery
