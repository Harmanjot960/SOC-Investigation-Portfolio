# MITRE ATT&CK Mapping

## Overview

This document maps the observed attacker activity to MITRE ATT&CK techniques identified during the investigation.

The mapping was based on evidence collected from:

- Sysmon telemetry
- Windows Security Event Logs
- Wireshark network analysis
- Command-line activity
- Malware behavior analysis

---

## Attack Technique Mapping

| Technique | ID | Evidence |
|-----------|----|----------|
| Exploitation for Client Execution | T1203 | Follina vulnerability exploitation through malicious Word document |
| PowerShell | T1059.001 | Attacker executed PowerShell commands and payload retrieval |
| Obfuscated Files or Information | T1027 | Base64 encoded attacker commands retrieved from C2 server |
| Ingress Tool Transfer | T1105 | Download of update.zip, first.exe, ch.exe, spf.exe, and final.exe |
| Web Protocols | T1071.001 | HTTP communication with attacker infrastructure |
| System Information Discovery | T1082 | Host reconnaissance commands executed after compromise |
| System Network Configuration Discovery | T1016 | Network discovery activity during reconnaissance |
| Credentials from Password Stores / Files | T1552.001 | Credentials discovered from sensitive file |
| Proxy | T1090 | Chisel reverse SOCKS tunnel creation |
| Windows Remote Management | T1021.006 | Remote PowerShell session through WinRM TCP 5985 |
| Exploitation for Privilege Escalation | T1068 | PrintSpoofer used to obtain SYSTEM privileges |
| Create Account: Local Account | T1136.001 | Creation of local user account `shion` |
| Account Manipulation | T1098.007 | Added `shion` account to the local Administrators group |
| Create or Modify System Process: Windows Service | T1543.003 | Malicious service `TempestUpdate2` created for persistence |
| Startup Folder Persistence | T1547.001 | Malware placed in Windows Startup folder |

---

## Attack Lifecycle Mapping

```text
Initial Access
      |
      ▼
T1203
Exploitation for Client Execution
      |
      ▼
T1059.001
PowerShell Execution
      |
      ▼
T1105
Payload Download
      |
      ▼
T1547.001
Startup Folder Persistence
      |
      ▼
T1071.001
C2 Communication
      |
      ▼
T1082 / T1016
System Discovery
      |
      ▼
T1552.001
Credential Discovery
      |
      ▼
T1090
Chisel Proxy Tunnel
      |
      ▼
T1021.006
WinRM Remote Access
      |
      ▼
T1068
Privilege Escalation
      |
      ▼
T1136.001 + T1098
Account Creation and Privilege Assignment
      |
      ▼
T1543.003
Windows Service Persistence
```

---

## Key ATT&CK Findings

### Initial Access

The attacker gained access through a malicious Microsoft Word document exploiting:

```text
CVE-2022-30190 (Follina)
```

Mapped technique:

```text
T1203 - Exploitation for Client Execution
```

---

### Command and Control

The malware communicated with attacker-controlled infrastructure:

```text
resolvecyber.xyz
```

using HTTP requests containing encoded commands.

Mapped techniques:

```text
T1071.001 - Web Protocols

T1027 - Obfuscated Files or Information
```

---

### Privilege Escalation

The attacker executed:

```text
spf.exe
```

identified as PrintSpoofer, resulting in:

```text
NT AUTHORITY\SYSTEM
```

Mapped technique:

```text
T1068 - Exploitation for Privilege Escalation
```

---

### Persistence

The attacker established persistence through:

```text
Startup Folder
+
Windows Service
+
Administrative Account
```

Mapped techniques:

```text
T1547.001 - Startup Folder

T1543.003 - Windows Service

T1136.001 - Local Account Creation
```

---

## Investigation Conclusion

The MITRE ATT&CK mapping demonstrates a complete attacker lifecycle:

```text
Exploit
  |
  ▼
Execute
  |
  ▼
Download Payloads
  |
  ▼
Establish C2
  |
  ▼
Discover Information
  |
  ▼
Pivot Through Chisel
  |
  ▼
Access Remotely
  |
  ▼
Escalate Privileges
  |
  ▼
Maintain Persistence
```

The investigation successfully mapped observed attacker behavior to multiple MITRE ATT&CK techniques using correlated endpoint and network evidence.
