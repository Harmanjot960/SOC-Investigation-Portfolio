# SOC Project 6 — Active Directory Ransomware Incident Investigation

## Overview

This project demonstrates a SOC investigation of a simulated ransomware attack against a Windows Active Directory environment.

The investigation correlates:

- Windows Security Event Logs
- Sysmon telemetry
- Active Directory events
- Command-line activity
- SMB, file share, and authentication events

Splunk was used for log searching and event correlation to reconstruct attacker activity and identify ransomware deployment techniques before encryption occurred.

---

## Tools Used

- Splunk
- Sysmon
- Windows Event Viewer
- Active Directory Event Logs
- PowerShell Logs
- MITRE ATT&CK

---

## Investigation Source

This investigation is based on simulated Active Directory and ransomware attack scenarios from TryHackMe. Evidence from multiple scenarios was correlated to reconstruct a complete ransomware attack lifecycle, correlate attacker activity across multiple log sources, and map observed techniques to the MITRE ATT&CK framework.

---

## Environment

| Component | Details |
|---|---|
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
      |
      ▼
Event Correlation
      |
      ▼
Attack Timeline Reconstruction
      |
      ▼
IOC Identification
      |
      ▼
MITRE ATT&CK Mapping
      |
      ▼
Incident Report
```

---

## Attack Chain

```
Internet
    |
    ▼
Compromised IIS Server
    |
    ▼
Web Shell Execution
    |
    ▼
CMD & PowerShell Execution
    |
    ▼
Credential Dumping
(LSASS / Mimikatz)
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
Privileged Domain Access
    |
    ▼
Ransomware Preparation
    |
    ├── Delete Shadow Copies
    ├── Disable Recovery
    ├── Delete Backups
    └── Clear Logs
          |
          ▼
Group Policy Modification
          |
          ▼
Ransomware Deployment
```

---

## Project Structure

```
SOC-Project-6-Ransomware-Investigation
|
├── README.md
|
├── Evidence
│   ├── attack-timeline.md
│   ├── commands.md
│   ├── ransomware-preparation.md
│   ├── indicators-of-compromise.md
│   └── findings.md
|
├── Screenshots
│   ├── 01_iis_web_shell_execution.png
│   ├── 02_powershell_command_execution.png
│   ├── 03_lsass_credential_dumping.png
│   ├── 04_active_directory_discovery_commands.png
│   ├── 05_domain_group_discovery.png
│   ├── 06_smb_admin_share_access.png
│   ├── 07_psexec_lateral_movement.png
│   ├── 08_privileged_domain_access.png
│   ├── 09_vssadmin_shadow_copy_deletion.png
│   ├── 10_reagentc_recovery_disabled.png
│   ├── 11_wbadmin_backup_deletion.png
│   ├── 12_wevtutil_log_clearing.png
│   ├── 13_sysvol_ransomware_payload_upload.png
│   ├── 14_gpo_modification_event.png
│   ├── 15_startup_script_ransomware_execution.png
│   └── 16_ransomware_process_execution.png
|
├── Sysmon
│   ├── process-events.md
│   ├── file-events.md
│   ├── network-events.md
│   └── screenshots
|
├── Active-Directory
│   ├── authentication-events.md
│   ├── lateral-movement.md
│   └── group-policy-events.md
|
├── Windows-Event-Logs
│   ├── security-events.md
│   ├── account-events.md
│   └── ransomware-events.md
|
├── Incident-Report
│   └── incident-report.md
|
└── MITRE-ATT&CK
    └── attack-mapping.md
```

---

## Investigation Findings

### Initial Access — Web Shell Execution

The attacker gained initial access through a compromised IIS web server and executed commands through a web shell.

#### Evidence

Process execution chain:

```
w3wp.exe
    |
    ▼
cmd.exe
    |
    ▼
