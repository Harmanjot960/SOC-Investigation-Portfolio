# SOC Project 6 — Active Directory Ransomware Incident Investigation

## Overview

This project demonstrates a SOC investigation of a simulated ransomware attack against a Windows Active Directory environment.

The investigation reconstructs the attacker lifecycle by correlating:

* Windows Security Event Logs
* Sysmon telemetry
* Active Directory events
* PowerShell activity
* Network and file activity
* MITRE ATT&CK techniques

The investigation identified:

* Initial access through a compromised IIS web server
* Web shell execution
* Credential dumping
* Active Directory reconnaissance
* SMB and PsExec lateral movement
* Privileged account compromise
* Ransomware preparation activity
* Group Policy ransomware deployment

---

## Tools Used

* Sysmon
* Windows Event Viewer
* Splunk
* Active Directory Event Logs
* PowerShell Logs
* VirusTotal
* MITRE ATT&CK

---

## Environment

| Component           | Details                      |
| ------------------- | ---------------------------- |
| Analysis Platform   | Windows Active Directory Lab |
| Endpoint Telemetry  | Sysmon                       |
| SIEM                | Splunk                       |
| Windows Logs        | Security Event Logs          |
| Domain Environment  | Active Directory             |
| Threat Intelligence | VirusTotal                   |

---

## Incident Scenario

A threat actor compromised an exposed Windows IIS web server and gained execution through a web shell.

After gaining access, the attacker:

1. Executed PowerShell commands
2. Dumped credentials from LSASS
3. Enumerated Active Directory resources
4. Moved laterally using SMB and PsExec
5. Obtained elevated privileges
6. Prepared the environment for ransomware deployment
7. Modified Group Policy to distribute ransomware across domain systems

The investigation focused on detecting attacker activity before ransomware encryption occurred.

---

# Attack Chain

```
Internet
   |
   ▼
Compromised IIS Server
   |
   ▼
Web Shell
   |
   ▼
PowerShell Execution
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
Privilege Escalation
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

# Project Structure

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

# Investigation Findings

## Initial Access — Web Shell

The attacker gained access through a compromised IIS web server.

### Process Chain

```
w3wp.exe
    |
    ▼
cmd.exe
    |
    ▼
powershell.exe
```

### Detection

* IIS logs
* Sysmon Event ID 1 — Process Creation

### MITRE ATT&CK

* T1505.003 — Web Shell

---

## Credential Access — LSASS Dumping

The attacker attempted credential extraction using LSASS memory dumping techniques.

### Tools Observed

* mimikatz.exe
* procdump.exe

### Detection

* Sysmon Event ID 1 — Process Creation
* Sysmon Event ID 10 — Process Access

### MITRE ATT&CK

* T1003.001 — LSASS Memory

---

## Active Directory Discovery

The attacker performed domain reconnaissance using:

```
nltest

net user /domain

net group "Domain Admins"

net view

Get-ADUser

Get-ADComputer
```

### Detection

* Sysmon Event ID 1
* PowerShell logging
* Active Directory events

### MITRE ATT&CK

* T1087 — Account Discovery

---

## Lateral Movement

The attacker used SMB administrative shares and PsExec for remote execution.

### SMB Access

```
\\HOST\ADMIN$
```

### Evidence

* Event ID 5140 — Network Share Access
* Event ID 5145 — Detailed File Share Access
* Event ID 4624 — Logon Events

### PsExec

```
PsExec.exe \\TARGET
```

### Evidence

* Event ID 7045 — Service Installation
* PSEXESVC
* Sysmon Event ID 1

### MITRE ATT&CK

* T1021.002 — SMB/Windows Admin Shares
* T1569.002 — PsExec

---

## Ransomware Preparation

Before encryption, the attacker attempted to remove recovery options and reduce visibility.

### Commands Observed

```
vssadmin.exe delete shadows

ReAgentc.exe /disable

wbadmin.exe delete catalog

wevtutil.exe cl Security
```

### Detection

* Sysmon Event ID 1
* Security Event ID 4688
* Security Event ID 1102

### MITRE ATT&CK

* T1490 — Inhibit System Recovery
* T1070.001 — Clear Windows Event Logs

---

## Group Policy Ransomware Deployment

The attacker abused Active Directory Group Policy to distribute ransomware.

### Attack Flow

```
Domain Admin
      |
      ▼
SYSVOL Access
      |
      ▼
Upload Ransomware Payload
      |
      ▼
Modify Group Policy
      |
      ▼
Startup Script Execution
      |
      ▼
Domain-wide Deployment
```

### Detection

| Event ID | Purpose              |
| -------- | -------------------- |
| 11       | Payload Creation     |
| 5145     | SYSVOL Access        |
| 5136     | GPO Modification     |
| 5137     | GPO Creation         |
| 1        | Ransomware Execution |

---

# Indicators of Compromise

## Suspicious Commands

```
vssadmin.exe
ReAgentc.exe
wbadmin.exe
wevtutil.exe
```

## Attack Tools

```
mimikatz.exe
procdump.exe
PsExec.exe
```

## Ransomware Artifact

```
Office364.exe
```

## Active Directory Indicators

```
SYSVOL modification

GPO changes

Startup scripts
```

---

# MITRE ATT&CK Mapping

| Technique                 | ID        | Description            |
| ------------------------- | --------- | ---------------------- |
| Web Shell                 | T1505.003 | IIS compromise         |
| PowerShell                | T1059.001 | Command execution      |
| LSASS Dumping             | T1003.001 | Credential theft       |
| Account Discovery         | T1087     | AD enumeration         |
| SMB Admin Shares          | T1021.002 | Lateral movement       |
| PsExec                    | T1569.002 | Remote execution       |
| Inhibit System Recovery   | T1490     | Delete backups/shadows |
| Clear Windows Logs        | T1070.001 | Remove evidence        |
| Group Policy Modification | T1484.001 | Domain policy abuse    |
| Data Encrypted for Impact | T1486     | Ransomware execution   |

---

# Incident Response Recommendations

## Containment

* Isolate compromised systems
* Disable compromised accounts
* Block attacker infrastructure

## Investigation

* Review Sysmon process chains
* Hunt ransomware indicators
* Audit Group Policy modifications
* Investigate lateral movement activity

## Remediation

* Remove persistence mechanisms
* Restore recovery capabilities
* Reset compromised credentials
* Harden exposed services

## Detection Improvements

Monitor for:

* IIS spawning PowerShell
* LSASS access attempts
* PsExec execution
* Suspicious GPO modifications
* Shadow copy deletion
* Backup destruction commands

---

# Conclusion

This investigation demonstrates how SOC analysts detect and respond to ransomware attacks by correlating endpoint telemetry, Active Directory activity, and attacker behavior.

The investigation identified:

* Initial compromise
* Credential theft
* Domain compromise
* Lateral movement
* Ransomware preparation
* Group Policy abuse

The project highlights the importance of detecting ransomware activity before encryption begins.
