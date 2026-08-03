# Windows Security Events

## Overview

This document contains Windows Security Event Log evidence identified during the ransomware investigation.

Security events were analyzed to identify authentication activity, SMB access, lateral movement, and attacker actions within the Active Directory environment.

---

# Successful Logon Activity

## Description

The attacker used compromised credentials to authenticate to multiple systems across the domain.

## Detection

Windows Security Event ID 4624 — Successful Logon

## Evidence

Splunk Query:

```spl
index=win EventCode=4624 Source_Network_Address="10.5.50.12" user="luke.sullivan"
| table _time host user Logon_Type Source_Network_Address Authentication_Package
| sort _time
```

Observed Activity:

```text
User:

luke.sullivan


Source Address:

10.5.50.12


Authentication:

Kerberos
```

Affected Systems:

```text
THM-SQL-SRV

THM-DEV-WS

THM-SHR-SRV
```

## Findings

The logon activity confirmed the use of compromised credentials for remote access.

MITRE ATT&CK:

- T1078 — Valid Accounts

---

# SMB Administrative Share Access

## Description

The attacker accessed Windows administrative shares as part of lateral movement.

## Detection

- Event ID 5140 — Network Share Access
- Event ID 5145 — Detailed File Share Access

## Evidence

Splunk Query:

```spl
index=win EventCode=5140 Share_Name IN ("*\\ADMIN$*", "*\\C$*")
| table _time host Source_Address user Share_Name
| sort _time
```

Observed Activity:

```text
Source Address:

10.5.50.12


User:

luke.sullivan


Share:

ADMIN$
```

Target Systems:

```text
THM-DEV-WS

THM-SHR-SRV

THM-SQL-SRV
```

## Findings

The attacker used administrative shares to support remote execution and file transfer activity.

MITRE ATT&CK:

- T1021.002 — SMB/Windows Admin Shares

---

# PsExec Service Installation

## Description

PsExec created a temporary service on the remote system to execute commands.

## Detection

Windows Security Event ID 7045 — Service Installation

## Evidence

Splunk Query:

```spl
index=win EventCode=7045 Service_Name=PSEXESVC
| table _time host Service_Name Service_File_Name Service_Account
```

Observed Activity:

```text
Service Name:

PSEXESVC


Service File:

%SystemRoot%\PSEXESVC.exe


Account:

LocalSystem
```

## Findings

The service creation confirmed PsExec remote execution on the target system.

MITRE ATT&CK:

- T1569.002 — Service Execution
