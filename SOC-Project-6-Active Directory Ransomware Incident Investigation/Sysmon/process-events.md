# Sysmon Process Events

## Overview

This document contains Sysmon Event ID 1 (Process Creation) evidence identified during the ransomware investigation.

Process creation events were analyzed to identify attacker execution activity, suspicious tools, lateral movement activity, and ransomware deployment behavior.

---

## IIS Web Shell Execution

### Description

The attacker gained command execution through a compromised IIS web server.

The IIS worker process (`w3wp.exe`) spawned command-line activity used to execute attacker commands.

### Detection

- Sysmon Event ID 1 — Process Creation

### Evidence

Splunk Query:

```spl
index="win" EventCode=1 ParentImage="*w3wp.exe"
| table _time Image CommandLine
```

Observed Process Activity:

```text
w3wp.exe
    |
    ▼
cmd.exe
    |
    ▼
Attacker Commands
```

Observed Commands:

```text
hostname

tasklist

netstat -an

dir C:\inetpub\wwwroot

net group "Domain Admins" /domain
```

### MITRE ATT&CK

- T1505.003 — Web Shell
- T1059 — Command and Scripting Interpreter

---

## LSASS Credential Dumping

### Description

The attacker attempted to extract credentials from LSASS memory using Procdump.

### Detection

- Sysmon Event ID 1 — Process Creation

### Evidence

Splunk Query:

```spl
index=task4 EventCode=1 lsass.exe
| table Image ParentImage CommandLine
```

Observed Activity:

```text
Image:

C:\Windows\Temp\procdump64.exe


ParentImage:

C:\Windows\System32\cmd.exe


CommandLine:

C:\Windows\Temp\procdump64.exe -accepteula -ma lsass.exe C:\Windows\Temp\lsass.dmp
```

### MITRE ATT&CK

- T1003.001 — LSASS Memory

---

## Active Directory Discovery

### Description

The attacker performed Active Directory reconnaissance to identify users, groups, trusts, and domain resources.

### Detection

- Sysmon Event ID 1 — Process Creation
- PowerShell logging

### Evidence

Splunk Query:

```spl
index=win EventCode=1
| search CommandLine IN ("*nltest*", "*net * user*", "*net * group*", "*net * view*", "*net * localgroup*")
| table _time host User Image CommandLine ParentImage
| sort _time
```

Observed Commands:

```text
nltest /domain_trusts

net user /domain

net group "Domain Admins" /domain

net group "Enterprise Admins" /domain

net localgroup Administrators

net view
```

### MITRE ATT&CK

- T1087 — Account Discovery
- T1069.002 — Permission Groups Discovery
- T1018 — Remote System Discovery

---

## PsExec Remote Execution

### Description

The attacker used PsExec to execute commands remotely across the environment.

### Detection

- Sysmon Event ID 1 — Process Creation

### Evidence

Splunk Query:

```spl
index=win EventCode=1 host=THM-MKT-WS Image="*PsExec.exe"
| table _time host User Image CommandLine
```

Observed Activity:

```text
User:

tryhatmestudios\luke.sullivan


Command:

C:\Tools\PsExec.exe \\THM-SQL-SRV cmd /c "hostname & whoami & ipconfig"


Command:

C:\Tools\PsExec.exe \\THM-SQL-SRV cmd /c "net localgroup administrators"
```

Source Host:

```text
THM-MKT-WS
```

Target Host:

```text
THM-SQL-SRV
```

### MITRE ATT&CK

- T1569.002 — Service Execution

---

## WMIC Remote Ransomware Deployment

### Description

The attacker used WMIC remote process creation to deploy the ransomware payload (`fixer.exe`) across multiple systems.

### Detection

- Sysmon Event ID 1 — Process Creation

### Evidence

Splunk Query:

```spl
index=* EventCode=1 Image="*WMIC.exe"
| table Image host CommandLine ParentImage
```

Observed Activity:

```text
Image:

C:\Windows\System32\wbem\WMIC.exe


CommandLine:

wmic /node:<target-host> /user:maria.garcia /password:<password> process call create C:\Windows\fixer.exe
```

Process Chain:

```text
updater.exe
      |
      ▼
cmd.exe
      |
      ▼
WMIC.exe
      |
      ▼
Remote Process Creation
      |
      ▼
C:\Windows\fixer.exe
```

Affected Systems:

```text
tsm-prod-01
tsm-prod-02
tsm-prod-03
tsm-prod-04
tsm-prod-05
tsm-prod-06
```

### MITRE ATT&CK

- T1047 — Windows Management Instrumentation

---

## Ransomware Preparation

### Shadow Copy Deletion

### Detection

- Sysmon Event ID 1 — Process Creation

### Evidence

Splunk Query:

```spl
index=* vssadmin.exe EventCode=1
| table Image host CommandLine
```

Observed Activity:

```text
Image:

C:\Windows\System32\vssadmin.exe


CommandLine:

vssadmin delete shadows /all /quiet
```

### MITRE ATT&CK

- T1490 — Inhibit System Recovery

---

## Windows Event Log Clearing

### Description

The attacker attempted to remove forensic evidence by clearing Windows Security logs.

### Detection

- Sysmon Event ID 1 — Process Creation

### Evidence

Splunk Query:

```spl
index=* EventCode=1 "wevtutil"
| table Image host CommandLine
```

Observed Activity:

```text
Image:

C:\Windows\System32\wevtutil.exe


CommandLine:

wevtutil cl Security
```

### MITRE ATT&CK

- T1070.001 — Clear Windows Event Logs
