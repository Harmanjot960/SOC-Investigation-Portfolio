# Indicators of Compromise (IOCs)

---

## Malicious Domains

### Payload Delivery Infrastructure

```text
phishteam.xyz
```

Purpose:

- Hosted malicious payloads
- Delivered attacker tools

Observed files:

```text
update.zip
first.exe
ch.exe
spf.exe
final.exe
```

---

### Command and Control Infrastructure

```text
resolvecyber.xyz
```

Purpose:

- Delivered encoded commands
- Controlled malware execution

Observed endpoint:

```text
/9ab62b5
```

Communication:

```text
HTTP Port 80
```

---

## Network Indicators

### Chisel Tunnel Endpoint

```text
167.71.199.191:8080
```

Purpose:

```text
Chisel reverse SOCKS tunnel endpoint used for attacker remote access and network pivoting
```

Used for:

- Proxy communication
- Network pivoting
- Remote access

---

## Malicious Files

```text
free_magicules.doc

update.zip

first.exe

ch.exe

spf.exe

final.exe
```

---

## Persistence Indicators

Startup Folder:

```text
C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

Service:

```text
TempestUpdate2
```

Payload:

```text
C:\ProgramData\final.exe
```

---

## Created Account

```text
shion
```

Privilege:

```text
Local Administrator
```
