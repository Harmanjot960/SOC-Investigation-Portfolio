# Account Activity Analysis

## Overview

Windows Security Event Logs were analyzed to identify account creation and privilege changes performed by the attacker.

The investigation identified unauthorized account creation after SYSTEM-level access was obtained.

---

# Local Account Creation

## Event ID 4720 — User Account Created

The attacker created a new local account:

```text
shion
```

Observed command:

```cmd
net user shion <password> /add
```

This created a persistent user account that could be used for future access.

Evidence:

- Sysmon Event ID 1 — Command execution
- Windows Security Event ID 4720 — User Account Created
- Command execution timeline

MITRE ATT&CK:

```text
T1136.001
Create Account: Local Account
```

---

# Administrator Group Modification

## Event ID 4732 — Member Added to Local Security Group

After creating the account, the attacker added the user to the local Administrators group.

Observed command:

```cmd
net localgroup administrators shion /add
```

Result:

```text
shion
        |
        ▼
Local Administrators Group
```

This provided the attacker with administrative privileges.

- Sysmon Event ID 1 — Command execution
- Windows Security Event ID 4732 — Member Added to Local Security Group
- Timeline correlation

MITRE ATT&CK:

```text
T1098.007
Additional Local or Domain Groups
```

---

# Account Persistence Timeline

```text
SYSTEM Access
        |
        ▼
Create Local Account
        |
        ▼
Add Account to Administrators Group
        |
        ▼
Persistent Administrative Access
```

---

# Investigation Conclusion

The attacker created a new administrator account after achieving elevated privileges.

This activity provided persistence and allowed continued administrative access even after the original malware execution chain ended.
