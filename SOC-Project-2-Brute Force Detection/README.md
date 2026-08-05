# RDP Brute Force Attack Investigation Using Splunk

## Overview

This project demonstrates a Security Operations Center (SOC) investigation of a simulated Remote Desktop Protocol (RDP) brute-force attack against a Windows endpoint.

The attack was simulated using Kali Linux Hydra. Windows Security Event Logs were collected and analyzed in Splunk to identify repeated failed authentication attempts, determine the source IP, and confirm successful authentication.

---

## Incident Scenario

A Windows workstation generated multiple failed RDP authentication attempts targeting the user account **analyst**.

The SOC analyst investigated:

- Source IP responsible for authentication attempts
- Targeted user account
- Failed authentication pattern
- Successful authentication after repeated failures
- Attack timeline

---

## Environment

| Component | Details |
|-----------|---------|
| SIEM | Splunk |
| Operating System | Windows |
| Hostname | DESKTOP-SHNKQPV |
| Target IP | 192.168.109.129 |
| Attacker IP | 192.168.109.130 |
| Attacker Platform | Kali Linux |
| Attack Tool | Hydra |
| Attack Protocol | Remote Desktop Protocol (RDP) |
| Log Source | Windows Security Logs |
| Splunk Index | windows |

---

## Investigation Source

This investigation was performed in a personal SOC Home Lab using simulated RDP brute-force activity generated from a Kali Linux attacker against a Windows endpoint.

The collected Windows Security Event Logs were analyzed in Splunk to reconstruct the attack timeline and identify indicators of compromise.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Splunk Enterprise | Log collection and investigation |
| Windows Event Viewer | Event log validation |
| Kali Linux | Attack simulation using Hydra |
| Hydra | Simulated RDP brute-force attack |

---

## Lab Architecture

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

## Investigation Workflow

The investigation followed a SOC workflow from attack simulation to incident documentation.

```
              RDP Brute Force Simulation
                         |
                         ▼
                  Alert Triage
        (Review authentication activity)
                         |
                         ▼
                Evidence Collection
        (Windows Security Event Logs)
                         |
                         ▼
                 Log Analysis
        (4625 Failed Logons / 4624 Successful Logon / 4672 Privileged Logon)
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

## Project Structure

```
RDP-Brute-Force-Investigation
│
├── README.md
│
├── Screenshots
│   ├── 01a_hydra_command.png
│   ├── 01b_hydra_password_found.png
│   ├── 02_failed_rdp_logons_event_4625.png
│   ├── 03_failed_login_analysis_splunk.png
│   ├── 04_successful_rdp_login_event_4624.png
│   ├── 05_event_4672_special_privileges.png
│   ├── 06_splunk_bruteforce_detection_query.png
│   ├── 07_rdp_attack_timeline.png
│   └── 08_incident_summary.png
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

## Attack Simulation

The RDP brute-force activity was simulated using **Kali Linux Hydra** against the Windows endpoint to generate Windows Security Events for analysis in Splunk.

### Hydra Brute Force Command

**Evidence:** [01a_hydra_command.png](Screenshots/01a_hydra_command.png)

This screenshot shows the Hydra command used to simulate repeated RDP authentication attempts against the target Windows system.

---

### Successful Credential Discovery

**Evidence:** [01b_hydra_password_found.png](Screenshots/01b_hydra_password_found.png)

This screenshot shows Hydra successfully identifying the valid credentials after multiple authentication attempts.

The simulated attack generated Windows Security Event IDs **4625 (Failed Logon)**, **4624 (Successful Logon)**, and **4672 (Special Privileges Assigned)**, which were collected and analyzed in Splunk during the investigation.

---

## Data Sources

### Windows Security Logs

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

## Splunk Investigation

Windows Security Event Logs were analyzed in Splunk to identify the brute-force attack, confirm successful authentication, and reconstruct the attack timeline.

### Detect Failed RDP Authentication Attempts

**Purpose:** Identify repeated failed RDP authentication attempts from the same source IP targeting the same account.

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Source_IP, TargetUserName
| where count >= 10
| sort -count
```

---

### Review Successful Authentication

**Purpose:** Confirm whether the brute-force attack resulted in a successful RDP login.

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4624
| table _time TargetUserName Source_IP Destination_IP Logon_Type
```

