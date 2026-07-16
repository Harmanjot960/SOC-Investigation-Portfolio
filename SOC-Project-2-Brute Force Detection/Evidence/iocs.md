# Indicators of Compromise (IOCs)

## Network Indicators

| Indicator Type | Value | Description |
|---------------|-------|-------------|
| Source IP | 192.168.109.130 | Kali Linux attacker system |
| Destination IP | 192.168.109.129 | Windows target endpoint |

---

## Account Indicators

| Indicator Type | Value | Description |
|---------------|-------|-------------|
| Target Account | analyst | Account targeted during brute-force attempts |

---

## Attack Indicators

| Indicator | Value |
|----------|-------|
| Protocol | Remote Desktop Protocol (RDP) |
| Attack Type | Brute Force / Password Guessing |
| Tool Used | Kali Linux Hydra |
| Failed Logon Event | Event ID 4625 |
| Successful Logon Event | Event ID 4624 |
| Privileged Logon Event | Event ID 4672 |

---

## MITRE ATT&CK Techniques

| Technique | ID |
|----------|----|
| Remote Services: Remote Desktop Protocol | T1021.001 |
| Brute Force: Password Guessing | T1110.001 |
| Valid Accounts | T1078 |
