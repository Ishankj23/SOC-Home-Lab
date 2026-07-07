# Windows 11 Agent Deployment

## Overview

This document describes the deployment of the Wazuh Agent on a Windows 11 virtual machine and its integration with the Wazuh Manager.

The Windows endpoint serves as the primary monitored host within the SOC Home Lab. It generates Windows security events and Sysmon telemetry, which are forwarded to the Wazuh Server for analysis.

---

# Lab Environment

| Component | Value |
|------------|-------|
| Operating System | Windows 11 Pro |
| Endpoint | Windows Workstation |
| Hypervisor | VMware Workstation Pro 25 H1 |
| SIEM | Wazuh |
| Agent | Wazuh Agent |

---

# Objective

The Windows endpoint is used to:

- Collect Windows Event Logs
- Monitor endpoint activity
- Forward security events to Wazuh
- Generate alerts
- Perform threat hunting
- Simulate attacks safely

---

# Network Configuration

Example Network

```
Ubuntu Server (Wazuh)

10.10.10.5

↓

Windows 11 Endpoint

10.10.10.10
```

> Example IP addresses are used throughout this documentation.

---

# Verify Connectivity

Ping the Wazuh Server.

```powershell
ping 10.10.10.5
```

Expected

```
Reply from 10.10.10.5
```

---

# Download Wazuh Agent

Download the latest Windows agent from:

https://packages.wazuh.com/

or directly from the Wazuh Dashboard using the Deploy New Agent wizard.

---

# Install Wazuh Agent

Run the installer as Administrator.

Example

```
WAZUH_MANAGER=10.10.10.5
```

Provide:

- Manager Address
- Agent Name
- Agent Group (Default)

Complete installation.

---

# Start Agent Service

Open Command Prompt as Administrator.

Start service.

```cmd
NET START WazuhSvc
```

Alternative

```powershell
Start-Service WazuhSvc
```

---

# Verify Service

```powershell
Get-Service WazuhSvc
```

Expected

```
Running
```

---

# Verify Connection

On Ubuntu Server

```bash
sudo /var/ossec/bin/agent_control -l
```

Expected

```
ID: 001

Name: WIN11

Status: Active
```

---

# Dashboard Verification

Navigate to

```
Dashboard

↓

Agent Management

↓

Summary
```

Expected

```
Windows 11

Active
```

---

# Install Sysmon

Download

Microsoft Sysinternals Sysmon

Install

```cmd
sysmon64.exe -i
```

Verify

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

---

# Sysmon Integration

Sysmon generates advanced endpoint telemetry.

Examples

- Process Creation
- Network Connections
- Registry Changes
- Driver Loading
- File Creation
- PowerShell Activity

These events are collected by the Wazuh Agent and forwarded to the Wazuh Manager.

---

# Data Flow

```
Windows User Activity

↓

Sysmon

↓

Windows Event Log

↓

Wazuh Agent

↓

Wazuh Manager

↓

Rule Engine

↓

Indexer

↓

Dashboard
```

---

# Verify Event Collection

Generate Windows activity.

Open PowerShell

```powershell
powershell.exe
```

Open Command Prompt

```cmd
cmd.exe
```

Create user

```cmd
net user SOCLab Password123! /add
```

Delete user

```cmd
net user SOCLab /delete
```

---

# Threat Hunting

Open

```
Threat Hunting
```

Useful searches

PowerShell

```
powershell.exe
```

Failed Login

```
4625
```

Successful Login

```
4624
```

User Created

```
4720
```

Command Prompt

```
cmd.exe
```

Agent

```
agent.name:WIN11
```

---

# Event IDs

| Event ID | Description |
|-----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4688 | Process Creation |
| 4720 | User Created |
| 4726 | User Deleted |
| 4728 | User Added to Group |
| 1102 | Audit Log Cleared |

---

# Firewall Considerations

Windows Firewall remained enabled.

Communication required

| Port | Purpose |
|-------|----------|
| 1514 | Agent Communication |
| 1515 | Agent Enrollment |

The Wazuh Manager firewall was configured to allow these ports.

---

# Troubleshooting

## Agent Offline

Checks performed

- Service status
- Manager IP
- Firewall
- Network connectivity

---

## Unable to Register

Verified

- Port 1515
- Manager running
- Correct Manager IP

---

## No Events Received

Verified

- Agent running
- Sysmon installed
- Event Viewer generating logs

---

## Dashboard Shows Never Connected

Verified

```bash
sudo systemctl status wazuh-manager
```

Checked

```bash
agent_control -l
```

---

# Skills Demonstrated

- Endpoint Monitoring
- Windows Administration
- Wazuh Agent Deployment
- Sysmon Deployment
- Windows Event Logging
- Threat Hunting
- Log Collection
- SOC Operations

---

# Lessons Learned

Through this deployment I gained practical experience with:

- Installing endpoint monitoring agents
- Connecting endpoints to a SIEM
- Understanding Windows Event Logs
- Integrating Sysmon with Wazuh
- Verifying endpoint health
- Investigating Windows security events

---

# Future Improvements

Planned enhancements include

- Sysmon SwiftOnSecurity Configuration
- Olaf Hartong Sysmon Modular
- Custom Wazuh Rules
- Sigma Rule Conversion
- PowerShell Detection Rules
- Scheduled Task Monitoring
- Registry Monitoring

---

# Next Step

Continue with

```
05-sysmon-installation.md
```

to configure Sysmon for advanced endpoint telemetry and improve Windows detection capabilities.
