# Incident Report — Malware Infection Investigation

## Incident Title

Malware Infection Investigation Using Wireshark and Suricata

---

## Executive Summary

A Security Operations Center (SOC) investigation was initiated after a user reported downloading a suspicious application while searching for Google Authenticator.

Based on the initial user report and threat intelligence references, the downloaded application was suspected to be a malicious fake authentication application.

A packet capture (PCAP) containing the associated network activity was collected and analyzed using Wireshark and Suricata.

The investigation confirmed malicious network activity originating from the affected workstation (`10.1.17.215`).

The analysis identified:

- Communication with suspicious external infrastructure
- DNS resolution of a suspicious authentication-themed domain
- HTTP-based malware delivery
- PowerShell payload retrieval
- Obfuscated PowerShell content
- Additional executable and DLL downloads
- Command-and-control (C2) communication
- Indicators consistent with a multi-stage malware infection

The incident was classified as a confirmed malware infection based on network evidence, IDS detections, and malware payload analysis.

---

## Incident Information

| Field | Details |
| --- | --- |
| Incident Type | Malware Infection |
| Investigation Type | Network Threat Hunting |
| Affected Host | 10.1.17.215 |
| Network Range | 10.1.17.0/24 |
| Domain | bluemoontuesday.com |
| Domain Controller | 10.1.17.2 - WIN-GSH54QLW48D |
| Analysis Platform | Kali Linux |
| Evidence Source | PCAP |
| Analysis Tools | Wireshark, Suricata, VirusTotal |
| Severity | High |
| Status | Confirmed |

---

## Environment Information

The provided PCAP represented a Windows Active Directory environment.

Network details:

| Field | Details |
| --- | --- |
| LAN | 10.1.17.0/24 |
| Gateway | 10.1.17.1 |
| Broadcast | 10.1.17.255 |
| Domain | bluemoontuesday.com |
| AD Domain | BLUEMOONTUESDAY |
| Domain Controller | 10.1.17.2 - WIN-GSH54QLW48D |

---

## Initial Network Assessment

### Protocol Analysis

The PCAP contained approximately 39,000 packets.

Initial protocol analysis showed:

| Category | Observed Protocols / Details |
| --- | --- |
| Network Layer | IPv4 over Ethernet |
| Transport Layer | TCP (~93% of packets) |
| Encryption | TLS-encrypted sessions |
| Windows Enterprise Protocols | SMB, SMB2, Kerberos, LDAP, DCE/RPC, NetBIOS |
| Infrastructure Protocols | DNS, DHCP, ARP, NTP, mDNS, LLMNR, SSDP |


At this stage, protocol distribution alone did not indicate malicious activity. The traffic was consistent with a normal Windows enterprise environment.

Further investigation focused on:

- Network conversations
- DNS activity
- HTTP traffic
- TLS metadata
- Packet streams
- IDS detections

---

## Endpoint Analysis

Wireshark endpoint analysis identified the primary systems involved in the network activity.

| Asset | Role | Observation |
| --- | --- | --- |
| `10.1.17.215` | Internal workstation | Primary investigation target. Communicated with multiple external hosts and generated the majority of observed external traffic. |
| `10.1.17.2` | Active Directory Domain Controller | Generated expected Windows domain activity, including internal enterprise communication. |

Initial observations:

| Observation | Analyst Reasoning |
| --- | --- |
| Multiple external connections from the workstation | Required further investigation as the likely infected endpoint |
| Domain controller communication | Consistent with normal Active Directory operations |
| Broadcast traffic | Expected DHCP/ARP behavior |
| Small external connections | Likely normal background internet activity |
| Large transfers involving external IP addresses | Prioritized for deeper investigation |

The investigation did not determine maliciousness based solely on traffic volume. Additional analysis focused on communication patterns, DNS activity, HTTP traffic, TLS metadata, and IDS detections.

---

## Conversation Analysis

Wireshark Conversations analysis was used to identify the primary communication relationships between internal and external hosts.

The largest internal communication observed was between:

| Source | Destination | Assessment |
| --- | --- | --- |
| `10.1.17.215` | `10.1.17.2` | Expected Active Directory communication between a workstation and domain controller |

The primary external communications requiring further investigation involved:

| External IP | Observation |
| --- | --- |
| `5.252.153.241` | Large data transfer volume observed from the workstation |
| `45.125.66.32` | Suspicious external communication involving TLS activity |
| `82.221.136.26` | Associated with suspicious domain resolution and payload hosting activity |

The affected workstation (`10.1.17.215`) received significantly more data than it transmitted during these external communications.

This traffic pattern was consistent with possible:

- Malware payload retrieval
- Software download activity
- Web-based content delivery

Further protocol-level analysis was performed using DNS, HTTP, TLS metadata, packet streams, and IDS detections to determine whether the activity was malicious.

---

## DNS Investigation

DNS analysis identified a suspicious domain associated with the malware activity:

`authenticatoor.org`

The domain appears to mimic legitimate authentication software branding and may represent a typosquatting attempt targeting users searching for Google Authenticator.

Characteristics:

- Similar spelling to Google Authenticator
- Potential typosquatting behavior
- Associated with suspicious malware activity

DNS resolution:

```text
authenticatoor.org
        |
        ▼
82.221.136.26
```
The DNS activity aligned with the initial user report involving a fake authentication application.

---

## TLS SNI Investigation

TLS Client Hello analysis identified the Server Name Indication (SNI):

`authenticatoor.org`

This confirmed that the affected workstation initiated an encrypted TLS connection to the suspicious domain.

Although TLS encryption prevented inspection of the packet payload contents, SNI metadata provided visibility into the requested hostname and supported the identification of the malicious communication.

---

