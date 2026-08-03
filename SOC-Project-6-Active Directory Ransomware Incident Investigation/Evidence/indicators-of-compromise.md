# Indicators of Compromise

## Files

C:\Windows\fixer.exe

C:\Windows\Temp\procdump64.exe


## Processes

updater.exe

procdump64.exe

wmic.exe

PsExec.exe

fixer.exe


## Commands

wmic process call create

vssadmin delete shadows

wevtutil cl Security


## Hosts

tsm-prod-01

tsm-prod-02

tsm-prod-03

tsm-prod-04

tsm-prod-05

tsm-prod-06


## Network Activity

Source:

10.5.50.12

Observed:

- SMB administrative share access
- Remote service execution
