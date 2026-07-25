# SOC Project 5 — Windows Endpoint Incident Response Investigation

## Overview

This project demonstrates a SOC endpoint incident response investigation of a compromised Windows workstation.

The investigation reconstructs the attacker lifecycle from initial access to SYSTEM-level compromise by correlating:

- Sysmon telemetry
- Windows Security Event Logs
- Wireshark network analysis
- Threat intelligence validation
- MITRE ATT&CK mapping

The investigation identified:

- Follina exploitation (CVE-2022-30190)
- PowerShell execution
- Malware delivery
- Startup folder persistence
- Command-and-control communication
- Chisel reverse SOCKS tunneling
- WinRM remote access
- Privilege escalation
- Unauthorized account creation
- Windows service persistence


---

# Tools Used

- Sysmon
- Windows Event Viewer
- Wireshark (network traffic validation)
- VirusTotal (threat intelligence)
- MITRE ATT&CK


---

# Investigation Source

This investigation was based on a TryHackMe Windows incident response scenario.

The investigation was independently analyzed using SOC workflows:

- Endpoint telemetry analysis
- Network traffic investigation
- Timeline reconstruction
- IOC extraction
- MITRE ATT&CK classification


---

# Incident Scenario

A user opened a malicious Microsoft Word document:

```text
free_magicules.doc
```

The document exploited the Follina vulnerability (CVE-2022-30190) through the Microsoft Support Diagnostic Tool (`msdt.exe`) execution chain.


The attacker used PowerShell to download additional payloads, established persistence, performed reconnaissance, obtained credentials from a sensitive file, created a Chisel reverse SOCKS tunnel, accessed the system through WinRM, escalated privileges using PrintSpoofer, created an administrator account, and installed a malicious Windows service for persistent SYSTEM access.


---

# Environment

| Component | Details |
|-----------|---------|
| Analysis Platform | Windows 10 |
| Endpoint Telemetry | Sysmon |
| Windows Logs | Windows Security Event Logs |
| Network Analysis | Wireshark |
| Threat Intelligence | VirusTotal |
| Hostname | TEMPEST |
| Initial Document | `free_magicules.doc` |


---

# Investigation Workflow

```text
Malicious Word Document
        |
        ▼
Endpoint Investigation
(Sysmon + Windows Event Logs)
        |
        ▼
Network Investigation
(Wireshark)
        |
        ▼
Timeline Reconstruction
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

# Attack Chain

```text
Malicious Word Document
        |
        ▼
Follina Exploit
(CVE-2022-30190)
        |
        ▼
PowerShell Execution
        |
        ▼
phishteam.xyz
(Payload Delivery)
        |
        ├───────────────┐
        ▼               ▼
   update.zip       first.exe
        |               |
        ▼               ▼
Startup Folder     resolvecyber.xyz
Persistence        (C2 Commands)
                        |
                        ▼
               Encoded Commands
                        |
                        ▼
             Host Reconnaissance
                        |
                        ▼
             Credential Discovery
                        |
                        ▼
                 ch.exe (Chisel)
                        |
                        ▼
          Reverse SOCKS Tunnel
          167.71.199.191:8080
                        |
                        ▼
              WinRM Remote Access
                 TCP 5985
                        |
                        ▼
          spf.exe + final.exe
                        |
                        ▼
             PrintSpoofer
                        |
                        ▼
             NT AUTHORITY\SYSTEM
                        |
                        ▼
              Create shion Account
                        |
                        ▼
          Administrator Privileges
                        |
                        ▼
          Windows Service Persistence
              TempestUpdate2
```


---

# Project Structure

```text
SOC-Project-5-Windows-Endpoint-Incident-Response
|
├── README.md
|
├── Evidence
│   ├── attack-timeline.md
│   ├── commands.md
│   ├── persistence.md
│   ├── indicators-of-compromise.md
│   └── findings.md
|
├── Sysmon
│   ├── README.md
│   ├── process-events.md
│   ├── network-events.md
│   ├── file-events.md
│   └── dns-events.md
|
├── Windows-Event-Logs
│   ├── README.md
│   ├── security-events.md
│   └── account-events.md
|
├── Wireshark
│   ├── README.md
│   └── network-analysis.md
|
├── Screenshots
│   ├── 01_follina_process_chain.png
│   ├── 02_powershell_execution.png
│   ├── 03_startup_folder_persistence.png
│   ├── 04_sysmon_file_creation.png
│   ├── 05_sysmon_network_connection.png
│   ├── 06_dns_resolution.png
│   ├── 07_chisel_tunnel_connection.png
│   ├── 08_winrm_wsmprovhost_execution.png
│   ├── 09_printspoofer_execution.png
│   ├── 10_system_privilege_confirmation.png
│   ├── 11_user_account_creation.png
│   ├── 12_administrator_group_addition.png
│   ├── 13_windows_service_creation.png
│   ├── 14_virustotal_analysis.png
│   └── 15_attack_timeline.png
|
├── Incident-Report
│   ├── incident-report.md
│   └── incident-summary.md
|
└── MITRE-ATT&CK
    └── attack-mapping.md
