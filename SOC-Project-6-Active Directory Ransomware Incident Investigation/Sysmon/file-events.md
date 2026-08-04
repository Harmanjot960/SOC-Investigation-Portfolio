# Sysmon File Events

## Overview

This document contains Sysmon file activity observed during the ransomware investigation.

File creation events were analyzed to identify attacker payload deployment, suspicious file creation, and ransomware-related artifacts.

---

## Ransomware Payload Creation

### Description

The attacker remotely deployed and created the ransomware payload (fixer.exe) on target systems using WMIC remote process creation.

### Detection

- Sysmon Event ID 11 — File Create

### Evidence

Splunk Query:

```spl
index=* EventCode=11 TargetFilename="*fixer.exe"
| table _time Image TargetFilename host
```

Observed Activity:

```text
Image:

powershell.exe


TargetFilename:

C:\Windows\fixer.exe
```

### Findings

The file `fixer.exe` was created on affected systems and later executed through WMIC remote process creation.

MITRE ATT&CK:

- T1105 — Ingress Tool Transfer
- T1047 — Windows Management Instrumentation

---

## PsExec File Transfer Activity

### Description

The attacker used SMB administrative shares to support PsExec-based remote execution.

PsExec transfers its service binary (`PSEXESVC.exe`) to the remote system before execution.

### Detection

- Sysmon Event ID 11 — File Create
- Windows Security Event ID 5145 — Detailed File Share Access

### Evidence

Observed Artifact:

```text
PSEXESVC.exe
```

Location:

```text
%SystemRoot%\PSEXESVC.exe
```

Related Activity:

```text
ADMIN$ share access

Remote service creation

PsExec execution
```

### Findings

The file activity supports PsExec lateral movement by showing the creation of the PsExec service binary on the remote host.

MITRE ATT&CK:

- T1021.002 — SMB/Windows Admin Shares
- T1569.002 — Service Execution

---

## Ransomware Artifact

### Description

The identified ransomware payload used during deployment was:

```text
C:\Windows\fixer.exe
```

### Detection

- Sysmon Event ID 11 — File Create
- Sysmon Event ID 1 — Process Creation

### Findings

The attacker distributed the ransomware payload across:

```text
tsm-prod-01

tsm-prod-02

tsm-prod-03

tsm-prod-04

tsm-prod-05

tsm-prod-06
```

The investigation confirmed ransomware deployment activity but did not identify successful encryption impact.

MITRE ATT&CK:

- T1486 — Data Encrypted for Impact
