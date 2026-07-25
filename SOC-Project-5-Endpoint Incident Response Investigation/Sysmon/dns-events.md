# Sysmon Event ID 22 — DNS Query

Sysmon Event ID 22 was used to identify domain resolution activity related to attacker infrastructure.

---

# Malicious Domain Resolution

The compromised endpoint resolved:

```text
phishteam.xyz
```

Purpose:

```text
Payload delivery infrastructure
```

---

The compromised endpoint also resolved:

```text
resolvecyber.xyz
```

Purpose:

```text
Command and Control infrastructure
```

---

# Domain Investigation

The DNS activity was correlated with:

```text
DNS Query
        +
HTTP Traffic
        +
Process Execution
```

This confirmed that malicious processes were communicating with attacker-controlled infrastructure.

---

# Related MITRE ATT&CK Techniques

```text
T1071.001 - Web Protocols

T1105 - Ingress Tool Transfer
```

---

# Evidence Sources

- Sysmon Event ID 22
- Wireshark DNS analysis
- Network timeline correlation
