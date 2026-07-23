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


### Additional Related Domains

- teamviewer.com activity observed during analysis

---

## Malware Artifacts

**Files:**
- TeamViewer.exe
- TV.dll
- Teamviewer_Resource_fr.dll
- pas.ps1
- TeamViewer.lnk

**Persistence Location:**
```
C:\Users\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```
