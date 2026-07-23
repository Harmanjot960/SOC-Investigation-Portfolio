# Attack Timeline

## Incident Timeline

| Time | Activity | Evidence |
|---|---|---|
| 14:44:56 | Initial malicious payload delivery | Suricata alert: Fake Microsoft Teams VBS Payload Inbound |
| 14:45:56 | Communication with malicious infrastructure | HTTP connection to 5.252.153.241 |
| 14:45:58 | PowerShell payload requested | PS1 PowerShell File Request detected |
| 14:47:01 | PowerShell downloader activity | DownloadString and DownloadFile behavior detected |
| 14:47:02 | Executable/DLL payload downloaded | PE EXE or DLL download detected |
| 14:55:08 | TeamViewer-related communication observed | Remote access tooling activity detected |

## Timeline Summary

The infection began with delivery of a malicious payload.
The compromised workstation then communicated with attacker-controlled infrastructure,
downloaded PowerShell content, retrieved additional malware components, and established
a persistence mechanism using fake TeamViewer components.
