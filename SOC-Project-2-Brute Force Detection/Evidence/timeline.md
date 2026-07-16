# RDP Brute Force Attack Timeline

## Incident Timeline

| Time | Event ID | Description |
|------|----------|-------------|
| 08:08:42 - 08:23:08 | 4625 | Multiple failed RDP authentication attempts |
| 08:23:11 | 4624 | Successful RDP authentication |
| 08:23:29 | 4672 | Special privileges assigned after successful authentication |

---

## Timeline Analysis

The investigation identified a sequence of authentication events consistent with an RDP brute-force attack.

1. Multiple failed logon attempts (Event ID 4625) were generated from the attacker IP:

```
192.168.109.130
```

2. The attempts targeted the Windows user account:

```
analyst
```

3. A successful authentication event (Event ID 4624) occurred after repeated failed attempts.

4. Event ID 4672 was generated after successful authentication, indicating that the authenticated session received special privileges.

---

## Attack Flow

```
RDP Brute Force Attempts
          |
          ▼
Event ID 4625
Failed Logons
          |
          ▼
Valid Credentials Discovered
          |
          ▼
Event ID 4624
Successful Logon
          |
          ▼
Event ID 4672
Special Privileges Assigned
```
