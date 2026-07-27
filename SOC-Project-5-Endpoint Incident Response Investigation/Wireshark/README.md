# Wireshark Network Analysis

This directory contains network traffic analysis performed during the Windows endpoint incident response investigation.

Wireshark was used to analyze network communication, identify attacker infrastructure, and correlate network activity with endpoint telemetry.

---

# Investigation Objectives

Network analysis was performed to identify:

- Malicious external communication
- Malware delivery traffic
- Command-and-control activity
- Attacker infrastructure
- Remote access activity
- Network tunneling behavior

---

# Analysis Workflow

```text
PCAP Traffic
      |
      ▼
Protocol Analysis
      |
      ▼
Endpoint & Conversation Analysis
      |
      ▼
DNS Investigation
      |
      ▼
HTTP Communication Analysis
      |
      ▼
IOC Extraction
      |
      ▼
Attack Timeline Correlation
```

---

# Key Findings

The investigation identified communication with:

## Payload Delivery Infrastructure

```text
phishteam.xyz
```

**Observed payload-related files:**

Initial delivery:

```text
update.zip
first.exe
ch.exe
```

Later WinRM session download:

```text
spf.exe
final.exe
```

---

## Command and Control Infrastructure

```text
resolvecyber.xyz
```

Used for:

- Encoded command retrieval
- Malware communication
- Remote attacker instructions

---

## Chisel Tunnel Endpoint

```text
167.71.199.191:8080
```

Used for:

- Reverse SOCKS tunneling
- Proxy communication
- Remote access

---

# Evidence Sources

The Wireshark investigation was correlated with:

- Sysmon Event ID 1 — Process Creation
- Sysmon Event ID 3 — Network Connection
- Sysmon Event ID 22 — DNS Query
- Windows Security Events

Detailed analysis:
[network-analysis.md](Wireshark/network-analysis.md)
