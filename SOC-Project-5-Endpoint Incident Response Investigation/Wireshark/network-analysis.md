# Wireshark Network Analysis

## Overview

Wireshark was used to analyze captured network traffic and reconstruct attacker communication during the incident.

The analysis identified malware delivery, command-and-control communication, tunneling activity, and remote access behavior.

---

## Initial Malware Delivery

The compromised endpoint communicated with:

```text
phishteam.xyz
```

Protocol:

```text
HTTP

Port 80
```

Observed requests:

```text
http://phishteam.xyz/02dcf07/

http://phishteam.xyz/02dcf07/index.html

http://phishteam.xyz/02dcf07/update.zip

http://phishteam.xyz/02dcf07/first.exe
```

The attacker used this infrastructure to deliver the initial malware payloads.

MITRE ATT&CK:

```text
T1105 - Ingress Tool Transfer
```

---

## Payload Downloads

Additional attacker tools were downloaded from:

```text
phishteam.xyz
```

Observed files:

```text
ch.exe

spf.exe

final.exe
```

Download activity was correlated with:

```text
HTTP Traffic
        +
Sysmon File Creation Events
        +
Process Execution Events
```

---

## Command and Control Communication

After execution of:

```text
first.exe
```

the malware contacted:

```text
resolvecyber.xyz
```

Protocol:

```text
HTTP

Port 80
```

Observed endpoint:

```text
/9ab62b5
```

The malware retrieved attacker commands through Base64 encoded parameters:

```text
?q=<encoded command>
```

### Command Decoding Analysis

The C2 server delivered Base64 encoded commands through HTTP requests.

The encoded parameters were extracted from Wireshark HTTP traffic and decoded using CyberChef.

Decoding process:

```text
Wireshark HTTP Request
        |
        ▼
Extract Base64 Parameter
        |
        ▼
CyberChef Base64 Decode
        |
        ▼
Decoded Attacker Command
```

Decoded commands revealed attacker activity including:
```text
pwd

system information discovery

command execution
```

Evidence:

- Wireshark HTTP traffic capture
- CyberChef Base64 decoding
- C2 communication analysis

MITRE ATT&CK:

```text
T1071.001 - Web Protocols
```

---

## Chisel Reverse SOCKS Tunnel

The attacker downloaded:

```text
ch.exe
```

The binary was identified as:

```text
Chisel
```

Network communication:

```text
Chisel Reverse Tunnel

Compromised Host
        |
        ▼
ch.exe
        |
        ▼
167.71.199.191:8080
        |
        ▼
Attacker Proxy Infrastructure
```

Purpose:

- Create reverse SOCKS tunnel
- Provide proxy access
- Enable network pivoting

MITRE ATT&CK:

```text
T1090 - Proxy
```

---

## WinRM Remote Access

During the post-compromise phase, the attacker accessed the endpoint through:

```text
Windows Remote Management

TCP 5985
```

Observed activity:

```text
wsmprovhost.exe
```

This confirmed remote PowerShell execution.

MITRE ATT&CK:

```text
T1021.006 - Windows Remote Management
```

---

## Attacker Infrastructure Summary

| Indicator | Role |
|-----------|------|
| phishteam.xyz | Malware payload delivery |
| resolvecyber.xyz | Command-and-control server |
| 167.71.199.191:8080 | Chisel reverse SOCKS tunnel endpoint |

---

## Investigation Conclusion

Wireshark analysis confirmed the complete network attack flow:

```text
Malicious Document
        |
        ▼
Malware Download
(phishteam.xyz)
        |
        ▼
C2 Communication
(resolvecyber.xyz)
        |
        ▼
Chisel Tunnel
(167.71.199.191:8080)
        |
        ▼
WinRM Remote Access
```

Network evidence was correlated with endpoint telemetry to reconstruct the attacker lifecycle.
