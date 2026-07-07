# Sysmon Installation & Configuration

## Overview

This document describes the installation and configuration of Microsoft Sysmon on the Windows 11 endpoint in my SOC Home Lab.

Sysmon (System Monitor) is a Windows system service that provides detailed endpoint telemetry for process execution, network connections, registry modifications, file creation, and other security-relevant activities.

The generated events are written to the Windows Event Log and collected by the Wazuh Agent for centralized analysis.

---

# Lab Environment

| Component | Value |
|------------|-------|
| Operating System | Windows 11 Pro |
| Endpoint | Windows Workstation |
| Monitoring Tool | Microsoft Sysmon |
| SIEM | Wazuh |
| Hypervisor | VMware Workstation Pro 25 H1 |

---

# Objective

Sysmon was deployed to improve endpoint visibility by monitoring:

- Process Creation
- Network Connections
- File Creation
- Registry Modifications
- Driver Loading
- DNS Queries
- PowerShell Activity
- Command Execution

---

# Why Sysmon?

Windows Security Logs provide useful information but are limited.

Sysmon provides much richer telemetry that is commonly used by SOC analysts during:

- Threat Hunting
- Malware Analysis
- Incident Response
- Digital Forensics
- Detection Engineering

---

# Architecture

```

User Activity
│
▼
Windows 11
│
▼
Sysmon
│
▼
Windows Event Log
│
▼
Wazuh Agent
│
▼
Wazuh Manager
│
▼
Detection Rules
│
▼
Indexer
│
▼
Wazuh Dashboard

```

---

# Download Sysmon

Official Download

https://learn.microsoft.com/sysinternals/downloads/sysmon

Extract

```

Sysmon64.exe

```

---

# Sysmon Configuration

Instead of using the default configuration, I deployed the **SwiftOnSecurity Sysmon Configuration**, which is widely used by the security community and provides a balanced level of logging suitable for SOC monitoring.

Repository

https://github.com/SwiftOnSecurity/sysmon-config

Future work will include evaluating **Olaf Hartong's Sysmon Modular** configuration for more advanced detection engineering.

---

# Installation

Run Command Prompt as Administrator.

Install Sysmon using the configuration file.

```cmd
Sysmon64.exe -accepteula -i sysmonconfig.xml
```

---

# Verify Installation

Check service.

```cmd
sc query Sysmon64
```

Expected

```
STATE : RUNNING
```

---

Verify driver.

```cmd
driverquery | findstr Sysmon
```

---

Verify Event Viewer.

Navigate to

```
Event Viewer

↓

Applications and Services Logs

↓

Microsoft

↓

Windows

↓

Sysmon

↓

Operational
```

Events should begin appearing immediately.

---

# Important Event IDs

| Event ID | Description |
|-----------|-------------|
| 1 | Process Creation |
| 2 | File Creation Time Changed |
| 3 | Network Connection |
| 5 | Process Terminated |
| 6 | Driver Loaded |
| 7 | Image Loaded |
| 8 | CreateRemoteThread |
| 10 | Process Access |
| 11 | File Created |
| 12 | Registry Object Created |
| 13 | Registry Value Modified |
| 15 | File Stream Created |
| 22 | DNS Query |

---

# Verify Wazuh Integration

The Wazuh Agent monitors the Sysmon Operational Event Log.

Configuration file

```
C:\Program Files (x86)\ossec-agent\ossec.conf
```

Verify Sysmon events are being forwarded.

Restart the agent if necessary.

```cmd
NET STOP WazuhSvc
NET START WazuhSvc
```

---

# Generate Test Events

## PowerShell

```powershell
powershell.exe
```

---

## Command Prompt

```cmd
cmd.exe
```

---

## Create User

```cmd
net user SOCLab Password123! /add
```

---

## Delete User

```cmd
net user SOCLab /delete
```

---

## Registry Modification

```cmd
reg add HKCU\Software\SOCLab /v Test /t REG_SZ /d Enabled
```

---

## DNS Query

```powershell
Resolve-DnsName google.com
```

---

## Network Connection

```powershell
curl https://example.com
```

---

# Threat Hunting

Open

```
Dashboard

↓

Threat Hunting
```

Useful searches

PowerShell

```
powershell.exe
```

Network Connections

```
event.code:3
```

DNS

```
event.code:22
```

Process Creation

```
event.code:1
```

Registry

```
event.code:13
```

---

# Example Investigation

Scenario

PowerShell was executed on the Windows endpoint.

Investigation

- Confirm timestamp
- Verify user account
- Review parent process
- Inspect command line
- Determine whether execution was expected

MITRE ATT&CK

```
T1059.001

PowerShell
```

---

# Troubleshooting

## Sysmon Service Not Running

Verify

```cmd
sc query Sysmon64
```

Restart if necessary.

---

## No Events in Event Viewer

Verify installation.

```cmd
Sysmon64.exe -c
```

---

## Wazuh Not Receiving Sysmon Logs

Verify

- Wazuh Agent running
- ossec.conf configuration
- Event Viewer contains Sysmon logs
- Agent connected to Wazuh Manager

---

## Configuration Update

Update Sysmon configuration.

```cmd
Sysmon64.exe -c sysmonconfig.xml
```

---

# Skills Demonstrated

- Endpoint Monitoring
- Windows Internals
- Windows Event Logging
- Sysmon Deployment
- Threat Hunting
- Detection Engineering
- Wazuh Integration
- Incident Investigation

---

# Lessons Learned

This exercise provided practical experience with:

- Windows endpoint telemetry
- Security event generation
- Process monitoring
- Registry monitoring
- Network monitoring
- Threat hunting
- MITRE ATT&CK mapping
- SOC investigation workflows

---

# Future Improvements

Planned enhancements include:

- Olaf Hartong Sysmon Modular
- Sigma Rules
- Custom Wazuh Detection Rules
- PowerShell Detection Engineering
- LOLBins Detection
- Persistence Detection
- Scheduled Task Monitoring
- Ransomware Simulation

---

# Next Step

Proceed to

```
06-ubuntu-agent.md
```

to onboard a Linux endpoint and begin monitoring Linux authentication logs, system logs, and security events alongside Windows telemetry.
