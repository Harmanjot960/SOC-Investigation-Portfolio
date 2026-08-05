# SOC Project 4 — Network Threat Hunting Investigation (Wireshark + Suricata)

## Overview

This project demonstrates a Security Operations Center (SOC) investigation of a malware infection using network traffic analysis.

A user searched for Google Authenticator and downloaded a malicious application masquerading as legitimate Google Authenticator software. After execution, the compromised workstation began communicating with attacker-controlled infrastructure.

A packet capture (PCAP) containing the associated network activity was analyzed using Wireshark and Suricata to identify malicious activity, attacker infrastructure, and indicators of compromise (IOCs).

---

## Incident Scenario

After execution of the malicious Google Authenticator application, the infected workstation generated suspicious network activity.

The SOC analyst investigated:

- Malicious network communications
- DNS and HTTP activity
- Suspicious external infrastructure
- Malware and executable downloads
- PowerShell payload delivery
- TLS certificate information
- Indicators of Compromise (IOCs)

---

## Environment

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

## Investigation Source

This investigation was performed in a personal SOC Home Lab using a packet capture (PCAP) from a simulated malware infection.

Wireshark, Suricata, and VirusTotal were used to analyze network traffic, identify malicious infrastructure, validate indicators of compromise, and reconstruct the attack timeline.

**PCAP Source:**

2025-01-22-traffic-analysis-exercise.pcap  
Malware Traffic Analysis — https://www.malware-traffic-analysis.net/

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Wireshark | Packet analysis |
| Suricata | IDS alert generation |
| VirusTotal | Threat intelligence |
| Kali Linux | Analysis platform |
| MITRE ATT&CK | Technique mapping |

---

## Lab Architecture

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

## Investigation Workflow

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

## Project Structure
```text
SOC-Project-4-Network-Threat-Hunting
│
├── README.md
│
├── PCAP
│   └── README.md
│
├── Suricata
│   ├── README.md
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
│   ├── 05_tls_sni_analysis.png 
│   ├── 06_http_stream_malware_download.png
│   ├── 07_http_stream_powershell_payload.png
│   ├── 08_tls_self_signed_certificate.png 
│   ├── 09_suricata_fast_log_alerts.png
│   ├── 10_malicious_ip_reputation.png
│   ├── 11_attack_timeline.png
│   └── 12_incident_summary.png
│
├── MITRE-ATT&CK
│   └── attack_mapping.md
│
└── Incident-Report
    └── incident_report.md
```

---

## Network Investigation

Wireshark and Suricata were used to analyze the packet capture and identify malicious network activity.

The compromised workstation:

```text
10.1.17.215
```

was observed communicating with suspicious external infrastructure.

The investigation included:

- Protocol hierarchy analysis
- Endpoint identification
- Conversation analysis
- DNS inspection
- HTTP stream analysis
- TLS certificate inspection
- Suricata alert correlation

---

### Malicious Infrastructure

The compromised host communicated with:

```text
5.252.153.241
```

Suricata generated alerts associated with malware delivery, PowerShell activity, and suspicious external communication.

**Detected alerts:**

```text
ET MALWARE Fake Microsoft Teams CnC Payload Request (GET)
ET INFO PS1 Powershell File Request
ET HUNTING Generic Powershell DownloadString Command
ET HUNTING Generic Powershell DownloadFile Command
ET INFO PE EXE or DLL Windows file download
```

These detections indicate:

* Malware payload delivery
* PowerShell-based download activity
* Additional payload retrieval
* Possible C2 communication

---

## Attack Chain

Network analysis identified the following malware delivery sequence:

```text
Victim Host
    |
    ▼
Malicious Google Authenticator Application Download
    |
    ▼
PowerShell Payload Delivery
    |
    ▼
Additional Malware Retrieval
    |
    ▼
Fake TeamViewer Components Downloaded
    |
    ▼
Startup Folder Persistence Established
    |
    ▼
C2 Communication
```

The downloaded PowerShell content contained suspicious functions:

```text
Invoke-Expression
FromBase64String()
DownloadString()
DownloadFile()
```

These behaviors are commonly associated with malware execution, payload delivery, obfuscation, and defense evasion.

---

## Downloaded Malware Artifacts

The investigation identified the following downloaded files:

```text
TeamViewer.exe
TV.dll
Teamviewer_Resource_fr.dll
pas.ps1
```

The attacker used legitimate software names to disguise malicious activity.

The PowerShell script created a Startup folder shortcut:

```text
TeamViewer.lnk
```

which executed:

```text
C:\ProgramData\huo\TeamViewer.exe
```

This behavior indicates persistence through automatic execution during user logon.

---

## TLS Investigation

Encrypted TLS communication was identified during the investigation.

Certificate details:

```text
Certificate Type:
Self-signed certificate

Certificate Identity:
45.125.66.32
```

**Observed characteristics:**

* Self-signed certificate
* Identity matched an IP address
* No trusted certificate authority validation was observed.
  
Due to encryption, payload contents could not be directly inspected. TLS metadata, network behavior, and Suricata detections were used for correlation.

---

