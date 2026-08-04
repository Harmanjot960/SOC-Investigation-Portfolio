# SOC Project 6 — Active Directory Ransomware Incident Investigation

## Overview

This project demonstrates a SOC investigation of a simulated ransomware attack against a Windows Active Directory environment.

The investigation correlates:

- Windows Security Event Logs
- Sysmon telemetry
- Active Directory events
- Command-line activity
- SMB, file share, and authentication events

Splunk was used to correlate security events, reconstruct the attacker timeline, and identify ransomware-related activity before encryption occurred.

---

## Tools Used

- Splunk
- Sysmon
- Windows Event Viewer
- Active Directory Event Logs
- PowerShell Logs
- MITRE ATT&CK Framework

---

## Investigation Source

This investigation is based on simulated Active Directory and ransomware attack scenarios from TryHackMe. Evidence from multiple scenarios was independently analyzed to reconstruct the attack timeline, correlate attacker activity across multiple log sources, and map observed techniques to the MITRE ATT&CK framework.

---

## Environment

| Component | Details |
|-----------|---------|
| Analysis Platform | Windows Active Directory Lab |
| Endpoint Monitoring | Sysmon |
| SIEM | Splunk |
| Windows Logs | Security Event Logs |
| Domain Environment | Active Directory |

---

## Incident Scenario

A Windows Active Directory environment showed signs of a ransomware intrusion originating from a compromised IIS web server.

The objective was to investigate attacker activity, determine the attack progression, identify compromised systems, and detect ransomware deployment before encryption occurred.

---

## Investigation Workflow

The investigation followed a SOC analysis workflow:

```
Evidence Review
      │
      ▼
Event Correlation
      │
      ▼
Attack Timeline Reconstruction
      │
      ▼
IOC Identification
      │
      ▼
MITRE ATT&CK Mapping
      │
      ▼
Incident Report
```

---

## Attack Chain
```
Internet
      |
      ▼
Compromised IIS Web Server
      |
      ▼
Web Shell Execution
      |
      ▼
CMD & PowerShell Execution
      |
      ▼
LSASS Credential Dumping
      |
      ▼
Privileged Credential Logon (luke.sullivan)
      |
      ▼
Active Directory Discovery
      |
      ▼
SMB Admin Share Access
      |
      ▼
PsExec Lateral Movement
      |
      ▼
Privileged Credential Usage (maria.garcia)
      |
      ▼
Ransomware Preparation
      |
      ├── Delete Shadow Copies
      └── Clear Event Logs
              |
              ▼
WMIC Remote Ransomware Deployment
              |
              ▼
fixer.exe Deployment Across Multiple Hosts

```

---

## Project Structure

```
SOC-Project-6-Ransomware-Investigation
│
├── README.md
│
├── Evidence
│   ├── attack-timeline.md
│   ├── commands.md
│   ├── indicators-of-compromise.md
│   ├── ransomware-preparation.md
│   └── findings.md
│
├── Screenshots
│   ├── 01_iis_web_shell_execution.png
│   ├── 02_webshell_process_creation.png
│   ├── 03_lsass_credential_dumping.png
│   ├── 04_procdump_execution.png
│   ├── 05_luke_sullivan_privileged_logon.png
│   ├── 06_powershell_active_directory_discovery.png
│   ├── 07_active_directory_discovery_commands.png
│   ├── 08_smb_admin_share_access.png
│   ├── 09_lateral_movement_source_host.png
│   ├── 10_psexec_remote_execution.png
│   ├── 11_psexec_service_installation.png
│   ├── 12_maria_garcia_privileged_credential_usage.png
│   ├── 13_vssadmin_shadow_copy_deletion.png
│   ├── 14_wevtutil_log_clearing.png
│   ├── 15_wmic_remote_execution.png
│   └── 16_remote_payload_creation.png
│
├── Sysmon
│   ├── process-events.md
│   ├── file-events.md
│   └── network-events.md
│
├── Active-Directory
│   ├── authentication-events.md
│   ├── lateral-movement.md
│   └── account-management.md
│
├── Windows-Event-Logs
│   ├── security-events.md
│   ├── account-events.md
│   └── process-events.md
│
├── Incident-Report
│   └── incident-report.md
│
└── MITRE-ATT&CK
    └── attack-mapping.md
```
## Investigation Findings

### Initial Access — IIS Web Shell Execution

The attacker gained initial access through a compromised IIS web server and obtained command execution through a web shell.

