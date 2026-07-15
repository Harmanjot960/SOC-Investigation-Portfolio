# RDP Brute Force Attack Investigation Using Splunk

## Overview

This project demonstrates a Security Operations Center (SOC) investigation of a simulated Remote Desktop Protocol (RDP) brute-force attack against a Windows endpoint.

Windows Security Event Logs were collected and analyzed in Splunk to detect repeated failed authentication attempts, identify the attacking source IP, and confirm successful account access.

---

# Incident Scenario

A Windows workstation generated multiple failed RDP authentication attempts against the user account **Analyst**.

The SOC analyst investigated:

- Source IP responsible for authentication attempts
- Targeted account
- Failed authentication pattern
- Successful authentication after brute-force attempts
- Timeline of the attack

---

# Environment

| Component | Details |
|-----------|---------|
| SIEM | Splunk |
| Operating System | Windows |
| Hostname | DESKTOP-SHNKQPV |
| Target IP | 192.168.109.129 |
| Attacker IP | 192.168.109.130 |
| Attack Protocol | Remote Desktop Protocol (RDP) |
| Log Source | Windows Security Logs |

---

# Lab Architecture

```
Kali Linux
192.168.109.130
        |
        | RDP Authentication Attempts
        |
        ▼
Windows Endpoint
DESKTOP-SHNKQPV
192.168.109.129
        |
        |
        ▼
Windows Security Logs
(Event ID 4625, 4624)
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

These events were used to identify brute-force attempts and confirm successful authentication.

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
Analyst
```

The attack generated multiple failed authentication events followed by a successful login.

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

The successful authentication occurred immediately after multiple failed attempts, indicating possible account compromise.

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

| Time | Event | Description |
|------|-------|-------------|
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

- Windows Security Event Logs
- Splunk searches
- RDP brute-force detection query
- Attack timeline
- Investigation screenshots
- MITRE ATT&CK mapping
- Incident findings

---

# Conclusion

The investigation confirmed a simulated RDP brute-force attack against a Windows endpoint.

The attacker IP `192.168.109.130` generated multiple failed authentication attempts against the `analyst` account before successfully authenticating through RDP.

By analyzing Windows Security Events in Splunk, the SOC analyst was able to identify the attack source, targeted account, authentication pattern, and successful access attempt.