---

### Reconstruct the Attack Timeline

**Purpose:** Correlate failed logons, successful authentication, and privileged logon events to reconstruct the attack sequence.

```spl
index=windows sourcetype="WinEventLog:Security"
(EventCode=4625 OR EventCode=4624 OR EventCode=4672)
| table _time EventCode TargetUserName Source_IP Destination_IP Logon_Type
| sort 0 _time
```

---

## Key Findings

The investigation identified repeated RDP brute-force authentication attempts originating from:
```
Source IP:
192.168.109.130

Target Account:
analyst
```

### Failed RDP Authentication

```
Event ID: 4625
Protocol: RDP
Logon Type: 3
```

Findings:

- 20 failed RDP authentication attempts were detected.
- All attempts originated from `192.168.109.130`.
- The `analyst` account was repeatedly targeted.
- The repeated failures indicate a password guessing (brute-force) attack.

---

### Successful Authentication

```
Event ID: 4624
Protocol: RDP
Logon Type: 3
```

Following the failed authentication attempts, a successful RDP logon was recorded from the same source IP.

An **Event ID 4672 (Special Privileges Assigned)** was generated immediately afterward, indicating that the authenticated session received special privileges.
Together, these events indicate that the brute-force attack successfully compromised the target account.

---

## Screenshots

### 1. Hydra Brute Force Command
- [01a_hydra_command.png](Screenshots/01a_hydra_command.png)

### 2. Hydra Password Discovery
- [01b_hydra_password_found.png](Screenshots/01b_hydra_password_found.png)

### 3. Failed RDP Logons (Event ID 4625)
- [02_failed_rdp_logons_event_4625.png](Screenshots/02_failed_rdp_logons_event_4625.png)

### 4. Failed Login Analysis in Splunk
- [03_failed_login_analysis_splunk.png](Screenshots/03_failed_login_analysis_splunk.png)

### 5. Successful RDP Logon (Event ID 4624)
- [04_successful_rdp_login_event_4624.png](Screenshots/04_successful_rdp_login_event_4624.png)

### 6. Special Privileges Assigned (Event ID 4672)
- [05_event_4672_special_privileges.png](Screenshots/05_event_4672_special_privileges.png)

### 7. Splunk Brute Force Detection Query
- [06_splunk_bruteforce_detection_query.png](Screenshots/06_splunk_bruteforce_detection_query.png)

### 8. RDP Attack Timeline
- [07_rdp_attack_timeline.png](Screenshots/07_rdp_attack_timeline.png)

### 9. Investigation Summary
- [08_incident_summary.png](Screenshots/08_incident_summary.png)

---

## Investigation Timeline

| Time | Event ID | Description |
|------|----------|-------------|
| 08:08:42 - 08:23:08 | 4625 | Multiple failed RDP authentication attempts |
| 08:23:11 | 4624 | Successful RDP authentication |
| 08:23:29 | 4672 | Special privileges assigned |

---

## MITRE ATT&CK Mapping

| Activity | Technique | ID |
|----------|-----------|----|
| RDP Access | Remote Services: Remote Desktop Protocol | T1021.001 |
| Password Guessing | Brute Force: Password Guessing | T1110.001 |
| Account Access | Valid Accounts | T1078 |

---

## Evidence Collected

The investigation contains:

- Hydra attack simulation screenshots
- Windows Security Event Logs
- Splunk detection queries
- RDP brute-force detection logic
- Attack timeline
- Investigation screenshots
- MITRE ATT&CK mapping
- Incident findings

Supporting investigation artifacts are available in the `Evidence/` directory:

- [artifacts.md](Evidence/artifacts.md)
- [iocs.md](Evidence/iocs.md)
- [timeline.md](Evidence/timeline.md)

---

## Conclusion

Analysis of Windows Security Event Logs confirmed a simulated RDP brute-force attack originating from 192.168.109.130. 

Multiple failed authentication attempts against the analyst account were followed by a successful login (Event ID 4624) and a privileged logon event (Event ID 4672). 

Splunk enabled the analyst to reconstruct the attack timeline, identify the source IP, determine the targeted account, and document the incident.


