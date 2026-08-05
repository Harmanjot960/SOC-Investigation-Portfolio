# Indicators of Compromise

## Malicious IP Addresses

### 5.252.153.241

Role:
- Malware delivery server
- Command and control communication

Observed Activity:
- Fake Microsoft Teams payload delivery
- PowerShell script download
- Additional malware retrieval


### 82.221.136.26

Role:
- Suspicious binary hosting infrastructure

Observed Activity:
- Associated with suspicious files
- Historical malicious relationships according to VirusTotal


### 45.125.66.32

Role:
- Suspicious TLS communication

Observed Activity:
- Self-signed TLS certificate
- IP-based certificate identity


---

## Domains

### authenticatoor.org

Description:

Typosquatted domain resembling Google Authenticator.

Observed Purpose:

- Fake authentication software distribution
- Malware delivery infrastructure

---

## Malware Artifacts

**Files:**
- TeamViewer.exe
- TV.dll
- Teamviewer_Resource_fr.dll
- pas.ps1
- TeamViewer.lnk

**Persistence Artifact:**
```
TeamViewer.lnk
```

**Observed Location:**
```
%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup
```

**Referenced Executable:**
```
C:\ProgramData\huo\TeamViewer.exe
```

**Observed Activity:**

- Startup shortcut created by the PowerShell payload
- Shortcut configured to execute TeamViewer.exe during user logon
- Persistence behavior identified through malware payload analysis







