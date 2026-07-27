# Attack Timeline

This document reconstructs the attacker activity timeline by correlating:

- Sysmon telemetry
- Windows Security Event Logs
- Wireshark network analysis
- Command-line activity

---

# Attack Timeline Overview

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
Payload Delivery
(phishteam.xyz)
        |
        ▼
Startup Persistence
        |
        ▼
first.exe Execution
        |
        ▼
C2 Communication
(resolvecyber.xyz)
        |
        ▼
Host Reconnaissance
        |
        ▼
Credential Discovery
        |
        ▼
Chisel Reverse SOCKS Tunnel
(167.71.199.191:8080)
        |
        ▼
WinRM Remote Access
        |
        ▼
Privilege Escalation
(PrintSpoofer)
        |
        ▼
SYSTEM Access
        |
        ▼
Local Administrator Creation
(shion)
        |
        ▼
Windows Service Persistence
(TempestUpdate2)
```

---

# Phase 1 — Initial Access

The attacker delivered a malicious Microsoft Word document:

```text
free_magicules.doc
```

The document exploited:

```text
CVE-2022-30190
(Follina)
```

Execution chain observed:

```text
WINWORD.EXE
      |
      ▼
msdt.exe
      |
      ▼
PowerShell.exe
```

Evidence:

- Sysmon Event ID 1 — Process Creation
- Parent-child process analysis

---

# Phase 2 — Payload Delivery

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

Evidence:

- Wireshark HTTP analysis
- Sysmon Event ID 3 — Network Connection
- Sysmon Event ID 11 — File Creation

---

# Phase 3 — Startup Persistence

The attacker placed a malicious payload in:

```text
C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

The payload executed automatically after user logon.
```text
Startup Folder
        |
        ▼
explorer.exe
        |
        ▼
Malicious Payload
```

Evidence:

- Sysmon Event ID 11 — File Creation
- Sysmon Event ID 1 — Process Creation

---

# Phase 4 — Command and Control

The first-stage malware:

```text
first.exe
```

established communication with:

```text
resolvecyber.xyz
```

The domain was used as attacker command-and-control infrastructure.

Observed activity:

- HTTP-based communication
- Remote command retrieval
- Encoded attacker instructions
- Malware task execution

Communication flow:

```text
first.exe
      |
      ▼
resolvecyber.xyz
      |
      ▼
Base64 encoded commands
```

Evidence:

- Sysmon Event ID 3
- Sysmon Event ID 22 — DNS Query
- Wireshark HTTP analysis
- CyberChef command decoding

---

# Phase 5 — Host Reconnaissance

The attacker executed discovery commands to collect information about the endpoint.

Observed activity:

- System information discovery
- Network information discovery
- User enumeration

Evidence:

- Sysmon Event ID 1
- Command-line analysis

---

# Phase 6 — Credential Discovery

The attacker discovered credentials stored in a sensitive file.

Recovered account:

```text
infernotempest
```

These credentials were later used for remote authentication.

Evidence:

- Command execution timeline
- File analysis
- Authentication activity correlation

---

# Phase 7 — Chisel Network Pivot

The attacker downloaded:

```text
ch.exe
```

identified as:

```text
Chisel
```

The attacker created a reverse SOCKS tunnel:

```text
167.71.199.191:8080
```

Purpose:

- Remote access
- Network pivoting
- Proxy communication

Evidence:

- Sysmon Event ID 1
- Sysmon Event ID 3
- Wireshark analysis

---

# Phase 8 — WinRM Remote Access

Using discovered credentials, the attacker accessed the system through:

```text
Windows Remote Management
```

Observed process:

```text
wsmprovhost.exe
```

Evidence:

- Sysmon Event ID 1
- Sysmon Event ID 3
- Authentication logs

---

# Phase 9 — Privilege Escalation

The attacker executed:

```text
spf.exe
```

identified as:

```text
PrintSpoofer
```

Result:

```text
NT AUTHORITY\SYSTEM
```

Evidence:

- Sysmon Event ID 1
- Command-line analysis
- VirusTotal analysis

---

# Phase 10 — Account Creation

The attacker created:

```text
shion
```

and added it to the administrators group.

Evidence:

- Windows Security Event ID 4720
- Windows Security Event ID 4732

---

# Phase 11 — Service Persistence

The attacker created:

```text
TempestUpdate2
```

Service payload:

```text
C:\ProgramData\final.exe
```

Evidence:

- Sysmon Event ID 1
- Service creation activity
