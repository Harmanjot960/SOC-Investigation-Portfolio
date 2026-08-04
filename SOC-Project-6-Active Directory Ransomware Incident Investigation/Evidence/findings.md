# Investigation Findings

## Initial Access

### Finding:

The attacker obtained command execution through a compromised IIS web server.

### Evidence:
```text
w3wp.exe
 |
 ▼
cmd.exe
```

## Credential Dumping

### Finding:

The attacker attempted LSASS credential extraction using procdump64.exe.

### Evidence:
```
TargetImage:
lsass.exe

SourceImage:
procdump64.exe
```

## Lateral Movement

### Finding:

The attacker used SMB administrative shares and PsExec to execute commands remotely.

### Evidence:

- ADMIN$ access
- PSEXESVC service creation
- Remote command execution


## Ransomware Deployment Attempt

### Finding:

The attacker used the compromised account maria.garcia with WMIC remote process creation to deploy fixer.exe across multiple hosts.

### Affected hosts:

- tsm-prod-01
- tsm-prod-02
- tsm-prod-03
- tsm-prod-04
- tsm-prod-05
- tsm-prod-06


### Impact:

Ransomware execution and encryption impact were not confirmed.
