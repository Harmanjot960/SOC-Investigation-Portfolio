# SOC Project 4 — Network Threat Hunting Investigation (Wireshark + Suricata)

## Overview

This project demonstrates a Security Operations Center (SOC) investigation of a malware infection using network traffic analysis.

A user reported downloading a suspicious application disguised as a legitimate Google Authenticator-related file. After execution, the compromised workstation began communicating with attacker-controlled infrastructure.

A packet capture (PCAP) containing the associated network activity was analyzed to identify:

- Malware download activity
- Malicious network communications
- Command-and-control (C2) infrastructure
- PowerShell-based payload delivery
- Suspicious executable downloads
- Indicators of Compromise (IOCs)

## Tools Used

- Wireshark
- Suricata IDS
- VirusTotal
- MITRE ATT&CK

---

# Incident Scenario

A user searched for Google Authenticator and downloaded a malicious application disguised as Google Authenticator software.
After execution, the infected workstation communicated with external attacker-controlled infrastructure.

The investigation focused on:

- Network conversations
- Suspicious IP addresses
- DNS activity
- HTTP traffic
- TLS certificates
- Malware downloads
- PowerShell payload delivery
- Command-and-control activity

---

# Environment

| Component | Details |
|-----------|---------|
| Analysis Platform | Kali Linux |
| Network Analysis Tool | Wireshark |
| IDS Tool | Suricata |
| Threat Intelligence | VirusTotal |
| PCAP File | 2025-01-22-traffic-analysis-exercise.pcap |
| Victim Host | 10.1.17.215 |
| Domain Controller | 10.1.17.2 |
| Network Range | 10.1.17.0/24 |
| AD Environment | BLUEMOONTUESDAY |

---

# Lab Architecture

The investigation used a PCAP-based analysis workflow. The captured network traffic was analyzed using Wireshark and Suricata to identify malicious activity.

```text
                 PCAP File
                    |
        ---------------------------
        |                         |
        ▼                         ▼

    Wireshark                 Suricata
 Packet Analysis          IDS Detection

        |                         |
        ▼                         ▼

 Network Evidence        Security Alerts

        \                    /
         \                  /

          Threat Investigation

                 |
                 ▼

      IOC Extraction
      Timeline Analysis
      MITRE ATT&CK Mapping
      Incident Report
```
---

# Investigation Workflow

```text
PCAP
 |
 ▼
Traffic Analysis
 |
 ▼
Threat Hunting
 |
 ▼
Protocol Investigation
(DNS / HTTP / TLS)
 |
 ▼
IDS Detection
(Suricata)
 |
 ▼
IOC Validation
 |
 ▼
Timeline Reconstruction
 |
 ▼
Incident Report
```
---

# Project Structure
```text
SOC-Project-4-Network-Threat-Hunting
│
├── README.md
│
├── PCAP
│   └── 2025-01-22-traffic-analysis-exercise.pcap
│
├── Suricata
│   ├── eve.json
│   ├── fast.log
│   ├── stats.log
│   └── suricata.log
│
├── Evidence
│   ├── timeline.md
│   ├── iocs.md
│   ├── network-analysis.md
│   └── suricata-alerts.md
│
├── Screenshots
│   ├── 01_wireshark_protocol_hierarchy.png
│   ├── 02_wireshark_endpoints.png
│   ├── 03_wireshark_conversations.png
│   ├── 04_dns_analysis.png
│   ├── 05_http_stream_powershell_payload.png
│   ├── 06_tls_self_signed_certificate.png
│   ├── 07_suricata_fast_log_alerts.png
│   ├── 08_malicious_ip_reputation.png
│   ├── 09_attack_timeline.png
│   └── 10_incident_summary.png
│
├── MITRE-ATT&CK
│   └── attack_mapping.md
│
└── Incident-Report
    ├── incident_report.md
    └── incident-summary.md
```

---

# Network Investigation

Wireshark was used to analyze the packet capture and identify malicious network activity.

The investigation included:

- Protocol hierarchy analysis
- Endpoint identification
- Conversation analysis
- DNS inspection
- HTTP stream analysis
- TLS certificate inspection

The compromised workstation:

```text
10.1.17.215
```

was observed communicating with multiple suspicious external systems.

---

# Malicious Infrastructure Identified

## IP Address: 5.252.153.241

This IP address was identified as the primary malware delivery and command-and-control (C2) infrastructure.

Suricata detected:

```text
ET MALWARE Fake Microsoft Teams CnC Payload Request (GET)
```

Additional Suricata detections:

```text
ET INFO PS1 Powershell File Request

ET HUNTING Generic Powershell DownloadString Command

ET HUNTING Generic Powershell DownloadFile Command

ET INFO PE EXE or DLL Windows file download
```

These alerts indicate:

- Malware C2 communication
- PowerShell script delivery
- Additional payload retrieval
- Second-stage malware downloads

---

# PowerShell Malware Delivery

Network analysis identified PowerShell-based malware activity.

Attack flow:

Victim Host
      │
      ▼
HTTP Request to Malicious Server
      │
      ▼
PowerShell Script Download
      │
      ▼
Additional Payload Retrieval

The downloaded PowerShell content contained the following suspicious behaviors:

- Invoke-Expression (IEX)
- FromBase64String()
- DownloadString()
- DownloadFile()

These techniques are commonly associated with malware loaders, payload execution, obfuscation, and defense evasion.

---

# Fake TeamViewer Payload

The following files were downloaded during the attack:

```text
TeamViewer.exe
TV.dll
Teamviewer_Resource_fr.dll
pas.ps1
```