```

---

# Evidence Organization

| Directory | Purpose |
|-----------|---------|
| `Sysmon/` | Process execution, file creation, DNS, and network telemetry |
| `Windows-Event-Logs/` | Authentication, account creation, and security events |
| `Wireshark/` | Network traffic and attacker infrastructure analysis |
| `Evidence/` | Timeline, commands, IOCs, and supporting evidence |
| `Incident-Report/` | Final SOC incident documentation |
| `MITRE-ATT&CK/` | ATT&CK technique mapping |


---

# Key Findings

## Initial Access

A malicious Word document exploited:

```text
CVE-2022-30190 (Follina)
```

Execution chain identified through Sysmon:

```text
WINWORD.EXE
      |
      ▼
msdt.exe
      |
      ▼
PowerShell.exe
```


## Malware Delivery

The attacker downloaded additional payloads from:

```text
phishteam.xyz
```

Observed files:

```text
update.zip
first.exe
ch.exe
spf.exe
final.exe
```


## Command and Control

The attacker used:

```text
resolvecyber.xyz
```

for:

- Encoded command delivery
- Remote attacker instructions
- Malware communication


## Network Pivoting

`ch.exe` was identified as Chisel.

The attacker created a reverse SOCKS tunnel:

```text
167.71.199.191:8080
```


## Remote Access

WinRM activity was confirmed through:

```text
wsmprovhost.exe
```

TCP port:

```text
5985
```


## Privilege Escalation

The attacker used:

```text
spf.exe
```

identified as PrintSpoofer to obtain:

```text
NT AUTHORITY\SYSTEM
```


## Persistence

The attacker created:

```text
shion
```

as an administrator account.

A malicious Windows service was installed:

```text
TempestUpdate2
```

Payload:

```text
C:\ProgramData\final.exe
```


---

# Indicators of Compromise

## Domains

### Payload Delivery

```text
phishteam.xyz
```

### Command and Control

```text
resolvecyber.xyz
```


---

## Network Indicators

```text
167.71.199.191:8080
```

Purpose:

```text
Chisel reverse SOCKS tunnel endpoint used for remote access and network pivoting
```
---

## Malicious Files

```text
free_magicules.doc

update.zip

first.exe

ch.exe

spf.exe

final.exe
```


---

## Persistence Artifacts

Startup Folder:

```text
C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

Service:

```text
TempestUpdate2
```

Payload:

```text
C:\ProgramData\final.exe
```


---

# MITRE ATT&CK Mapping

| Technique | ID | Description |
|-----------|----|-------------|
| Exploitation for Client Execution | T1203 | Follina vulnerability exploitation |
| PowerShell | T1059.001 | Malicious PowerShell execution |
| Obfuscated Files or Information | T1027 | Encoded commands |
| Ingress Tool Transfer | T1105 | Payload downloads |
| Startup Folder Persistence | T1547.001 | Automatic execution at login |
| Web Protocols | T1071.001 | HTTP communication |
| Proxy | T1090 | Chisel reverse tunnel |
| Windows Remote Management | T1021.006 | WinRM access |
| Exploitation for Privilege Escalation | T1068 | PrintSpoofer privilege escalation |
| Create Account | T1136.001 | Local administrator creation |
| Windows Service | T1543.003 | Service persistence |
| System Information Discovery | T1082 | Host reconnaissance |
| Account Discovery | T1087 | Credential/account enumeration |


---

# Incident Response Recommendations

## Containment

- Isolate the compromised endpoint.
- Block malicious domains and IP addresses.
- Disable unauthorized accounts.


## Investigation

- Review Sysmon process, file, DNS, and network events.
- Search other endpoints for matching indicators.
- Review authentication logs for additional access.


## Remediation

- Remove malicious files.
- Remove Startup Folder persistence.
- Remove malicious services.
- Reset compromised credentials.
- Patch vulnerable systems against Follina exploitation.


## Detection Improvements

Monitor for:

- Suspicious PowerShell execution
- Encoded PowerShell commands
- Startup folder modifications
- New administrator accounts
- Unauthorized Windows services
- WinRM activity
- Proxy/tunneling tools such as Chisel


---

# Conclusion

This investigation demonstrates a complete SOC incident response workflow from initial compromise detection to attacker persistence.

By correlating Sysmon telemetry, Windows Security Events, and Wireshark network traffic, the investigation identified:

- Initial exploitation
- Malware delivery infrastructure
- C2 communication
- Network tunneling
- Remote access
- Privilege escalation
- Persistence mechanisms

This investigation demonstrates how SOC analysts correlate endpoint telemetry, authentication events, and network evidence to reconstruct attacker activity, identify persistence mechanisms, extract indicators of compromise, and support incident response decisions.
