# Attack Timeline

## Phase 1 — Initial Access

- IIS web server compromise detected.
- Web shell activity identified through IIS logs and Sysmon process creation.
- w3wp.exe spawned cmd.exe to execute attacker commands.

**Evidence:**

- IIS logs
- Sysmon Event ID 1


## Phase 2 — Credential Access

- The attacker attempted LSASS credential dumping using procdump64.exe.

**Evidence:**

- Sysmon Event ID 10
- Sysmon Event ID 1

## Phase 3 — Privileged Credential Logon

- The attacker authenticated using the compromised account luke.sullivan after credential dumping activity.

**Evidence:**

- Event ID 4624 — Successful Logon
- Event ID 4672 — Special Privileges Assigned to New Logon

## Phase 4 — Active Directory Discovery

- The attacker enumerated domain users, groups, and computers.

**Observed commands:**

- net user /domain
- net group "Domain Admins" /domain
- Get-ADUser
- Get-ADComputer

## Phase 5 — Lateral Movement

- The attacker used SMB administrative shares and PsExec for remote execution.

**Evidence:**

- Event ID 5140
- Event ID 5145
- Event ID 4624
- Event ID 7045
- Sysmon Event ID 1

## Phase 6 — Ransomware Preparation

- The attacker attempted to remove recovery options and forensic evidence.

**Evidence:**

- Sysmon Event ID 1 — Process Creation

**Observed commands:**

- vssadmin delete shadows /all /quiet
- wevtutil cl Security

## Phase 7 — Ransomware Payload Deployment

- The attacker used the compromised account maria.garcia and WMIC remote process creation to deploy fixer.exe across multiple systems.

**Evidence:**

- Sysmon Event ID 1
- Sysmon Event ID 11
