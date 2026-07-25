# Windows Event Logs Evidence

This directory contains Windows Security Event Log analysis performed during the endpoint incident response investigation.

Windows Event Logs were analyzed to identify:

- Authentication activity
- Account creation
- Privilege changes
- Administrative group modifications
- Attacker actions after gaining access

## Evidence Sources

```text
Windows Security Event Logs
        |
        ▼
Event ID Analysis
        |
        ▼
Timeline Correlation
        |
        ▼
Attacker Activity Reconstruction
```

## Events Investigated

| Event ID | Description |
|----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4720 | User Account Created |
| 4732 | Member Added to Local Security Group |

## Investigation Findings

The investigation identified that the attacker:

- Successfully accessed the compromised endpoint
- Created a new local user account
- Added the account to the local Administrators group
- Established persistent administrative access

Detailed analysis is available in:

- `security-events.md`
- `account-events.md`
