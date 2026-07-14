# RDP Brute Force Attack Investigation Using Splunk

## Overview

This project demonstrates a Security Operations Center (SOC) investigation of a suspected Remote Desktop Protocol (RDP) brute-force attack against a Windows endpoint.

Simulated Windows Security Event Logs and Sysmon telemetry were ingested into Splunk to detect suspicious authentication activity, identify the attacking source, confirm successful access, and investigate post-compromise activity.

---

# Incident Scenario

A Windows workstation generated a large number of failed RDP authentication attempts targeting the local administrator account.

The SOC analyst received an alert for possible brute-force activity and investigated:

- Source IP responsible for authentication attempts
- Targeted user account
- Successful authentication after failed attempts
- Privilege assignment
- Attacker activity after access

---

# Environment

| Component | Details |
|---|---|
| SIEM | Splunk |
| Operating System | Windows |
| Hostname | DESKTOP-SHNKQPV |
| Target IP | 192.168.109.128 |
| Attacker IP | 192.168.109.50 |
| Attack Protocol | Remote Desktop Protocol (RDP) |
| Log Sources | Windows Security Logs and Sysmon |
| Splunk Index | windows |

---

# Lab Architecture

The investigation environment simulated a SOC workflow where endpoint telemetry was collected and analyzed through Splunk.

```
Attacker Machine
192.168.109.50
        |
        | RDP Authentication Attempts
        ▼
Windows Endpoint
DESKTOP-SHNKQPV
192.168.109.128
        |
        |
        +----------------------+
        |                      |
        ▼                      ▼
Windows Security Logs       Sysmon Telemetry
(Event ID 4625,4624,4672)   (Event ID 1,3)
        |                      |
        +----------+-----------+
                   |
                   ▼
              Splunk SIEM
              Index: windows
                   |
                   ▼
        Detection & Investigation
        - RDP brute-force detection
        - Authentication analysis
        - Process investigation
        - Network activity review
```

The architecture demonstrates the flow of security telemetry from endpoint activity to SIEM investigation.

---

# Data Sources

## Windows Security Logs

**Sourcetype**

```
WinEventLog:Security
```

Collected Event IDs:

| Event ID | Description |
|---|---|
| 4625 | Failed Logon |
| 4624 | Successful Logon |
| 4672 | Special Privileges Assigned |

These events were used to identify brute-force attempts, successful authentication, and privileged access.

---

## Sysmon Logs

**Sourcetype**

```
XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```

Collected Event IDs:

| Event ID | Description |
|---|---|
| 1 | Process Creation |
| 3 | Network Connection |

These events were used to investigate attacker actions after successful login.

---

# Investigation Summary

The investigation identified repeated failed RDP authentication attempts against the administrator account.

Findings:

- Multiple failed RDP login attempts detected
- Same source IP repeatedly attempted authentication
- Successful RDP login occurred after brute-force attempts
- Administrative privileges were assigned
- PowerShell execution and network activity were observed after compromise

---

# Key Findings

## Initial Access

Detected activity:

```
Event ID: 4625
Event Name: Failed Logon
Protocol: RDP
Logon Type: 10
Account: administrator
Source IP: 192.168.109.50
```

The repeated failed authentication attempts indicate a possible RDP brute-force attack.

---

## Successful Authentication

A successful login was identified:

```
Event ID: 4624
Event Name: Successful Logon
Account: administrator
Protocol: RDP
Source IP: 192.168.109.50
```

This indicates that the attacker successfully authenticated.

---

## Privileged Access

The account received elevated privileges:

```
Event ID: 4672
Event Name: Special Privileges Assigned
Account: administrator
```

This confirms privileged account activity after authentication.

---

## Post-Compromise Activity

Sysmon telemetry identified suspicious activity after login.

Observed activity:

- Process execution
- PowerShell usage
- Network communication

Examples:

```
powershell.exe -Command whoami
```

---

# Splunk Investigation

## Detect RDP Brute Force Activity

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Source_IP,Account_Name
| where count > 10
```

---

## Investigate Successful Login

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4624
| table _time Account_Name Source_IP Destination_IP Logon_Type Protocol
```

---

## Analyze Privileged Access

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4672
| table _time Account_Name Source_IP Event_Name
```

---

## Correlate Security and Sysmon Events

```spl
index=windows Source_IP="192.168.109.50"
| sort 0 _time
| table _time sourcetype EventCode Event_Name Image CommandLine
```

---

# Investigation Timeline

| Time | Event | Description |
|---|---|---|
| 10:00:01 - 10:01:59 | 4625 | Multiple failed RDP authentication attempts |
| 10:02:01 | 4624 | Successful RDP authentication |
| 10:02:10 | 4672 | Administrator privileges assigned |
| 10:02:20 | Sysmon Event ID 1 | Process creation detected |
| 10:02:30 | Sysmon Event ID 3 | Network connection detected |

---

# MITRE ATT&CK Techniques

| Activity | Technique | ID |
|---|---|---|
| RDP Access | Remote Services: Remote Desktop Protocol | T1021.001 |
| External RDP Exposure | External Remote Services | T1133 |
| Account Usage | Valid Accounts | T1078 |
| PowerShell Execution | Command and Scripting Interpreter: PowerShell | T1059.001 |

---

# Evidence Collected

The investigation contains:

- Windows Security event logs
- Sysmon telemetry
- Splunk detection queries
- Investigation screenshots
- Extracted indicators
- Attack timeline
- MITRE ATT&CK mapping
- Incident report

---

# Conclusion

The investigation confirmed a suspected RDP brute-force attack against a Windows endpoint.

The attacker performed multiple failed authentication attempts before successfully accessing the administrator account.

By correlating Windows Security Events and Sysmon telemetry in Splunk, the SOC analyst identified the attack pattern, confirmed compromise, analyzed post-compromise behavior, and documented the incident.