## HTTP Malware Delivery Analysis

HTTP stream analysis identified malicious payload delivery.

Observed requests included:

`GET /api/file/get-file/29842.ps1`

The response contained PowerShell content.

The downloaded script contained suspicious functionality including:

`Invoke-Expression`

`FromBase64String()`

`DownloadString()`

`DownloadFile()`

These techniques are commonly associated with:

- Malware loaders
- Payload execution
- Obfuscation
- Defense evasion

---

## PowerShell Loader Analysis

The PowerShell payload demonstrated multi-stage malware behavior, including payload decoding, in-memory execution, additional file retrieval, and persistence preparation.

Observed behaviors:

1. Decoding embedded Base64 content
2. Executing decoded PowerShell content in memory
3. Downloading additional malware components
4. Creating persistence mechanisms

Files retrieved during the malware execution chain included:

- `TeamViewer.exe`
- `TV.dll`
- `Teamviewer_Resource_fr.dll`
- `pas.ps1`

The use of legitimate software names, such as TeamViewer, suggests possible masquerading behavior designed to appear as legitimate software and evade user suspicion.

---

## Persistence Analysis

The PowerShell payload contained functionality to establish persistence by creating a startup shortcut:

`TeamViewer.lnk`

The shortcut referenced the following executable:

`C:\ProgramData\huo\TeamViewer.exe`

This behavior indicates an attempt to automatically execute the malware during user logon through the Windows Startup mechanism.

**Note:**  
Persistence was identified through malware content analysis. Actual endpoint execution and successful persistence activation could not be directly confirmed from the PCAP alone.

---

## Suricata IDS Analysis

Suricata was used to analyze the PCAP and provide IDS-based detection of suspicious network activity.

Suricata provides:

- Signature-based malware detection
- Known malicious pattern identification
- Alert generation
- Network threat visibility

The following Suricata alerts were observed during the investigation:

| Alert Category | Suricata Signature | Analyst Interpretation |
| --- | --- | --- |
| Malware Payload Delivery | `ET MALWARE Fake Microsoft Teams VBS Payload Inbound` | Detected initial malicious payload delivery activity. |
| Command and Control Activity | `ET MALWARE Fake Microsoft Teams CnC Payload Request (GET)` | Identified communication with suspected malware infrastructure. |
| PowerShell Download Activity | `ET INFO PS1 Powershell File Request`<br>`ET HUNTING Generic Powershell DownloadFile Command`<br>`ET HUNTING Generic Powershell DownloadString Command` | Identified PowerShell-based payload retrieval and downloader behavior. |
| Executable Download | `ET INFO PE EXE or DLL Windows file download` | Detected retrieval of additional executable payload components. |

---

## Indicators of Compromise

| IP Address | Role | Observed Activity |
| --- | --- | --- |
| `5.252.153.241` | Malware delivery server / C2 infrastructure | Fake Microsoft Teams payload activity and PowerShell download activity |
| `82.221.136.26` | Suspicious payload hosting infrastructure | Associated with suspicious files and DNS resolution activity |
| `45.125.66.32` | Suspicious TLS infrastructure | Self-signed certificate and IP-based certificate identity observed |

---

## Attack Timeline

| Time | Activity | Evidence |
| --- | --- | --- |
| 14:44:56 | Initial malicious payload delivery | Suricata malware alert |
| 14:45:56 | Connection to malicious server | HTTP communication |
| 14:45:58 | PowerShell payload requested | PS1 request detected |
| 14:47:01 | PowerShell downloader activity | DownloadString/DownloadFile |
| 14:47:02 | Executable/DLL download | PE download detection |
| 14:55:08 | TeamViewer-related communication | Remote access activity |

---

## MITRE ATT&CK Mapping

| Technique | ID | Evidence |
| --- | --- | --- |
| User Execution | T1204 | User executed fake authentication software |
| PowerShell | T1059.001 | PowerShell payload execution |
| Ingress Tool Transfer | T1105 | Malware downloads |
| Obfuscated Files or Information | T1027 | Base64 and PowerShell obfuscation |
| Web Protocols | T1071.001 | HTTP communication |
| Masquerading | T1036 | Fake Google Authenticator and TeamViewer naming |
| Boot or Logon Autostart Execution | T1547.001 | Startup shortcut creation |

---

## Impact Assessment

The investigation confirmed the following:

- A malicious application was downloaded
- The workstation communicated with attacker-controlled infrastructure
- Malware payloads were delivered
- PowerShell-based execution was attempted
- Additional malware components were retrieved
- Persistence behavior was identified

### Potential impact includes:

- Unauthorized remote access
- Additional malware installation
- Credential theft
- Continued attacker access

---

## Recommendations

| Priority | Recommendation |
| --- | --- |
| High | Isolate affected workstation (`10.1.17.215`) |
| High | Block malicious indicators: `5.252.153.241`, `82.221.136.26`, `45.125.66.32`, `authenticatoor.org` |
| High | Perform endpoint investigation including process analysis, persistence review, and malware removal |
| Medium | Reset credentials if compromise is suspected |
| Medium | Review DNS and proxy logs for additional affected systems |
| Low | Monitor for similar fake software campaigns |

---

## Final Analyst Conclusion

The investigation confirmed a malware infection involving a fake Google Authenticator application.

Network evidence demonstrated:

```text
Malicious Application Download
            |
            ▼
DNS Resolution
            |
            ▼
TLS Communication
            |
            ▼
PowerShell Payload Delivery
            |
            ▼
Additional Malware Downloads
            |
            ▼
Persistence Attempt
```

The combination of Wireshark packet analysis and Suricata IDS detection provided sufficient evidence to reconstruct the malware infection chain and document the incident response process.