The web server process spawned command-line activity, indicating successful exploitation and remote command execution.

 #### Evidence

Process execution chain:

```text
w3wp.exe
│
└── cmd.exe
    │
    └── Command Execution
```

 #### Detection

- IIS logs
- Sysmon Event ID 1 — Process Creation

 #### MITRE ATT&CK

- T1505.003 — Web Shell

---

### Credential Access — LSASS Credential Dumping

After gaining execution access, the attacker attempted to extract credentials from LSASS memory.

Credential dumping activity was identified through suspicious process access attempts against the LSASS process.

 #### Tools Observed

```
procdump.exe
```

 #### Detection

- Sysmon Event ID 1 — Process Creation
- Sysmon Event ID 10 — Process Access

 #### MITRE ATT&CK

- T1003.001 — LSASS Memory

---

### Privileged Credential Logon

Following credential dumping, the attacker authenticated using the compromised domain account `luke.sullivan`, receiving special privileges required for administrative activity across the environment.

Observed Account
```
luke.sullivan
```

 #### Detection

- Event ID 4624 — Successful Logon
- Event ID 4672 — Special Privileges Assigned to New Logon

 #### MITRE ATT&CK

- T1078 — Valid Accounts

---

### Active Directory Discovery

The attacker performed domain reconnaissance to identify users, groups, and available domain resources.

These commands allowed the attacker to understand the Active Directory environment and identify privileged accounts.

 #### Commands Observed

```
nltest

net user /domain

net group "Domain Admins"

net view

Get-ADUser

Get-ADComputer
```

 #### Detection

- Sysmon Event ID 1 — Process Creation
- PowerShell logs
- Active Directory events

 #### MITRE ATT&CK

- T1087 — Account Discovery
- T1069.002 — Permission Groups Discovery
- T1018 — Remote System Discovery

---

### Lateral Movement — SMB and PsExec

The attacker used SMB administrative shares and PsExec to move laterally across the environment.

Administrative share access provided a method to transfer files and execute commands on remote systems.

 ### SMB Activity

Example:

```
\\THM-SQL-SRV\ADMIN$
```

 #### Detection

- Event ID 5140 — Network Share Access
- Event ID 5145 — Detailed File Share Access
- Event ID 4624 — Logon Events

---

 ### PsExec Remote Execution

The attacker used PsExec for remote command execution.

 #### Commands Observed

```
C:\Tools\PsExec.exe \THM-SQL-SRV cmd /c "net localgroup administrators"
C:\Tools\PsExec.exe -accepteula \THM-SQL-SRV cmd /c "hostname & whoami & ipconfig"
```

 #### Detection

- Event ID 7045 — Service Installation
- PSEXESVC service creation
- Sysmon Event ID 1 — Process Creation

 #### MITRE ATT&CK

- T1021.002 — SMB/Windows Admin Shares
- T1569.002 — Service Execution

---

### Ransomware Preparation

Before ransomware deployment, the attacker attempted to reduce recovery capability and remove forensic evidence.

 #### Commands Observed
```
vssadmin.exe delete shadows
wevtutil.exe cl Security
```

 #### Detection

- Sysmon Event ID 1 — Process Creation
- Security Event ID 4688 — Process Creation

 #### MITRE ATT&CK

- T1490 — Inhibit System Recovery
- T1070.001 — Clear Windows Event Logs

---

### WMIC Remote Ransomware Deployment

After gaining administrative access during lateral movement, the attacker used another compromised privileged account, `maria.garcia`, to perform remote ransomware deployment.

The attacker used the compromised account `maria.garcia` to remotely deploy the ransomware payload (`fixer.exe`) across multiple systems using Windows Management Instrumentation Command-line (WMIC). The activity originated from the Domain Controller and leveraged WMIC remote process creation to execute the ransomware payload on multiple hosts.

 #### Attack Chain

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

 #### Evidence

**Sysmon Event ID 1 — Process Creation**

```text
Image:
C:\Windows\System32\wbem\WMIC.exe

CommandLine:
wmic /node:<target-host> /user:maria.garcia /password:<password> process call create C:\Windows\fixer.exe
```

The attacker used the compromised account `maria.garcia` to authenticate and perform remote administrative execution across multiple systems.

**Affected Systems**

```text
tsm-prod-01
tsm-prod-02
tsm-prod-03
tsm-prod-04
tsm-prod-05
tsm-prod-06
```

 #### Detection

