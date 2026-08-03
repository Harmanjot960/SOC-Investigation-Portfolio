# MITRE ATT&CK Mapping

## Overview

This document maps the observed attacker activity to MITRE ATT&CK techniques identified during the investigation.

The mapping was based on evidence collected from:

- IIS Logs
- Sysmon telemetry
- Windows Security Event Logs
- PowerShell Script Block Logging
- Command-line activity
- Splunk analysis

---

## Attack Technique Mapping

| Technique | ID | Evidence |
|-----------|----|----------|
| Web Shell | T1505.003 | IIS web shell execution through compromised IIS server |
| Command and Scripting Interpreter | T1059 | Command execution through cmd.exe |
| PowerShell | T1059.001 | PowerShell Script Block Logging Event ID 4104 showing Get-ADUser and Get-ADComputer execution |
| OS Credential Dumping: LSASS Memory | T1003.001 | Procdump64.exe used to attempt LSASS credential extraction |
| Account Discovery | T1087 | Domain account enumeration using net user /domain and Get-ADUser |
| Permission Groups Discovery | T1069.002 | Domain group enumeration using net group and Get-ADGroupMember |
| Remote System Discovery | T1018 | Domain computer discovery using Get-ADComputer and nltest |
| SMB/Windows Admin Shares | T1021.002 | ADMIN$ access for lateral movement |
| Service Execution | T1569.002 | PsExec remote execution and PSEXESVC service creation |
| Valid Accounts | T1078 | Compromised accounts used for authentication and ransomware deployment |
| Inhibit System Recovery | T1490 | Shadow copy deletion using vssadmin |
| Clear Windows Event Logs | T1070.001 | Security log clearing attempt using wevtutil |
| Windows Management Instrumentation | T1047 | WMIC remote process creation used to deploy fixer.exe |

---

## Attack Lifecycle Mapping

```text
Initial Access
      |
      ▼
T1505.003
Web Shell
      |
      ▼
T1059 / T1059.001
Command Execution
      |
      ▼
T1003.001
Credential Dumping
      |
      ▼
T1087 / T1069.002 / T1018
Active Directory Discovery
      |
      ▼
T1021.002
SMB Lateral Movement
      |
      ▼
T1569.002
PsExec Execution
      |
      ▼
T1078
Valid Accounts
      |
      ▼
T1490 / T1070.001
Ransomware Preparation
      |
      ▼
T1047
WMIC Ransomware Deployment
