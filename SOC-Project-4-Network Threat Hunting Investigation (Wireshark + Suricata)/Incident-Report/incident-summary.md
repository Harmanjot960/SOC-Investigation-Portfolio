### Incident Summary

| Incident Type       | Host        | Domain           | Tools                  | Severity | Status    |
| ------------------- | ----------- | ---------------- | ---------------------- | -------- | --------- |
| Malware Infection   | 10.1.17.215 | BLUEMOONTUESDAY  | Wireshark + Suricata   | High     | CONFIRMED |

---

#### Key Findings

| Initial Access                 | Malware Delivery        | C2 Communication | Payloads              | Persistence              |
| ----------------------------- | ----------------------- | ---------------- | --------------------- | ------------------------ |
| Fake Google Authenticator App | PowerShell Payload      | 5.252.153.241    | TeamViewer Components | Startup Shortcut         |

---

#### Network Evidence

| Malicious Domain    | Payload Server | Suspicious TLS | Payload Hosting |
| ------------------- | -------------- | -------------- | --------------- |
| authenticatoor.org  | 5.252.153.241 | 45.125.66.32   | 82.221.136.26   |

---

#### Final Assessment

Confirmed malware infection involving:

- Malicious application download
- PowerShell payload execution
- C2 communication
- Additional malware retrieval
- Persistence attempt

Evidence collected using Wireshark packet analysis and Suricata IDS detection. 