- Sysmon Event ID 1 — Process Creation
- Sysmon Event ID 11 — File Creation
- Windows Security Event ID 4624 — Successful Logon
- Windows Security Event ID 4672 — Special Privileges Assigned to New Logon

 #### MITRE ATT&CK

- T1047 — Windows Management Instrumentation
- T1078 — Valid Accounts

---

### Ransomware Payload Creation

The ransomware payload (`fixer.exe`) was created on the target systems through command-line execution. WMIC was used to remotely execute the deployment process across multiple hosts.

**Sysmon Event ID 11 — File Creation**

```text
Image:
powershell.exe

TargetFilename:
C:\Windows\fixer.exe
```

 #### Detection

- Sysmon Event ID 1 — Process Creation
- Sysmon Event ID 11 — File Creation
- Command-line logging
- Authentication events

 #### MITRE ATT&CK

- T1078 — Valid Accounts
- T1047 — Windows Management Instrumentation

---

## Indicators of Compromise

### Suspicious Processes

```
updater.exe

wmic.exe

cmd.exe

powershell.exe

fixer.exe
```

---

### Attack Tools

```
procdump.exe

PsExec.exe
```

---

### Payload Artifact

```
C:\Windows\fixer.exe
```

---

### Command Indicators

```
wmic process call create

vssadmin delete shadows

wevtutil cl Security
```

---

### Network / Remote Execution Indicators

```
WMIC remote process creation

SMB ADMIN$ access

Remote service execution

Multiple remote hosts:
tsm-prod-01
tsm-prod-02
tsm-prod-03
tsm-prod-04
tsm-prod-05
tsm-prod-06
```

## MITRE ATT&CK Mapping

| Technique | ID | Description |
|-----------|----|-------------|
| Web Shell | T1505.003 | IIS web shell execution |
| PowerShell | T1059.001 | Command execution and scripting |
| OS Credential Dumping | T1003.001 | LSASS memory credential extraction |
| Account Discovery | T1087 | Domain user and account enumeration |
| Permission Groups Discovery | T1069.002 | Domain group enumeration |
| Remote System Discovery | T1018 | Domain and host discovery |
| SMB/Windows Admin Shares | T1021.002 | Lateral movement using SMB |
| PsExec | T1569.002 | Remote service execution |
| Valid Accounts | T1078 | Use of compromised domain credentials |
| Windows Management Instrumentation | T1047 | Remote execution using WMIC |
| Inhibit System Recovery | T1490 | Delete backups and recovery options |
| Clear Windows Event Logs | T1070.001 | Remove forensic evidence |

---

## Incident Response Recommendations

### Containment

- Isolate compromised systems involved in the attack chain.
- Disable or reset compromised domain accounts.
- Restrict unnecessary administrative access.
- Block suspicious remote execution activity.

---

### Investigation

- Review Sysmon process creation chains.
- Investigate suspicious parent-child relationships such as:
  - IIS worker process spawning command shells.
  - Unknown applications launching WMIC.
  - Remote process creation activity.
- Review authentication events for compromised accounts.
- Investigate SMB administrative share usage.
- Audit privileged account activity.

---

### Remediation

- Remove unauthorized tools and payloads.
- Reset compromised credentials.
- Review and apply least privilege access controls.
- Harden exposed web services.
- Restrict remote administration methods where not required.
- Enable enhanced logging for PowerShell and process execution.

---

## Detection Improvements

Monitor for:

- IIS processes spawning `cmd.exe` or `powershell.exe`.
- Suspicious LSASS memory access attempts.
- Credential dumping tools such as `procdump.exe`.
- SMB ADMIN$ access from unusual systems.
- PsExec service creation.
- WMIC remote process execution.
- Suspicious payload creation in system directories.
- Recovery destruction commands:
  - `vssadmin`
  - `wevtutil`

---

## Conclusion

The investigation identified:

- Initial access through an IIS web shell.
- Command execution through CMD and PowerShell.
- LSASS credential dumping and privileged credential acquisition.
- Active Directory reconnaissance.
- SMB and PsExec lateral movement.
- Privileged credential usage for ransomware deployment.
- Ransomware preparation through shadow copy deletion and event log clearing.
- WMIC-based remote payload deployment across multiple systems.

The project highlights the importance of detecting attacker behavior before ransomware execution and limiting the impact of domain-wide compromise through early investigation and response.
