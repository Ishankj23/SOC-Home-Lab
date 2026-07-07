# Kali Linux Setup

## Overview

This document describes the installation and configuration of the Kali Linux virtual machine used within my SOC Home Lab.

Kali Linux serves as the attack simulation workstation for validating Wazuh detections, performing security assessments, and generating realistic security events against lab endpoints.

> **Disclaimer**
>
> All testing described in this document is performed exclusively within my isolated home lab environment. No attacks are directed toward public systems or networks.

---

# Lab Environment

| Component | Value |
|------------|-------|
| Operating System | Kali Linux (Latest Stable Release) |
| Hypervisor | VMware Workstation Pro 25 H1 |
| Role | Attack Simulation |
| Network | NAT (VMnet8) |

---

# Objective

The Kali Linux virtual machine is used to:

- Simulate attacker behavior
- Generate security events
- Validate Wazuh detections
- Perform vulnerability assessments
- Practice penetration testing techniques
- Learn attack methodologies
- Improve blue-team detection capabilities

---

# Why Kali Linux?

Kali Linux was selected because it includes hundreds of security tools commonly used by:

- Penetration Testers
- Red Teams
- Blue Teams
- Security Researchers
- SOC Analysts

Using Kali allows me to generate realistic attack traffic and verify whether my SIEM detects it correctly.

---

# Virtual Machine Configuration

| Setting | Value |
|----------|-------|
| Operating System | Kali Linux |
| CPU | 2 vCPU |
| Memory | 2 GB |
| Storage | 40 GB |
| Network Adapter | NAT |

---

# Network Configuration

Example Network

```
Ubuntu Server (Wazuh)
10.10.10.5

Windows 11
10.10.10.10

Ubuntu Desktop
10.10.10.20

Kali Linux
10.10.10.30
```

> Example IP addresses are used throughout this repository.

---

# Verify Connectivity

Check network connectivity.

```bash
ping -c 4 10.10.10.5
```

Ping Windows endpoint.

```bash
ping -c 4 10.10.10.10
```

Ping Ubuntu Desktop.

```bash
ping -c 4 10.10.10.20
```

---

# Update Kali

```bash
sudo apt update
sudo apt full-upgrade -y
```

---

# Verify Installed Tools

```bash
which nmap
```

```bash
which hydra
```

```bash
which nikto
```

```bash
which burpsuite
```

```bash
which msfconsole
```

---

# Primary Tools

| Tool | Purpose |
|------|----------|
| Nmap | Network Discovery |
| Hydra | Password Auditing |
| Nikto | Web Server Scanning |
| Burp Suite | Web Security Testing |
| Metasploit Framework | Controlled Exploitation |
| Netcat | Network Testing |
| Tcpdump | Packet Capture |
| Wireshark | Traffic Analysis |

---

# Attack Simulation Workflow

```
Kali Linux

↓

Generate Activity

↓

Windows / Ubuntu

↓

Wazuh Agent

↓

Wazuh Manager

↓

Detection Rules

↓

Indexer

↓

Dashboard

↓

Threat Hunting
```

---

# Lab Exercises

## Network Discovery

```bash
nmap 10.10.10.0/24
```

Purpose

- Host Discovery
- Port Enumeration

Expected

Wazuh detects scanning activity.

---

## Service Detection

```bash
nmap -sV 10.10.10.10
```

---

## OS Detection

```bash
sudo nmap -O 10.10.10.10
```

---

## Aggressive Scan

```bash
sudo nmap -A 10.10.10.10
```

---

## SSH Brute Force (Lab Only)

```bash
hydra -l testuser -P passwords.txt ssh://10.10.10.20
```

Expected

- Failed SSH Logins
- Brute-force Detection

---

## Web Server Scan

```bash
nikto -h http://10.10.10.10
```

---

## Banner Grabbing

```bash
nc 10.10.10.10 80
```

---

## Packet Capture

```bash
sudo tcpdump -i eth0
```

---

# Threat Hunting

Navigate to

```
Dashboard

↓

Threat Hunting
```

Search for

```
nmap
```

```
hydra
```

```
sshd
```

```
failed login
```

```
agent.name:ubuntu-desktop
```

```
agent.name:WIN11
```

---

# Validation

| Activity | Expected Detection |
|-----------|-------------------|
| Ping Sweep | Network Activity |
| Nmap Scan | Port Scan |
| SSH Brute Force | Failed Authentication |
| Web Scan | HTTP Requests |
| Netcat | Network Connection |

---

# Skills Demonstrated

- Network Enumeration
- Vulnerability Assessment
- Security Validation
- Threat Simulation
- SOC Detection Testing
- Linux Administration
- TCP/IP Networking
- Threat Hunting

---

# Lessons Learned

Through these exercises I learned:

- How attackers enumerate systems
- How Wazuh detects reconnaissance
- How endpoint logs differ from network activity
- How to validate SIEM detections
- How to investigate generated alerts

---

# Troubleshooting

## Cannot Reach Target

Verify

```bash
ip a
```

Check

```bash
ping TARGET_IP
```

Verify VMware NAT networking.

---

## Nmap Returns No Hosts

Check

- Windows Firewall
- Ubuntu Firewall (UFW)
- VM Network Adapter

---

## No Wazuh Alerts

Verify

- Wazuh Agent Running
- Manager Online
- Threat Hunting Filters
- Agent Connectivity

---

# Security Notice

This Kali Linux VM is used **only** within my isolated SOC Home Lab.

No testing is performed against systems without authorization.

---

# Future Improvements

Planned additions include:

- OWASP Juice Shop
- DVWA
- Metasploitable 2
- Windows Server (Active Directory)
- pfSense Firewall
- Atomic Red Team
- Caldera
- Detection Engineering Labs
- Purple Team Exercises

---

# Validation Checklist

| Test | Expected Result | Status |
|------|-----------------|--------|
| Kali Installed | ✅ | Complete |
| Network Connectivity | ✅ | Complete |
| Nmap Scan | ✅ | Complete |
| SSH Brute Force | ✅ | Complete |
| Wazuh Detection | ✅ | Complete |
| Threat Hunting | ✅ | Complete |

---

# Next Phase

With the lab fully operational, the next stage is to build practical detection and investigation scenarios, including:

- Windows Event Investigation
- Linux Authentication Monitoring
- PowerShell Detection
- SSH Brute Force Detection
- Nmap Detection
- File Integrity Monitoring
- Vulnerability Detection
- MITRE ATT&CK Mapping
- Custom Wazuh Rules
- Incident Response Documentation
