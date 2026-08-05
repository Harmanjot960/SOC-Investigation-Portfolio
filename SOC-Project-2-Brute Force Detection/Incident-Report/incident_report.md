# Incident Report — RDP Brute Force Attack Investigation

## Executive Summary

A Security Operations Center (SOC) investigation was conducted following suspicious Remote Desktop Protocol (RDP) authentication activity against a Windows endpoint.

The attack was simulated using Kali Linux Hydra to generate repeated RDP authentication attempts. Windows Security Event Logs were collected and analyzed using Splunk to identify failed authentication patterns, determine the attack source, identify the targeted account, and confirm whether successful access occurred.

The investigation confirmed multiple failed RDP authentication attempts followed by successful authentication from the same source IP. Event ID 4672 confirmed that the authenticated session received special privileges after login.

The incident was classified as a confirmed simulated RDP brute-force attack resulting in successful account access.

---

## Incident Overview

| Field | Details |
|---|---|
| Incident Title | RDP Brute Force Attack Investigation |
| Incident Type | Authentication Attack |
| Severity | Medium |
| Status | Closed |
| Investigation Type | Windows Security Log Analysis |
| Affected System | Windows Endpoint |
| Hostname | DESKTOP-SHNKQPV |
| Target IP | 192.168.109.129 |
| Source IP | 192.168.109.130 |
| Target Account | analyst |
| Attack Tool | Kali Linux Hydra |
| SIEM Platform | Splunk |
| Final Classification | Confirmed RDP Brute Force Attack |

---

## Initial Assessment

Initial analysis identified repeated RDP authentication attempts targeting the Windows endpoint.

The following indicators were identified:

- Multiple failed authentication attempts
- Same source IP repeatedly targeting the endpoint
- Same user account targeted during authentication attempts
- Successful authentication following failed attempts
- Privileged logon activity after successful authentication

Based on the authentication pattern, the activity was escalated for further investigation.

---

## Investigation Methodology

The investigation followed a structured SOC authentication analysis workflow:

- Reviewed Windows Security Event Logs collected in Splunk.
- Identified repeated failed RDP authentication attempts.
- Analyzed source IP and targeted user account.
- Correlated failed and successful authentication events.
- Reviewed Event ID 4672 for privilege assignment.
- Reconstructed the attack timeline.
- Extracted indicators of compromise.
- Mapped observed activity to MITRE ATT&CK techniques.

---

## Evidence Collected

| Evidence Type | Description |
|---|---|
| Windows Security Logs | Authentication events related to RDP activity |
| Event ID 4625 | Failed RDP authentication attempts |
| Event ID 4624 | Successful RDP authentication |
| Event ID 4672 | Special privileges assigned after authentication |
| Splunk Queries | Detection and investigation queries |
| Attack Simulation | Hydra RDP brute-force activity |
| Timeline Data | Reconstructed authentication sequence |

---

## Investigation Findings

### RDP Brute Force Activity

The investigation identified repeated failed RDP authentication attempts originating from:

**Source IP**

```
192.168.109.130
```

**Target Account**

```
analyst
```

#### Observed Activity

| Event ID | Activity |
|---|---|
| 4625 | Multiple failed RDP authentication attempts |
| 4624 | Successful RDP authentication |
| 4672 | Special privileges assigned |

#### Findings

- 20 failed RDP authentication attempts were detected.
- All attempts originated from `192.168.109.130`.
- The `analyst` account was repeatedly targeted.
- The authentication pattern matched password guessing activity.

---

### Successful Authentication Analysis

A successful RDP login was identified after multiple failed authentication attempts.

#### Authentication Details

```
Event ID: 4624
Protocol: RDP
Logon Type: 3
Account: analyst
Source IP: 192.168.109.130
Destination IP: 192.168.109.129
```

Following successful authentication, Event ID 4672 was generated:

```
Event ID: 4672
Description: Special Privileges Assigned
```

This indicated that the authenticated session received special privileges.

---

## Attack Timeline

| Time | Event ID | Activity |
|---|---|---|
| 08:08:42 - 08:23:08 | 4625 | Multiple failed RDP authentication attempts |
| 08:23:11 | 4624 | Successful RDP authentication |
| 08:23:29 | 4672 | Special privileges assigned |

---

## Detection Analysis

### Detection Logic

The attack was detected through Splunk analysis of Windows Security Events.

Detection criteria:

```
Multiple Event ID 4625
+
Same Source IP
+
Same Target User
+
RDP Authentication Activity
```

### SPL Detection Query

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Source_IP, TargetUserName
| where count >= 10
| sort -count
```

---

## Indicators of Compromise (IOC)

| Type | Indicator |
|---|---|
| Source IP | 192.168.109.130 |
| Target IP | 192.168.109.129 |
| Target Account | analyst |
| Protocol | RDP |
| Attack Tool | Hydra |

---

## MITRE ATT&CK Mapping

| Technique ID | Technique Name | Evidence |
|---|---|---|
| T1110.001 | Brute Force: Password Guessing | Hydra generated repeated authentication attempts against the target account |
| T1021.001 | Remote Services: Remote Desktop Protocol | RDP was used to access the Windows endpoint |
| T1078 | Valid Accounts | Successful authentication occurred using valid credentials |

---

## Impact Assessment

The investigation confirmed that the simulated brute-force attack resulted in successful authentication to the Windows endpoint.

The attacker obtained access through valid credentials after repeated authentication attempts. Event ID 4672 indicated that the resulting session received special privileges.

---

## Root Cause Analysis

The incident occurred due to weak authentication controls allowing repeated RDP authentication attempts against a user account.

The attack succeeded because the attacker was able to identify valid credentials through repeated password guessing attempts.

---

## Incident Classification

| Category | Classification |
|---|---|
| Incident Type | RDP Brute Force Attack |
| Attack Method | Password Guessing |
| Severity | Medium |
| Outcome | Successful Authentication |
| Status | Closed |

---

## Recommendations

- Restrict unnecessary RDP exposure.
- Implement account lockout policies.
- Enforce strong password requirements.
- Monitor repeated authentication failures.
- Enable MFA for remote access.
- Review privileged authentication events.

---

## Lessons Learned

- Authentication logs provide valuable visibility into brute-force activity.
- Correlating failed and successful logons helps identify compromised accounts.
- SIEM-based detection improves investigation speed and accuracy.
- Monitoring privileged logon events helps identify suspicious access.

---

## Conclusion

The investigation confirmed a simulated RDP brute-force attack against the Windows endpoint.

Multiple failed authentication attempts from `192.168.109.130` were followed by successful login activity and privileged session assignment.

Splunk analysis enabled identification of the attack source, targeted account, authentication pattern, and complete attack timeline.

**Final Outcome: RDP brute-force attack detected and successful authentication confirmed.**
