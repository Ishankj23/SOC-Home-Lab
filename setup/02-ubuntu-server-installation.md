# Ubuntu Server Installation

## Overview

This document describes the installation and initial configuration of the Ubuntu Server virtual machine used to host the Wazuh SIEM platform in my SOC Home Lab.

The Ubuntu Server acts as the central monitoring server responsible for:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Agent Management
- Threat Detection
- Log Collection

---

# System Information

| Component | Value |
|------------|-------|
| Operating System | Ubuntu Server 26.04.4 LTS |
| Hostname | wazuh-server |
| Hypervisor | VMware Workstation Pro 25 H1 |
| CPU | 2 vCPU |
| RAM | 4 GB |
| Storage | 60 GB |
| Network | NAT (VMnet8) |

---

# Why Ubuntu Server?

Ubuntu Server was selected because:

- Lightweight operating system
- No graphical interface (reduced resource usage)
- Enterprise-grade stability
- Excellent compatibility with Wazuh
- Long-Term Support (LTS)
- Large community and documentation

---

# Virtual Machine Creation

VM Specifications

| Setting | Value |
|----------|-------|
| Guest OS | Ubuntu Server 64-bit |
| CPU | 2 vCPU |
| RAM | 4096 MB |
| Disk | 60 GB |
| Firmware | UEFI |
| Network Adapter | NAT |

---

# Operating System Installation

Installation Type

```
Ubuntu Server 26.04.4 LTS
```

Installation Options

- OpenSSH Server
- Standard Ubuntu utilities

Filesystem

```
Use Entire Disk
```

No LVM or encryption was enabled because this is a learning environment.

---

# Initial Configuration

## Hostname

```
wazuh-server
```

---

## User Account

Example

```
Username:
socadmin
```

> Actual usernames and passwords are omitted.

---

# Verify Installation

Display operating system information

```bash
lsb_release -a
```

Check kernel version

```bash
uname -r
```

Check hostname

```bash
hostname
```

Verify logged in user

```bash
whoami
```

---

# System Update

Update package repository

```bash
sudo apt update
```

Upgrade installed packages

```bash
sudo apt upgrade -y
```

Remove unnecessary packages

```bash
sudo apt autoremove -y
```

---

# Install Common Utilities

```bash
sudo apt install curl wget unzip vim git net-tools htop -y
```

These utilities are used for:

- Downloading packages
- Editing configuration files
- Monitoring system performance
- Troubleshooting

---

# Network Configuration

Display IP Address

```bash
hostname -I
```

Alternative

```bash
ip a
```

Example

```
10.10.10.5
```

> Example IP address only.

---

# Connectivity Tests

Check Internet

```bash
ping -c 4 google.com
```

Check DNS Resolution

```bash
nslookup google.com
```

Check Gateway

```bash
ip route
```

---

# Enable SSH

Verify SSH status

```bash
sudo systemctl status ssh
```

Enable SSH

```bash
sudo systemctl enable ssh
```

Start SSH

```bash
sudo systemctl start ssh
```

---

# Remote Administration

From Windows host

```powershell
ssh socadmin@10.10.10.5
```

Example only.

SSH allows remote management without opening the VMware console.

---

# Configure Firewall (UFW)

Enable required services

```bash
sudo ufw allow ssh
sudo ufw allow https
sudo ufw allow 55000/tcp
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp
```

Enable firewall

```bash
sudo ufw enable
```

Verify rules

```bash
sudo ufw status
```

---

# Time Synchronization

Verify timezone

```bash
timedatectl
```

Correct time is essential because SIEM platforms rely on accurate timestamps for log correlation.

---

# System Monitoring

Memory Usage

```bash
free -h
```

CPU Information

```bash
lscpu
```

Disk Usage

```bash
df -h
```

Running Services

```bash
systemctl list-units --type=service
```

---

# VMware Optimizations

The following VMware features are enabled:

- Shared Clipboard
- Drag and Drop
- Snapshot Support
- NAT Networking

VMware Tools are installed to improve:

- Performance
- Time synchronization
- Clipboard integration
- Shutdown handling

---

# Snapshot

After completing the base installation, create a VMware snapshot.

Suggested name

```
Ubuntu Server - Clean Install
```

This allows quick rollback before installing Wazuh.

---

# Lessons Learned

During the Ubuntu Server installation I learned:

- Linux server installation
- SSH administration
- Basic Linux networking
- Firewall configuration using UFW
- System updates
- Service management
- VMware virtual networking
- Remote administration

---

# Next Step

Proceed to:

```
03-wazuh-installation.md
```

where the Wazuh platform (Manager, Indexer, and Dashboard) will be installed and configured.

---

# References

- Ubuntu Server Documentation
- Wazuh Documentation
- VMware Workstation Pro Documentation