powershell.exe
```

#### Detection

- IIS logs
- Sysmon Event ID 1 — Process Creation

#### MITRE ATT&CK

- T1505.003 — Web Shell

---

### Credential Access — LSASS Dumping

The attacker attempted to extract credentials from LSASS memory using credential dumping techniques.

#### Tools Observed

- mimikatz.exe
- procdump.exe

#### Detection

- Sysmon Event ID 1 — Process Creation
- Sysmon Event ID 10 — Process Access

#### MITRE ATT&CK

- T1003.001 — LSASS Memory

---

### Active Directory Discovery

After gaining execution access, the attacker performed domain reconnaissance to identify users, groups, and domain resources.

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

- Sysmon Event ID 1
- PowerShell logs
- Active Directory events

#### MITRE ATT&CK

- T1087 — Account Discovery
- T1069.002 — Permission Groups Discovery
- T1018 — Remote System Discovery

---

### Lateral Movement — SMB and PsExec

The attacker used SMB administrative shares and PsExec for remote execution across the environment.

#### SMB Activity

```
\\HOST\ADMIN$
```

#### Evidence

- Event ID 5140 — Network Share Access
- Event ID 5145 — Detailed File Share Access
- Event ID 4624 — Logon Events

### PsExec Activity

```
PsExec.exe \\TARGET
```

#### Evidence

- Event ID 7045 — Service Installation
- PSEXESVC
- Sysmon Event ID 1

#### MITRE ATT&CK

- T1021.002 — SMB/Windows Admin Shares
- T1569.002 — PsExec

---

### Ransomware Preparation

Before ransomware deployment, the attacker attempted to disable recovery mechanisms and remove evidence from the environment.

#### Commands Observed

```
vssadmin.exe delete shadows

ReAgentc.exe /disable

wbadmin.exe delete catalog

wevtutil.exe cl Security
```

#### Detection

- Sysmon Event ID 1
- Security Event ID 4688 — Process Creation
- Security Event ID 1102 — Audit Log Cleared

#### MITRE ATT&CK

- T1490 — Inhibit System Recovery
- T1070.001 — Clear Windows Event Logs

---

### Group Policy Ransomware Deployment

The attacker abused Active Directory Group Policy to distribute ransomware across domain systems.

#### Attack Flow
```
Domain Admin → SYSVOL Access → Ransomware Payload Upload → Group Policy Modification → Startup Script Execution → Domain-wide Deployment
```

#### Detection

| Event ID | Purpose |
|---|---|
| Sysmon 11 | Payload File Creation |
| 5145 | SYSVOL Access |
| 5136 | Existing GPO Modification |
| 5137 | New GPO Creation |
| Sysmon 1 | Process Execution |

#### MITRE ATT&CK

- T1484.001 — Group Policy Modification
- T1486 — Data Encrypted for Impact

---

## Indicators of Compromise

### Suspicious Commands

```
vssadmin.exe
ReAgentc.exe
wbadmin.exe
wevtutil.exe
```

### Attack Tools

```
mimikatz.exe
procdump.exe
PsExec.exe
```

### Ransomware Artifact

```
Office364.exe
```

### Network Indicators

```
SMB ADMIN$ access
SYSVOL access
Remote service execution
```

### Active Directory Indicators

```
SYSVOL modification
GPO changes
Startup scripts
```

---

## MITRE ATT&CK Mapping

| Technique                         | ID        | Description                         |
| --------------------------------- | --------- | ----------------------------------- |
| Web Shell                         | T1505.003 | IIS web shell execution             |
| PowerShell                        | T1059.001 | Command execution and scripting     |
| OS Credential Dumping             | T1003.001 | LSASS memory credential extraction  |
| Account Discovery                 | T1087     | Domain user and account enumeration |
| Permission Groups Discovery       | T1069.002 | Domain group enumeration            |
| Remote System Discovery           | T1018     | Domain and host discovery          |
| SMB/Windows Admin Shares          | T1021.002 | Lateral movement using SMB          |
| PsExec                            | T1569.002 | Remote service execution            |
| Inhibit System Recovery           | T1490     | Delete backups and recovery options |
| Clear Windows Event Logs          | T1070.001 | Remove forensic evidence            |
| Group Policy Modification         | T1484.001 | Abuse Active Directory GPOs         |
| Data Encrypted for Impact         | T1486     | Ransomware encryption activity      |

---

## Incident Response Recommendations

### Containment

- Isolate compromised systems
- Disable compromised accounts
- Block attacker infrastructure

### Investigation

- Review Sysmon process chains
- Hunt ransomware indicators
- Audit Group Policy modifications
- Investigate lateral movement activity

### Remediation

- Remove attacker persistence mechanisms
- Restore recovery capabilities
- Reset compromised credentials
- Harden exposed services

### Detection Improvements

Monitor for:

- IIS spawning command shells or PowerShell
- LSASS access attempts
- PsExec execution
- Suspicious GPO modifications
- Shadow copy deletion
- Backup destruction commands

---

## Conclusion

This investigation demonstrates how SOC analysts detect and respond to ransomware attacks by correlating endpoint activity, Windows security events, and Active Directory changes.

The investigation identified:

- Initial compromise through a web shell
- Credential dumping activity
- Active Directory reconnaissance
- Lateral movement
- Ransomware preparation
- Group Policy abuse

The project highlights the importance of detecting attacker activity before ransomware encryption occurs.
