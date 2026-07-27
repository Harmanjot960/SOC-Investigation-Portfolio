# Sysmon Event ID 3 — Network Connections

Sysmon Event ID 3 was used to identify attacker communication, malware infrastructure, and remote access activity.

---

## Malware Delivery Communication

The compromised endpoint established network connections to attacker-controlled infrastructure associated with:

```text
phishteam.xyz
```
Observed activity:

- HTTP payload delivery
- Malware download requests
- Tool transfer

Purpose:

```text
Payload delivery
```

Observed downloads:

```text
update.zip
first.exe
ch.exe
spf.exe
final.exe
```

---

## Command and Control Communication

The malware:

```text
first.exe
```

communicated with:

```text
resolvecyber.xyz
```

Purpose:

- Retrieve encoded commands
- Deliver attacker instructions
- Maintain malware communication

Communication flow:

```text
first.exe
      |
      ▼
resolvecyber.xyz
(DNS Resolution)
      |
      ▼
HTTP C2 Communication
      |
      ▼
Encoded Commands
```

MITRE ATT&CK:

```text
T1071.001 - Web Protocols
```

---

## Chisel Reverse SOCKS Tunnel

The attacker used:

```text
ch.exe
```

to create a reverse SOCKS tunnel.

Observed connection:

```text
167.71.199.191:8080
```

Purpose:

- Proxy communication
- Network pivoting
- Remote access

MITRE ATT&CK:

```text
T1090 - Proxy
```

---

## WinRM Activity

The attacker used:

```text
Windows Remote Management
```

Evidence:

- Network connection telemetry
- wsmprovhost.exe execution
- Authentication activity

MITRE ATT&CK:

```text
T1021.006 - Windows Remote Management
```

---

## Evidence Correlation

Network telemetry was correlated with:

- Sysmon Event ID 1
- Sysmon Event ID 22
- Wireshark traffic analysis
