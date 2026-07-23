# Incident Report

## Incident Overview

## Executive Summary

## Incident Details

Date:
2025-01-22

Affected Host:
10.1.17.215

Domain:
BLUEMOONTUESDAY


## Initial Alert / User Report

A user reported downloading a suspicious application after searching for Google Authenticator.

Network traffic analysis confirmed malware infection.


## Investigation Methodology

Tools:

- Wireshark
- Suricata
- VirusTotal


## Attack Analysis

### Initial Access

User downloaded malicious software disguised as Google Authenticator.


### Malware Communication

Identified communication:

Victim:
10.1.17.215

Destination:
5.252.153.241

Protocol:
HTTP


### Payload Delivery

Observed:

- PowerShell script download
- Executable download
- DLL download


### Persistence

Observed:

- Fake TeamViewer components
- Startup folder persistence behavior


## Indicators of Compromise

IPs:

5.252.153.241
82.221.136.26
45.125.66.32


Domain:

authenticatoor.org


## MITRE ATT&CK Mapping


## Impact Assessment


## Recommendations


## Conclusion
