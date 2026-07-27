# Command Evidence

This document contains attacker commands and observed execution activity.

---

## Follina Execution Chain

Observed execution:

```text
WINWORD.EXE
      |
      ▼
msdt.exe
      |
      ▼
PowerShell.exe
```

---

## PowerShell Activity

Observed:

```text
powershell.exe
```

Activity included:

- Payload retrieval
- Command execution
- Malware staging

Evidence:

- Sysmon Event ID 1
- Command-line telemetry

---

## Payload Downloads

Payload delivery infrastructure:

```text
phishteam.xyz
```

Downloaded files:

```text
update.zip
first.exe
ch.exe
spf.exe
final.exe
```

Evidence:

- Wireshark HTTP requests
- Sysmon network events
- File creation events

---

## Chisel Execution

Binary:

```text
ch.exe
```

Purpose:

```text
Reverse SOCKS tunneling
```

Tunnel endpoint:

```text
167.71.199.191:8080
```

---

## PrintSpoofer Execution

Binary:

```text
spf.exe
```

Purpose:

```text
Privilege escalation to SYSTEM
```

Result:

```text
NT AUTHORITY\SYSTEM
```

---

## Account Creation Commands

Observed activity:

```text
net user shion <password> /add
```

Administrator group addition:

```text
net localgroup administrators shion /add
```

Evidence:

- Sysmon Event ID 1 — Command-line execution
- Windows Security Event ID 4720
- Windows Security Event ID 4732

---

## Windows Service Creation

Observed:

```text
sc.exe create TempestUpdate2 binpath= C:\ProgramData\final.exe start= auto
```

Purpose:

```text
Persistent SYSTEM-level execution
```
