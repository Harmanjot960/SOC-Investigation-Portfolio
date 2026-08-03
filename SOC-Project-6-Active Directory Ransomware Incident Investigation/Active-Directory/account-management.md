# Active Directory Account Management

## Overview

This document contains account-related activity identified during the ransomware investigation.

Account activity was reviewed to identify compromised credentials and privileged account usage.

---

## Compromised Account Usage

### Description

The investigation identified the use of the account:

```text
luke.sullivan
```

for remote authentication and administrative activity across multiple domain systems.

### Evidence

Authentication activity:

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

### Findings

The account was used to access remote systems through SMB administrative shares and PsExec execution.

MITRE ATT&CK:

- T1078 — Valid Accounts

---

## Account Discovery Activity

### Description

The attacker performed Active Directory discovery to identify users and privileged groups.

### Observed Commands

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

The attacker enumerated domain accounts and privileged groups to understand the Active Directory environment.

MITRE ATT&CK:

- T1087 — Account Discovery
- T1069.002 — Permission Groups Discovery
