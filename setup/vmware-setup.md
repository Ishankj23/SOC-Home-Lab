# VMware Workstation Pro Setup

## Overview

This document describes the virtualization platform and environment used to build my SOC Home Lab.

The objective of this lab is to simulate an enterprise Security Operations Center (SOC) environment for learning:

- SIEM Deployment
- Endpoint Monitoring
- Threat Detection
- Threat Hunting
- Incident Response
- Malware Analysis
- Detection Engineering

---

# Host Machine

| Component | Specification |
|------------|---------------|
| Operating System | Windows 11 |
| Processor | Intel Core i5 (11th Gen) |
| Memory | 16 GB DDR4 RAM |
| Storage | 1 TB HDD |
| Hypervisor | VMware Workstation Pro 25 H1 |

---

# Hypervisor

## VMware Workstation Pro

Version

```
VMware Workstation Pro 25 H2u1
```

### Why VMware?

VMware Workstation Pro was selected because it provides:

- Excellent Linux compatibility
- Stable virtual networking
- Snapshot functionality
- Easy virtual machine management
- Widely used in enterprise and cybersecurity labs
- Free for personal use

---

# Virtual Network

Network Mode

```
NAT (VMnet8)
```

Example Network

```
10.10.10.0/24
```

> Example IP addresses are used throughout this repository. No real IP addresses or sensitive information are disclosed.

---

# Virtual Machine Inventory

| VM | Operating System | Purpose |
|----|------------------|---------|
| Ubuntu Server | Ubuntu Server 26.04.4 LTS | Wazuh Server |
| Windows 11 | Windows 11 Pro | Windows Endpoint |
| Ubuntu Desktop | Ubuntu Desktop 26.04.4 LTS | Linux Endpoint |
| Kali Linux | Kali Linux (Latest Stable) | Attack Simulation |

---

# Virtual Machine Configuration

## VM 1 — Ubuntu Server

### Purpose

This server hosts the Wazuh platform.

Installed Components

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

Configuration

| Setting | Value |
|----------|-------|
| CPU | 2 vCPU |
| Memory | 4 GB |
| Storage | 60 GB |
| Network | NAT |

---

## VM 2 — Windows 11

### Purpose

Primary monitored Windows endpoint.

Installed Components

- Wazuh Agent
- Sysmon
- Windows Event Logs

Configuration

| Setting | Value |
|----------|-------|
| CPU | 2 vCPU |
| Memory | 4 GB |
| Storage | 60 GB |
| Network | NAT |

---

## VM 3 — Ubuntu Desktop

### Purpose

Linux endpoint used for monitoring and log collection.

Installed Components

- Wazuh Agent
- Syslog
- Authentication Logs

Configuration

| Setting | Value |
|----------|-------|
| CPU | 2 vCPU |
| Memory | 2 GB |
| Storage | 40 GB |
| Network | NAT |

---

## VM 4 — Kali Linux

### Purpose

Attack simulation and security testing.

Tools

- Nmap
- Hydra
- Nikto
- Burp Suite
- Metasploit Framework
- Netcat

Configuration

| Setting | Value |
|----------|-------|
| CPU | 2 vCPU |
| Memory | 2 GB |
| Storage | 40 GB |
| Network | NAT |

---

# Resource Allocation

The host machine has **16 GB RAM**, therefore not all virtual machines are executed simultaneously.

Typical usage:

## Wazuh Administration

Running VMs

- Ubuntu Server
- Windows 11

Approximate RAM Usage

```
8 GB
```

---

## Linux Monitoring

Running VMs

- Ubuntu Server
- Ubuntu Desktop

Approximate RAM Usage

```
6 GB
```

---

## Attack Simulation

Running VMs

- Ubuntu Server
- Windows 11
- Kali Linux

Approximate RAM Usage

```
10 GB
```

---

## Full Lab

Running VMs

- Ubuntu Server
- Windows 11
- Ubuntu Desktop
- Kali Linux

Approximate RAM Usage

```
12 GB
```

This configuration leaves sufficient memory for the Windows host operating system.

---

# Virtual Network Topology

```
                   VMware Workstation Pro

              NAT Network (Example 10.10.10.0/24)

                           │
     ┌─────────────────────┼─────────────────────┐
     │                     │                     │
     ▼                     ▼                     ▼

Ubuntu Server        Windows 11         Ubuntu Desktop
(Wazuh Server)       (Endpoint)         (Endpoint)

     ▲
     │
     │
Kali Linux
(Attack Machine)
```

---

# VMware Features Used

- NAT Networking
- Snapshots
- Shared Clipboard
- Drag and Drop
- Virtual Disk Management
- ISO Mounting

---

# Snapshot Strategy

| Snapshot | Purpose |
|-----------|---------|
| Clean Install | Fresh operating system installation |
| System Updated | Fully patched operating system |
| Before Wazuh Installation | Rollback point |
| Working SOC Lab | Stable lab configuration |

---

# Network Communication

| Source | Destination | Purpose |
|----------|-------------|---------|
| Windows 11 | Wazuh Server | Endpoint telemetry |
| Ubuntu Desktop | Wazuh Server | Linux log forwarding |
| Kali Linux | Windows / Ubuntu | Attack simulation |
| Analyst Browser | Wazuh Dashboard | Monitoring & Investigation |

---

# Security Considerations

This project intentionally excludes:

- Real IP addresses
- Passwords
- SSH private keys
- API keys
- Client enrollment keys
- Sensitive configuration files

Example values are used wherever applicable.

---

# Lessons Learned

During the setup of this lab I gained hands-on experience with:

- VMware virtual networking
- NAT configuration
- Ubuntu Server administration
- Windows endpoint monitoring
- Wazuh deployment
- Linux firewall (UFW)
- SSH remote management
- Windows Event Logging
- Sysmon integration
- Virtual machine resource optimization

---

# Future Improvements

Planned enhancements include:

- pfSense Firewall
- Windows Server (Active Directory)
- Active Directory integration
- Sigma rule development
- Custom Wazuh detection rules
- Active Response
- Security Onion
- TheHive
- Shuffle SOAR
- OpenVAS Vulnerability Scanner
- Docker-based security services

---

# Repository

This document is part of the **SOC Home Lab** project, where I document the complete lifecycle of building, operating, and improving a practical cybersecurity home lab for blue team and SOC analyst learning.
