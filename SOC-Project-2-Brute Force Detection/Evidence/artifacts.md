# Investigation Artifacts

## Collected Evidence

| Artifact | Description |
|----------|-------------|
| Hydra Command Screenshot | Shows RDP brute-force simulation command |
| Hydra Result Screenshot | Shows successful credential discovery |
| Windows Event ID 4625 | Evidence of failed authentication attempts |
| Windows Event ID 4624 | Evidence of successful authentication |
| Windows Event ID 4672 | Evidence of special privileges assigned |
| Splunk Detection Query | Query used to identify brute-force activity |
| Attack Timeline | Chronological reconstruction of events |

---

## Log Sources

### Windows Security Logs

```
WinEventLog:Security
```

Collected Event IDs:

```
4625 - Failed Logon
4624 - Successful Logon
4672 - Special Privileges Assigned
```

---

## Splunk Investigation

Splunk was used to analyze Windows Security Events and identify:

- Source IP
- Target username
- Authentication pattern
- Successful access
- Attack timeline

---

## Evidence Location

Screenshots and supporting investigation evidence are stored in:

```
Screenshots/
Evidence/
SPL-Queries/
```