The downloaded files used legitimate software names to disguise malicious activity.

Attack chain:

```text
PowerShell Downloader

        |
        ▼

TeamViewer Components Downloaded

        |
        ▼

Additional Payload Execution

        |
        ▼

Persistence / Remote Access
```

---

# TLS Investigation

A suspicious TLS connection was identified during analysis.

Certificate details:

```text
Common Name:
Self-signed certificate

Certificate Identity:
45.125.66.32: Self-signed certificate
```

Observed characteristics:

- Self-signed certificate
- Certificate identity matched an IP address
- No legitimate organization information

Self-signed certificates are commonly observed in malicious infrastructure, although they may also be used in legitimate environments.

Because the traffic was encrypted, payload contents could not be directly inspected.

The investigation relied on:

- TLS certificate metadata
- IP reputation
- Network behavior
- Related HTTP activity
- Suricata detections

---

# Suricata Analysis

Suricata was executed against the PCAP:

```bash
suricata \
-r PCAP/2025-01-22-traffic-analysis-exercise.pcap \
-l Suricata/
```

Generated files:

```text
eve.json
fast.log
stats.log
suricata.log
```

---

# Suricata Alerts

The primary alert source analyzed:

```text
fast.log
```

## Malware Delivery

```text
ET MALWARE Fake Microsoft Teams VBS Payload Inbound
```

Observed Behavior:

- Initial malicious payload delivery

---

## Command-and-Control Communication

```text
ET MALWARE Fake Microsoft Teams CnC Payload Request (GET)
```

Observed Behavior:

- Communication with malware infrastructure

---

## PowerShell Activity

```text
ET INFO PS1 Powershell File Request

ET HUNTING Generic Powershell DownloadFile Command

ET HUNTING Generic Powershell DownloadString Command
```

Observed Behavior:

- PowerShell script delivery
- Additional payload retrieval

---

## Executable Download

```text
ET INFO PE EXE or DLL Windows file download
```

Observed Behavior:

- Windows executable or DLL retrieval

---

# Attack Timeline

```text
14:44:56
Malicious payload delivered

        |
        ▼

14:45:56
Communication with malicious server

        |
        ▼

14:45:58
PowerShell script downloaded

        |
        ▼

14:47:01
Additional payload retrieval

        |
        ▼

14:47:02
Executable/DLL downloaded

        |
        ▼

14:55:08
TeamViewer-related communication observed
```

---

# Indicators of Compromise (IOCs)

## Malicious IP Addresses

```text
5.252.153.241
```

Observed Activity:

- Malware C2 communication
- Payload delivery infrastructure


```text
82.221.136.26
```

Observed Activity:

- Associated with suspicious binary hosting
- Historical malicious associations observed through threat intelligence
- Potential payload hosting infrastructure

```text
45.125.66.32
```

Observed Activity:

- Suspicious TLS communication
- Self-signed certificate observed


## Suspicious Domain

```text
authenticatoor.org
```

Reason:

- Typosquatting domain impersonating **Google Authenticator**
- Associated with fake authentication software
- Potential malware delivery infrastructure

The domain name aligns with the reported user activity and was used to deliver the malicious application.

---

# MITRE ATT&CK Mapping

| Activity | Technique | ID |
|---|---|---|
| Malicious File Execution | User Execution | T1204 |
| PowerShell Execution | Command and Scripting Interpreter: PowerShell | T1059.001 |
| Payload Download | Ingress Tool Transfer | T1105 |
| PowerShell Obfuscation | Obfuscated Files or Information | T1027 |
| HTTP C2 Communication | Application Layer Protocol: Web Protocols | T1071.001 |
| Startup Persistence | Boot or Logon Autostart Execution | T1547.001 |

---

# Key Findings

The investigation confirmed:

- Malware infection on host `10.1.17.215`
- Communication with malicious infrastructure
- PowerShell-based payload delivery
- Multi-stage malware downloads
- Fake TeamViewer deployment
- Suspicious TLS communication
- Command-and-control activity

---

# Screenshots

## Wireshark Protocol Hierarchy

[View Screenshot](Screenshots/01_wireshark_protocol_hierarchy.png)


## Wireshark Endpoints Analysis

[View Screenshot](Screenshots/02_wireshark_endpoints.png)


## Wireshark Conversations

[View Screenshot](Screenshots/03_wireshark_conversations.png)


## DNS Investigation

[View Screenshot](Screenshots/04_dns_analysis.png)


## HTTP Stream Analysis

[View Screenshot](Screenshots/05_http_stream_powershell_payload.png)


## TLS Certificate Analysis

[View Screenshot](Screenshots/06_tls_self_signed_certificate.png)


## Suricata IDS Alerts

[View Screenshot](Screenshots/07_suricata_fast_log_alerts.png)


## VirusTotal Reputation Analysis

[View Screenshot](Screenshots/08_malicious_ip_reputation.png)


## Attack Timeline

[View Screenshot](Screenshots/09_attack_timeline.png)


## Incident Summary

[View Screenshot](Screenshots/10_incident_summary.png)

---

# Conclusion

This project demonstrates a SOC analyst workflow for investigating malware infections using network telemetry.

Wireshark provided packet-level visibility, while Suricata detected malicious network behavior and generated IDS alerts.

The investigation reconstructed the following attack sequence:

```text
Malicious Download

        |
        ▼

C2 Communication

        |
        ▼

PowerShell Payload Delivery

        |
        ▼

Additional Malware Download

        |
        ▼

Persistence / Remote Access
```

The collected evidence was analyzed, documented, and mapped to MITRE ATT&CK techniques as part of an incident response investigation.
