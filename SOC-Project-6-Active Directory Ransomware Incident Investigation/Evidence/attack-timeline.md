# Attack Timeline

## Phase 1 — Initial Access

- IIS web server compromise detected.
- Web shell activity identified through IIS logs and Sysmon process creation.
- w3wp.exe spawned cmd.exe to execute attacker commands.

Evidence:
- IIS logs
- Sysmon Event ID 1


## Phase 2 — Credential Access

- The attacker attempted LSASS credential dumping using procdump64.exe.

Evidence:
- Sysmon Event ID 10
- Sysmon Event ID 1


## Phase 3 — Active Directory Discovery

- The attacker enumerated domain users, groups, and computers.

Observed commands:

- net user /domain
- net group "Domain Admins" /domain
- Get-ADUser
- Get-ADComputer


## Phase 4 — Lateral Movement

- The attacker used SMB administrative shares and PsExec for remote execution.

Evidence:

- Event ID 5140
- Event ID 5145
- Event ID 4624
- Event ID 7045
- Sysmon Event ID 1


## Phase 5 — Ransomware Preparation

- The attacker attempted to remove recovery options and forensic evidence.

Evidence:

- Sysmon Event ID 1 — Process Creation

Observed:

- vssadmin delete shadows /all /quiet
- wevtutil cl Security


## Phase 6 — Ransomware Payload Deployment

- The attacker used compromised credentials and WMIC remote process creation to deploy fixer.exe.

Evidence:

- Sysmon Event ID 1
- Sysmon Event ID 11