## Suricata Analysis

Suricata was executed against the PCAP:

```bash
suricata \
-r PCAP/2025-01-22-traffic-analysis-exercise.pcap \
-l Suricata/
```

Generated Suricata logs:

- [eve.json](Suricata/eve.json)
- [fast.log](Suricata/fast.log)
- [stats.log](Suricata/stats.log)
- [suricata.log](Suricata/suricata.log)


The primary alert source analyzed:

```text
fast.log
```

**Observed Suricata detections included:**

- Fake Microsoft Teams malware communication
- PowerShell script download activity
- DownloadString / DownloadFile behavior
- Executable and DLL file retrieval

These findings correlated with Wireshark analysis and confirmed malware delivery activity and suspicious external communication.


---

## Indicators of Compromise (IOCs)

### Malicious IP Addresses

```text
5.252.153.241
```

**Observed Activity:**

- Malware C2 communication
- Payload delivery infrastructure


```text
82.221.136.26
```

**Observed Activity:**

- Associated with suspicious binary hosting
- Historical malicious associations observed through threat intelligence
- Potential payload hosting infrastructure

```text
45.125.66.32
```

**Observed Activity:**

- Suspicious TLS communication
- Self-signed certificate observed


### Suspicious Domain

```text
authenticatoor.org
```

**Reason:**

- Typosquatting domain impersonating **Google Authenticator**
- Associated with fake authentication software
- Potential malware delivery infrastructure

The domain name aligns with the reported user activity and was used to deliver the malicious application.

---


## Investigation Summary

The investigation identified a malware infection involving a fake Google Authenticator application and suspicious network activity from the compromised workstation.

A detailed investigation report is available here: [Incident Report](Incident-Report/incident_report.md)

---


## Key Findings

The investigation confirmed:

- Fake Google Authenticator malware delivery
- Communication with attacker-controlled infrastructure
- PowerShell-based payload delivery
- Multi-stage malware downloads
- Startup folder persistence through fake TeamViewer components
- Suspicious TLS communication
  
---

## Screenshots

### 1. Wireshark Protocol Hierarchy
- [01_wireshark_protocol_hierarchy.png](Screenshots/01_wireshark_protocol_hierarchy.png)

### 2. Wireshark Endpoints Analysis
- [02_wireshark_endpoints.png](Screenshots/02_wireshark_endpoints.png)

### 3. Wireshark Conversations
- [03_wireshark_conversations.png](Screenshots/03_wireshark_conversations.png)

### 4. DNS Investigation
- [04_dns_analysis.png](Screenshots/04_dns_analysis.png)

### 5. TLS SNI Analysis
- [05_tls_sni_analysis.png](Screenshots/05_tls_sni_analysis.png)

### 6. HTTP Stream Malware Download
- [06_http_stream_malware_download.png](Screenshots/06_http_stream_malware_payload_delivery.png)

### 7. HTTP Stream Obfuscated PowerShell Payload
- [07_http_stream_powershell_payload.png](Screenshots/07_http_stream_obfuscated_powershell_payload.png)

### 8. TLS Certificate Analysis
- [08_tls_self_signed_certificate.png](Screenshots/08_tls_self_signed_certificate.png)

### 9. Suricata IDS Alerts
- [09_suricata_fast_log_alerts.png](Screenshots/09_suricata_fast_log_alerts.png)

### 10. VirusTotal Reputation Analysis
- [10_malicious_ip_reputation.png](Screenshots/10_malicious_ip_reputation.png)

### 11. Attack Timeline
- [11_attack_timeline.png](Screenshots/11_attack_timeline.png)

### 12. Incident Summary
- [12_incident_summary.png](Screenshots/12_incident_summary.png)


---

## Attack Timeline

| Time     | Activity |
| -------- | -------- |
| 14:44:56 | Malicious payload delivered |
| 14:45:56 | Communication with malicious server |
| 14:45:58 | PowerShell script downloaded |
| 14:47:01 | Additional payload retrieval |
| 14:47:02 | Executable/DLL downloaded |
| 14:55:08 | TeamViewer-related communication observed |

The complete investigation timeline, including evidence sources and detection details, is available in the Evidence folder.

---


## MITRE ATT&CK Mapping

| Activity | Technique | ID |
|---|---|---|
| Malicious Application Execution | User Execution: Malicious File | T1204.002 |
| PowerShell Execution | Command and Scripting Interpreter: PowerShell | T1059.001 |
| Payload Download | Ingress Tool Transfer | T1105 |
| PowerShell Obfuscation | Obfuscated Files or Information | T1027 |
| HTTP C2 Communication | Application Layer Protocol: Web Protocols | T1071.001
| Startup Persistence | Boot or Logon Autostart Execution | T1547.001 |
| Fake Application Naming | Masquerading | T1036 |

---


## Conclusion

This project demonstrates a SOC analyst workflow for investigating malware infections using network telemetry.

Wireshark provided packet-level visibility, while Suricata detected malicious network behavior and generated IDS alerts.

The collected evidence was analyzed, documented, and mapped to MITRE ATT&CK techniques as part of an incident response investigation.
