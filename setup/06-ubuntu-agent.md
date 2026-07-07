# Ubuntu Desktop Agent Deployment

## Overview

This document describes the deployment of the Wazuh Agent on an Ubuntu Desktop virtual machine within my SOC Home Lab.

The Ubuntu Desktop endpoint simulates a Linux workstation commonly found in enterprise environments. It forwards authentication logs, system logs, and security events to the Wazuh Manager for centralized monitoring and threat detection.

---

# Lab Environment

| Component | Value |
|------------|-------|
| Operating System | Ubuntu Desktop 26.04.4 LTS |
| Hypervisor | VMware Workstation Pro 25 H1 |
| Endpoint Type | Linux Workstation |
| SIEM | Wazuh |
| Agent | Wazuh Agent |

---

# Objective

The Linux endpoint is used to:

- Monitor authentication activity
- Forward system logs
- Detect privilege escalation
- Monitor SSH activity
- Detect brute-force attacks
- Generate Linux security events
- Practice threat hunting

---

# Network Configuration

Example Network

```
Ubuntu Server (Wazuh)

10.10.10.5

↓

Ubuntu Desktop

10.10.10.20
```

> Example IP addresses are used throughout this repository.

---

# Verify Connectivity

Ping the Wazuh Server.

```bash
ping -c 4 10.10.10.5
```

Expected

```
64 bytes from 10.10.10.5
```

---

# Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

---

# Download Wazuh Agent

```bash
curl -sO https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_VERSION_amd64.deb
```

> Replace VERSION with the latest supported version.

---

# Install Wazuh Agent

```bash
sudo WAZUH_MANAGER="10.10.10.5" dpkg -i wazuh-agent_VERSION_amd64.deb
```

---

# Enable Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
```

---

# Start Agent

```bash
sudo systemctl start wazuh-agent
```

---

# Verify Status

```bash
sudo systemctl status wazuh-agent
```

Expected

```
active (running)
```

---

# Verify Registration

On Ubuntu Server

```bash
sudo /var/ossec/bin/agent_control -l
```

Expected

```
ID: 002

Name: ubuntu-desktop

Status: Active
```

---

# Verify Dashboard

Navigate

```
Dashboard

↓

Agent Management

↓

Summary
```

Expected

```
Ubuntu Desktop

Active
```

---

# Linux Logs Collected

Examples

- Authentication Logs
- Syslog
- Kernel Messages
- Sudo Activity
- SSH Events
- User Management
- Package Installation

---

# Important Log Files

Authentication

```bash
/var/log/auth.log
```

System Log

```bash
/var/log/syslog
```

Kernel

```bash
/var/log/kern.log
```

---

# Verify Agent Configuration

Configuration file

```bash
/var/ossec/etc/ossec.conf
```

Restart agent after changes.

```bash
sudo systemctl restart wazuh-agent
```

---

# Generate Security Events

## Failed SSH Login

From Kali Linux

```bash
ssh fakeuser@10.10.10.20
```

Enter an incorrect password several times.

Expected Detection

- Failed SSH Authentication
- Brute-force Attempts

---

## Successful SSH Login

```bash
ssh username@10.10.10.20
```

---

## Sudo Activity

```bash
sudo apt update
```

Expected Detection

Privilege escalation event.

---

## User Creation

```bash
sudo adduser soclab
```

Delete user

```bash
sudo deluser soclab
```

---

## Failed Sudo

```bash
sudo ls
```

Enter an incorrect password multiple times.

---

## File Creation

```bash
touch testfile.txt
```

---

## Install Software

```bash
sudo apt install tree
```

---

# Threat Hunting

Open

```
Threat Hunting
```

Useful searches

SSH

```
sshd
```

Authentication

```
authentication
```

Sudo

```
sudo
```

Linux Agent

```
agent.name:ubuntu-desktop
```

---

# Investigation Example

Scenario

Multiple failed SSH login attempts detected.

Investigation

- Source IP
- Username
- Number of attempts
- Timestamp
- Destination host

MITRE ATT&CK

```
T1110

Brute Force
```

---

# Troubleshooting

## Agent Offline

Verify

```bash
sudo systemctl status wazuh-agent
```

---

## Cannot Reach Manager

Check

```bash
ping 10.10.10.5
```

Verify firewall.

---

## Agent Not Appearing

Restart

```bash
sudo systemctl restart wazuh-agent
```

Check

```bash
sudo /var/ossec/bin/agent_control -l
```

---

## Firewall Issues

Allow communication.

```bash
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp
```

---

# Skills Demonstrated

- Linux Administration
- Endpoint Monitoring
- Wazuh Agent Deployment
- SSH Monitoring
- Authentication Monitoring
- Threat Hunting
- Log Analysis
- Incident Investigation

---

# Lessons Learned

This exercise provided experience with:

- Linux endpoint onboarding
- Authentication monitoring
- SSH security
- Linux log analysis
- Agent troubleshooting
- Threat hunting
- Enterprise endpoint monitoring

---

# Future Improvements

Planned enhancements include:

- Auditd Integration
- File Integrity Monitoring (FIM)
- Rootcheck
- Vulnerability Detection
- Custom Linux Detection Rules
- Active Response
- Docker Monitoring

---

# Validation Checklist

| Test | Expected Result | Status |
|------|-----------------|--------|
| Agent Connected | Ubuntu Desktop Active | ✅ |
| SSH Login | Logged | ✅ |
| Failed SSH | Detected | ✅ |
| Sudo Command | Logged | ✅ |
| User Created | Logged | ✅ |
| Package Installed | Logged | ✅ |

---

# Next Step

Proceed to

```
07-kali-linux.md
```

to configure the attack machine used for vulnerability assessment, network scanning, brute-force simulations, and security validation against monitored endpoints.
