# Incident Report — Windows Endpoint Compromise Investigation

## Incident Title

Windows Endpoint Incident Response Investigation Following Follina Exploitation

---

## Executive Summary

A Security Operations Center (SOC) investigation was initiated after suspicious activity was identified on a Windows workstation.

Analysis of endpoint telemetry and network traffic confirmed that the workstation had been compromised through a malicious Microsoft Word document exploiting the Follina vulnerability (CVE-2022-30190).

Following successful exploitation, the attacker executed PowerShell commands to retrieve multiple payloads, established persistence, communicated with external infrastructure, performed host reconnaissance, discovered credentials, created a Chisel reverse SOCKS tunnel, authenticated through WinRM, escalated privileges using PrintSpoofer, created a local administrator account, and installed a malicious Windows service.

The investigation reconstructed the complete attacker lifecycle by correlating Sysmon telemetry, Windows Security Event Logs, and Wireshark network traffic.

The incident was classified as a confirmed Windows endpoint compromise with successful privilege escalation and persistence.

---

## Incident Information

| Field | Details |
|-------|---------|
| Incident Type | Windows Endpoint Compromise |
| Investigation Type | Endpoint Incident Response |
| Severity | Critical |
| Status | Confirmed |
| Affected Host | TEMPEST |
| Initial Vector | Malicious Microsoft Word Document |
| Exploit | Follina (CVE-2022-30190) |
| Evidence Sources | Sysmon, Windows Security Event Logs, Wireshark |
| Analysis Tools | Sysmon, Event Viewer, Wireshark, VirusTotal |
| Investigation Focus | Endpoint Telemetry and Network Correlation |

---

## Investigation Scope

The objective of this investigation was to reconstruct the complete attacker lifecycle and determine how the compromise occurred.

The investigation focused on:

- Initial access
- Process execution
- Malware delivery
- File creation
- DNS activity
- Network communication
- Command-and-control activity
- Credential discovery
- Remote access
- Privilege escalation
- Persistence mechanisms
- Indicators of Compromise (IOCs)

---

## Environment Information

| Field | Details |
|-------|---------|
| Operating System | Windows 10 |
| Hostname | TEMPEST |
| Endpoint Telemetry | Sysmon |
| Windows Logs | Security Event Logs |
| Network Analysis | Wireshark |
| Threat Intelligence | VirusTotal |
| Initial Document | `free_magicules.doc` |

---

## Initial Assessment

Initial analysis identified execution of a malicious Microsoft Word document that triggered the Microsoft Support Diagnostic Tool (`msdt.exe`) through the Follina vulnerability.

Sysmon telemetry confirmed the following execution chain:

```text
WINWORD.EXE
      │
      ▼
msdt.exe
      │
      ▼
PowerShell.exe
```

Subsequent investigation identified additional payload downloads, command-and-control communication, privilege escalation, and multiple persistence mechanisms, indicating a multi-stage endpoint compromise.

The investigation then proceeded to reconstruct each stage of the attack using correlated endpoint and network evidence.

---

## Investigation Timeline

### Phase 1 — Initial Access

The malicious document executed the following process chain:

```text
WINWORD.EXE
      |
      ▼
msdt.exe
      |
      ▼
PowerShell.exe
```

MITRE ATT&CK

```text
T1203 - Exploitation for Client Execution
```

---

### Phase 2 — Malware Delivery

PowerShell contacted the attacker infrastructure:

```text
phishteam.xyz
```

Observed downloads:

```text
update.zip

first.exe

ch.exe

spf.exe

final.exe
```

MITRE ATT&CK

```text
T1105 - Ingress Tool Transfer
```

---

### Phase 3 — Persistence

The attacker copied malware into the Windows Startup folder to achieve persistence.

Location:

```text
C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

MITRE ATT&CK

```text
T1547.001 - Startup Folder
```

---

### Phase 4 — Command and Control

After execution, the malware communicated with:

```text
resolvecyber.xyz
```

Purpose:

- Receive encoded commands
- Execute attacker instructions
- Continue post-compromise activity

MITRE ATT&CK

```text
T1071.001 - Web Protocols
```

---

### Phase 5 — Host Reconnaissance

Encoded commands performed host discovery including:

- Working directory enumeration
- System information discovery
- Network discovery
- User enumeration

MITRE ATT&CK

```text
T1082 - System Information Discovery

T1016 - System Network Configuration Discovery

T1087 - Account Discovery

T1033 - System Owner/User Discovery
```

---

### Phase 6 — Credential Discovery

The attacker discovered credentials stored in a sensitive file.

Recovered account:

```text
infernotempest
```

These credentials were later used for remote authentication.

MITRE ATT&CK

```text
T1552.001 - Credentials in Files
```

---

### Phase 7 — Network Pivoting

The attacker downloaded:

```text
ch.exe
```

identified as Chisel.

A reverse SOCKS tunnel was established:

```text
167.71.199.191:8080
```

MITRE ATT&CK

```text
T1090 - Proxy
```

---

### Phase 8 — Remote Access

Using the discovered credentials and the established tunnel, the attacker authenticated through:

```text
WinRM

TCP 5985
```

Evidence:

```text
wsmprovhost.exe
```

MITRE ATT&CK

```text
T1021.006 - Windows Remote Management
```

---

### Phase 9 — Privilege Escalation

The attacker executed:

```text
spf.exe
```

identified as PrintSpoofer.

Command:

```text
spf.exe -c C:\ProgramData\final.exe
```

Result:

```text
NT AUTHORITY\SYSTEM
```

MITRE ATT&CK

```text
T1068 - Exploitation for Privilege Escalation
```

---

### Phase 10 — Payload Investigation

Timeline correlation confirmed that:

```text
final.exe
```

was **not** extracted from:

```text
update.zip
```

Instead, it was downloaded later during the WinRM session before being executed with SYSTEM privileges.

This conclusion was confirmed by correlating:

- Sysmon Event ID 1
- Sysmon Event ID 11
- Wireshark HTTP traffic
- Timeline analysis

---

### Phase 11 — Administrative Persistence

The attacker created:

```text
shion
```

using:

```text
net user shion <password> /add
```

The account was then added to the local Administrators group.

Evidence:

- Event ID 4720
- Event ID 4732

MITRE ATT&CK

```text
T1136.001 - Create Account: Local Account

T1098.007 - Additional Local or Domain Groups
```

---

### Phase 12 — Service Persistence

The attacker created the Windows service:

```text
TempestUpdate2
```

Command:

```text
sc.exe create TempestUpdate2 binpath= C:\ProgramData\final.exe start= auto
```

MITRE ATT&CK

```text
T1543.003 - Create or Modify System Process: Windows Service
```

---

## Investigation Conclusion

The investigation reconstructed the complete attacker lifecycle from initial compromise to persistent SYSTEM-level access.

Correlation of endpoint telemetry and network traffic identified:

- Initial exploitation
- Malware delivery
- Command-and-control communication
- Credential discovery
- Network pivoting
- Remote access
- Privilege escalation
- Administrative account persistence
- Windows service persistence

The investigation demonstrates how multiple evidence sources can be combined to accurately reconstruct attacker activity during a Windows endpoint incident response investigation.
