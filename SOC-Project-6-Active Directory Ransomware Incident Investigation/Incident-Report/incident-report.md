# Incident Report — Active Directory Ransomware Incident Investigation

---

## Incident Title

Active Directory Ransomware Investigation Following Web Shell Compromise and Remote Ransomware Payload Deployment

---

## Executive Summary

A Security Operations Center (SOC) investigation was initiated after suspicious activity was identified within a Windows Active Directory environment.

Analysis of IIS logs, Sysmon telemetry, and Windows Security Events confirmed that an attacker gained initial access through a compromised IIS web server and executed commands through a web shell.

Following initial access, the attacker performed credential dumping attempts against LSASS, conducted Active Directory reconnaissance, accessed SMB administrative shares, and used PsExec for lateral movement.

The attacker later performed ransomware preparation activities by attempting to remove recovery capabilities and clear security logs. The attacker then used compromised domain credentials and Windows Management Instrumentation Command-line (WMIC) to remotely deploy the ransomware payload (`fixer.exe`) across multiple domain systems.

The investigation confirmed ransomware deployment activity; however, no evidence of successful encryption impact was identified.

---

## Incident Information

| Field | Details |
|---|---|
| Incident Type | Active Directory Ransomware Investigation |
| Investigation Type | SOC Incident Investigation |
| Severity | High |
| Status | Confirmed Malicious Activity |
| Initial Access | IIS Web Shell |
| Affected Environment | Windows Active Directory Domain |
| Evidence Sources | IIS Logs, Sysmon, Windows Security Events |
| Analysis Platform | Splunk |
| Primary Focus | Attacker Activity Reconstruction |

---

## Environment Information

| Component | Details |
|---|---|
| SIEM | Splunk |
| Endpoint Monitoring | Sysmon |
| Windows Logs | Security Event Logs |
| Domain Environment | Active Directory |
| Investigation Platform | Windows AD Lab |

---

## Investigation Scope

The objective of this investigation was to reconstruct the attacker lifecycle and identify malicious activity before ransomware encryption occurred.

The investigation focused on:

- Initial access
- Command execution
- Credential dumping
- Active Directory discovery
- Lateral movement
- Privileged account usage
- Ransomware preparation
- Remote ransomware deployment

---

# Investigation Timeline

## Phase 1 — Initial Access: IIS Web Shell Execution

The attacker gained access through a compromised IIS web server and executed commands through a web shell.

### Observed Web Shell Activity

    GET /aspnet_client/system_web/error.aspx?cmd=hostname
    GET /aspnet_client/system_web/error.aspx?cmd=tasklist
    GET /aspnet_client/system_web/error.aspx?cmd=netstat -an
    GET /aspnet_client/system_web/error.aspx?cmd=dir C:\inetpub\wwwroot
    GET /aspnet_client/system_web/error.aspx?cmd=net group "Domain Admins" /domain

The IIS worker process spawned command execution activity.

### Process Execution Chain

    w3wp.exe
    │
    └── cmd.exe
        │
        └── Command Execution

### Evidence

- IIS Logs
- Sysmon Event ID 1 — Process Creation

### MITRE ATT&CK

- T1505.003 — Web Shell


---

## Phase 2 — Credential Access: LSASS Credential Dumping

After obtaining command execution, the attacker attempted to extract credentials from LSASS memory.

### Observed Credential Dumping Tool

    C:\Windows\Temp\procdump64.exe

### Command

    procdump64.exe -accepteula -ma lsass.exe C:\Windows\Temp\lsass.dmp

### Detection

- Sysmon Event ID 10 — Process Access
- Sysmon Event ID 1 — Process Creation

### MITRE ATT&CK

- T1003.001 — LSASS Memory


---

## Phase 3 — Privileged Credential Logon

Following credential dumping activity, the attacker authenticated using the compromised domain account `luke.sullivan`.

The account received elevated privileges required for administrative activity across the environment.

### Observed Account

    luke.sullivan

### Authentication Details

    Source Address:
    10.5.50.12

    Authentication:
    Kerberos

### Evidence

- Event ID 4624 — Successful Logon
- Event ID 4672 — Special Privileges Assigned to New Logon

### MITRE ATT&CK

