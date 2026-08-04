# Ransomware Preparation Activity

Before payload deployment, the attacker attempted to reduce recovery capability and remove forensic evidence.

## Shadow Copy Deletion

### Command:

vssadmin delete shadows /all /quiet


### Evidence:

- Sysmon Event ID 1
- Host: DC-01


## Windows Event Log Clearing

### Command:

wevtutil cl Security


### Evidence:

- Sysmon Event ID 1
- Host: DC-01


## MITRE ATT&CK

- T1490 — Inhibit System Recovery
- T1070.001 — Clear Windows Event Logs
