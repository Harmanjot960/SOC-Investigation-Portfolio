# Investigation Findings

---

## Attacker Infrastructure Separation

The investigation identified three separate attacker infrastructure roles.

---

### Payload Delivery

```text
phishteam.xyz
```

Used to host and deliver:

```text
update.zip
first.exe
ch.exe
spf.exe
final.exe
```

---

### Command and Control

```text
resolvecyber.xyz
```

Used for:

- Encoded command delivery
- Malware communication
- Remote instructions

---

### Network Pivot Infrastructure

```text
167.71.199.191:8080
```

Used by:

```text
Chisel
```

for reverse SOCKS tunneling.

---

## Important Timeline Finding

Initial assumption:

```text
final.exe extracted from update.zip
```

Investigation conclusion:

```text
final.exe was downloaded later during the WinRM session.
```

Evidence:

- Sysmon Event ID 1
- Sysmon Event ID 11
- Timeline correlation
- PowerShell activity

---

## Final Attack Outcome

The attacker achieved:

```text
Initial Access
        |
        ▼
Code Execution
        |
        ▼
Persistence
        |
        ▼
C2 Communication
        |
        ▼
Remote Access
        |
        ▼
SYSTEM Privileges
        |
        ▼
Administrator Persistence
```
