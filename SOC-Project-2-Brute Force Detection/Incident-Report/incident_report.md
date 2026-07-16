# Incident Report: RDP Brute Force Attack Investigation

## Incident Overview

A simulated Remote Desktop Protocol (RDP) brute-force attack was conducted against a Windows endpoint. The attack was performed using Kali Linux Hydra to generate repeated authentication attempts.

Windows Security Logs were collected and analyzed in Splunk to identify malicious authentication activity, determine the source IP, and confirm successful access.

---

# Incident Details

| Field | Details |
|-------|---------|
| Incident Type | RDP Brute Force Attack |
| Severity | Medium |
| Affected System | Windows Endpoint |
| Hostname | DESKTOP-SHNKQPV |
| Target IP | 192.168.109.129 |
| Source IP | 192.168.109.130 |
| Target Account | analyst |
| Attack Tool | Kali Linux Hydra |
| Protocol | Remote Desktop Protocol (RDP) |
| SIEM Platform | Splunk |

---

# Attack Summary

The attacker performed multiple RDP authentication attempts against the Windows endpoint using Hydra.

The attack generated multiple failed authentication events followed by a successful login using valid credentials.

Observed Windows Security Events:

```
Event ID 4625 - Failed Logon
Event ID 4624 - Successful Logon
Event ID 4672 - Special Privileges Assigned
```

---

# Investigation Findings

The SOC investigation identified:

- Multiple failed RDP authentication attempts from `192.168.109.130`
- Repeated targeting of the `analyst` account
- Successful authentication after multiple failed attempts
- Privileged session activity after successful authentication

---

# Timeline

| Time | Event ID | Activity |
|------|----------|----------|
| 08:08:42 - 08:23:08 | 4625 | Multiple failed RDP authentication attempts |
| 08:23:11 | 4624 | Successful RDP authentication |
| 08:23:29 | 4672 | Special privileges assigned |

---

# Detection Method

The attack was detected using Splunk analysis of Windows Security Events.

Detection logic:

```
Multiple Event ID 4625
+
Same Source IP
+
Same Target User
+
RDP Authentication Activity
```

Splunk query:

```spl
index=windows sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Source_IP, TargetUserName
| where count >= 10
| sort -count
```

---

# Indicators of Compromise

| Indicator Type | Value |
|----------------|-------|
| Attacker IP | 192.168.109.130 |
| Target IP | 192.168.109.129 |
| Target Username | analyst |
| Protocol | RDP |
| Attack Tool | Hydra |

---

# MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| Brute Force: Password Guessing | T1110.001 |
| Valid Accounts | T1078 |
| Remote Services: Remote Desktop Protocol | T1021.001 |

---

# Response Recommendations

Recommended SOC actions:

- Block or restrict the source IP if malicious activity is confirmed
- Review additional authentication activity from the source IP
- Enforce strong password policies
- Enable account lockout controls
- Restrict unnecessary RDP exposure
- Monitor privileged authentication events

---

# Conclusion

The investigation confirmed a simulated RDP brute-force attack against the Windows endpoint.

The attacker generated repeated failed authentication attempts before successfully authenticating with valid credentials. Splunk analysis of Windows Security Events enabled identification of the attack source, targeted account, authentication pattern, and complete attack timeline.

The incident demonstrates the process of detecting, investigating, and documenting an authentication-based attack in a SOC environment.