- T1078 — Valid Accounts


---

## Phase 4 — Active Directory Discovery

After gaining access, the attacker performed Active Directory reconnaissance to identify users, groups, and domain resources.

### Observed Commands

    nltest /domain_trusts

    net user /domain

    net group "Domain Admins" /domain

    Get-ADUser

    Get-ADGroupMember 'Domain Admins'

    Get-ADComputer

### Detection

- Sysmon Event ID 1 — Process Creation
- PowerShell Logs
- Active Directory Events

### MITRE ATT&CK

- T1087 — Account Discovery
- T1069.002 — Permission Groups Discovery
- T1018 — Remote System Discovery


---

## Phase 5 — Lateral Movement: SMB and PsExec

The attacker used Windows administrative shares and PsExec for remote execution across the environment.

### SMB Administrative Share Access

#### Observed SMB Activity

    Share:
    ADMIN$

    Source Address:
    10.5.50.12

    Account:
    luke.sullivan

### Target Systems

    THM-DEV-WS
    THM-SHR-SRV
    THM-SQL-SRV

### Evidence

- Event ID 5140 — Network Share Access
- Event ID 4624 — Authentication Events


### PsExec Remote Execution

The attacker executed PsExec from:

    THM-MKT-WS

### Observed Commands

    C:\Tools\PsExec.exe \\THM-SQL-SRV cmd /c "hostname & whoami & ipconfig"

    C:\Tools\PsExec.exe \\THM-SQL-SRV cmd /c "net localgroup administrators"

### Evidence

- Sysmon Event ID 1 — Process Creation
- Event ID 7045 — Service Installation
- PSEXESVC Service Creation

### MITRE ATT&CK

- T1021.002 — SMB/Windows Admin Shares
- T1569.002 — Service Execution


---

## Phase 6 — Ransomware Preparation

Before deploying ransomware, the attacker attempted to reduce recovery capability and remove forensic evidence.

### Observed Commands

    vssadmin delete shadows /all /quiet

    wevtutil cl Security

### Detection

- Sysmon Event ID 1 — Process Creation
- Security Event ID 4688 — Process Creation

### MITRE ATT&CK

- T1490 — Inhibit System Recovery
- T1070.001 — Clear Windows Event Logs


---

## Phase 7 — WMIC Remote Ransomware Deployment

After lateral movement, the attacker used another compromised privileged account, `maria.garcia`, to remotely deploy the ransomware payload.

The attacker used Windows Management Instrumentation Command-line (WMIC) to execute the ransomware payload (`fixer.exe`) across multiple domain systems.

### Process Execution Chain

    updater.exe
    │
    └── cmd.exe
        │
        └── WMIC.exe
            │
            └── Remote Process Creation
                │
                └── C:\Windows\fixer.exe

### Observed Command

    wmic /node:<target-host> /user=maria.garcia /password:<password> process call create C:\Windows\fixer.exe

### Affected Systems

    tsm-prod-01
    tsm-prod-02
    tsm-prod-03
    tsm-prod-04
    tsm-prod-05
    tsm-prod-06

### Evidence

- Sysmon Event ID 1 — Process Creation
- Sysmon Event ID 11 — File Creation
- Windows Security Event ID 4624 — Successful Logon
- Windows Security Event ID 4672 — Special Privileges Assigned to New Logon

### Payload Artifact

    C:\Windows\fixer.exe

The investigation confirmed ransomware deployment activity across multiple systems. No evidence of successful encryption impact was identified.

### MITRE ATT&CK

- T1047 — Windows Management Instrumentation
- T1078 — Valid Accounts


---

# Investigation Conclusion

The investigation reconstructed attacker activity from initial compromise to ransomware deployment.

The investigation identified:

- IIS web shell execution
- Command execution through CMD
- LSASS credential dumping attempts
- Privileged credential usage (`luke.sullivan` and `maria.garcia`)
- Active Directory reconnaissance
- SMB administrative share access
- PsExec lateral movement
- Ransomware preparation activity
- WMIC-based ransomware deployment across multiple systems

The investigation demonstrates how SOC analysts can correlate IIS logs, Sysmon telemetry, and Windows Security Events to detect ransomware activity before widespread encryption occurs.

