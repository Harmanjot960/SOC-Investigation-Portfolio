# MITRE ATT&CK Mapping

This investigation involved a simulated RDP brute-force attack against a Windows endpoint. The observed activities were mapped to the following MITRE ATT&CK techniques.

---

## Technique Mapping

| Activity | MITRE ATT&CK Technique | Technique ID | Description |
|----------|------------------------|--------------|-------------|
| RDP Access | Remote Services: Remote Desktop Protocol | T1021.001 | The attacker used RDP to access the Windows endpoint remotely. |
| Password Guessing | Brute Force: Password Guessing | T1110.001 | The attacker performed repeated authentication attempts to discover valid credentials. |
| Successful Authentication | Valid Accounts | T1078 | Valid credentials were used to successfully authenticate to the Windows system. |

---

# Attack Chain Mapping

```
RDP Brute Force Attempts
            |
            ▼
T1110.001
Brute Force: Password Guessing
            |
            ▼
Valid Credentials Obtained
            |
            ▼
T1078
Valid Accounts
            |
            ▼
Remote Desktop Access
            |
            ▼
T1021.001
Remote Services: Remote Desktop Protocol
```

---

# Evidence Supporting Mapping

## T1110.001 - Brute Force: Password Guessing

Evidence:

- Multiple failed authentication attempts detected
- Windows Security Event ID 4625 generated repeatedly
- Same source IP targeted the same user account

Source:

```
Source IP: 192.168.109.130
Target Account: analyst
Event ID: 4625
```

---

## T1078 - Valid Accounts

Evidence:

- Successful authentication occurred after repeated failed attempts
- Windows Security Event ID 4624 confirmed successful logon

Source:

```
Event ID: 4624
Account: analyst
Logon Type: 3
```

---

## T1021.001 - Remote Services: Remote Desktop Protocol

Evidence:

- Authentication activity was performed through RDP
- Windows endpoint received remote logon attempts

Source:

```
Protocol: Remote Desktop Protocol (RDP)
Destination IP: 192.168.109.129
```

---

# Summary

The simulated attack demonstrated an attacker using brute-force techniques to obtain valid credentials and authenticate to a Windows endpoint through Remote Desktop Protocol.

The investigation identified the attack behavior through Windows Security Events and Splunk analysis, allowing the SOC analyst to map observed activity to MITRE ATT&CK techniques.
