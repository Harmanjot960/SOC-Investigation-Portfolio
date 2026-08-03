# Active Directory Authentication Events

## Overview

This document contains Active Directory authentication activity identified during the ransomware investigation.

Authentication events were analyzed to identify compromised account usage, remote logons, and attacker movement across domain systems.

---

## Privileged Credential Usage

### Description

The attacker used the compromised account `luke.sullivan` to authenticate to multiple systems across the Active Directory environment.

### Detection

Windows Security Event ID 4624 — Successful Logon

### Evidence

Splunk Query:

```spl
index=win EventCode=4624 Source_Network_Address="10.5.50.12" user="luke.sullivan"
| table _time host user Logon_Type Source_Network_Address Authentication_Package
| sort _time
```

Observed Activity:

```text
Source Address:

10.5.50.12


User:

luke.sullivan


Authentication:

Kerberos
```

Affected Systems:

```text
THM-SQL-SRV

THM-DEV-WS

THM-SHR-SRV
```

### Findings

The account `luke.sullivan` was used for remote authentication from the source system `10.5.50.12`, supporting lateral movement activity.

MITRE ATT&CK:

- T1078 — Valid Accounts
