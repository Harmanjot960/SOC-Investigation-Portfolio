# RDP Brute Force Attack Investigation Using Splunk

## Overview

This project demonstrates a Security Operations Center (SOC) investigation of a simulated Remote Desktop Protocol (RDP) brute-force attack against a Windows endpoint.

The attack was simulated using Kali Linux Hydra. Windows Security Event Logs were collected and analyzed in Splunk to identify repeated failed authentication attempts, determine the source IP, and confirm successful authentication.

---

# Incident Scenario

A Windows workstation generated multiple failed RDP authentication attempts targeting the user account **analyst**.

The SOC analyst investigated:

- Source IP responsible for authentication attempts
- Targeted user account
- Failed authentication pattern
- Successful authentication after repeated failures
- Attack timeline

---

# Environment

| Component | Details |
|-----------|---------|
| SIEM | Splunk |
| Operating System | Windows |
| Hostname | DESKTOP-SHNKQPV |
| Target IP | 192.168.109.129 |
| Attacker IP | 192.168.109.130 |
| Attack Tool | Kali Linux Hydra |
| Attack Protocol | Remote Desktop Protocol (RDP) |
| Log Source | Windows Security Logs |
| Splunk Index | windows |

---

# Lab Architecture

```
Kali Linux
192.168.109.130
        |
        | Hydra RDP Brute Force Simulation
        |
        ▼
Windows Endpoint
DESKTOP-SHNKQPV
192.168.109.129
        |
        ▼
Windows Security Logs
(Event ID 4625, 4624, 4672)
        |
        ▼
Splunk SIEM
Index: windows
        |
        ▼
RDP Brute Force Detection
and Investigation
```

---

# Investigation Flow

The investigation followed a SOC workflow from attack simulation to incident documentation.

```
              RDP Brute Force Simulation
                         |
                         ▼
                  Initial Triage
        (Review authentication activity)
                         |
                         ▼
                Evidence Collection
        (Windows Security Event Logs)
                         |
                         ▼
                 Log Analysis
        (4625 Failed Logons / 4624 Success)
                         |
                         ▼
                  IOC Extraction
        (Source IP, Username, RDP Activity)
                         |
                         ▼
              Splunk Investigation
        (Detection Query and Timeline Analysis)
                         |
                         ▼
              MITRE ATT&CK Mapping
                         |
                         ▼
                Incident Report
```

---

# Supporting Documents

```
RDP-Brute-Force-Investigation
│
├── README.md
│
├── Screenshots
│   ├── kali_hydra_command.png
│   ├── kali_hydra_result.png
│   ├── splunk_failed_logons.png
│   ├── splunk_successful_logon.png
│   ├── splunk_attack_timeline.png
│   └── splunk_bruteforce_detection_query.png
│
├── Evidence
│   ├── timeline.md
│   ├── iocs.md
│   └── artifacts.md
│
├── SPL-Queries
│   ├── brute_force_detection.spl
│   └── authentication_analysis.spl
│
├── MITRE-ATT&CK
│   └── attack_mapping.md
│
└── Incident-Report
    └── incident_report.md
```

---

# Attack Simulation

The RDP brute-force activity was simulated using Kali Linux Hydra against the Windows endpoint.

## Hydra Command

Screenshot:

```
kali_hydra_command.png
```

This screenshot shows the command used to simulate repeated RDP authentication attempts.

---

## Hydra Result

Screenshot:

```
kali_hydra_result.png
```

This screenshot shows the result of the simulated password testing activity.

The authentication attempts generated Windows Security Events that were later analyzed in Splunk.

---

# Data Sources

## Windows Security Logs

**Sourcetype**

```
WinEventLog:Security
```

Collected Event IDs:

| Event ID | Description |
|----------|-------------|
| 4625 | Failed Logon |
| 4624 | Successful Logon |
| 4672 | Special Privileges Assigned (Additional Evidence) |

These events were used to identify brute-force activity and confirm successful authentication.

---

# Splunk Detection Rule

## RDP Brute Force Detection

This detection identifies multiple failed RDP authentication attempts from the same source IP targeting the same account.

### Detection Logic

```
Multiple Event ID 4625 (Failed Logon)
+
Same Source IP
+
Same Account Name
+
RDP Authentication Activity
```

### SPL Detection Query

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Source_IP, Account_Name
| where count >= 10
```

### Detection Purpose

This query identifies possible RDP password guessing activity by detecting repeated failed authentication attempts from the same source.

---

# Investigation Summary

The investigation identified repeated failed RDP authentication attempts originating from:

```
Source IP:
192.168.109.130
```

Targeting:

```
Account:
analyst
```

Findings:

- 21 failed RDP authentication attempts detected
- Same source IP used for all attempts
- Same account targeted
- Successful authentication occurred after failed attempts

---

# Key Findings

## RDP Brute Force Activity

Detected activity:

```
Event ID: 4625
Event Name: Failed Logon
Protocol: RDP
Logon Type: 3
Account: analyst
Source IP: 192.168.109.130
Destination IP: 192.168.109.129
```

The repeated failed authentication attempts indicate a possible password guessing attack.

---

## Successful Authentication

A successful login was identified:

```
Event ID: 4624
Event Name: Successful Logon
Protocol: RDP
Logon Type: 3
Account: analyst
Source IP: 192.168.109.130
Destination IP: 192.168.109.129
```

The successful authentication occurred immediately after multiple failed attempts, indicating that valid credentials may have been obtained.

---

# Splunk Investigation

## Identify Failed RDP Attempts

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Source_IP, Account_Name
| where count > 10
```

---

## Review Successful Logons

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4624
| table _time Account_Name Source_IP Destination_IP Logon_Type
```

---

## Create Attack Timeline

```spl
index=windows sourcetype="WinEventLog:Security"
(EventCode=4625 OR EventCode=4624 OR EventCode=4672)
| table _time EventCode Account_Name Source_IP Destination_IP Logon_Type
| sort 0 _time
```

---

# Investigation Timeline

| Time | Event ID | Description |
|------|----------|-------------|
| 08:08:42 - 08:23:08 | 4625 | Multiple failed RDP authentication attempts |
| 08:23:11 | 4624 | Successful RDP authentication |
| 08:34:10 | 4672 | Special privileges assigned |

---

# MITRE ATT&CK Mapping

| Activity | Technique | ID |
|----------|-----------|----|
| RDP Access | Remote Services: Remote Desktop Protocol | T1021.001 |
| Password Guessing | Brute Force: Password Guessing | T1110.001 |
| Account Access | Valid Accounts | T1078 |

---

# Evidence Collected

The investigation contains:

- Hydra attack simulation screenshots
- Windows Security Event Logs
- Splunk detection queries
- RDP brute-force detection logic
- Attack timeline
- Investigation screenshots
- MITRE ATT&CK mapping
- Incident findings

---

# Conclusion

The investigation confirmed a simulated RDP brute-force attack against a Windows endpoint.

The attacker IP `192.168.109.130` generated multiple failed authentication attempts against the `analyst` account before successfully authenticating through RDP.

By analyzing Windows Security Events in Splunk, the SOC analyst identified the attack source, targeted account, authentication pattern, and successful access attempt.
