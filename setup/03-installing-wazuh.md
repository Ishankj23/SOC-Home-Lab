# Wazuh Installation

## Overview

This document describes the deployment and configuration of the Wazuh SIEM platform in my SOC Home Lab.

Wazuh provides centralized log collection, endpoint monitoring, threat detection, vulnerability assessment, file integrity monitoring, and security event correlation.

The Wazuh server is installed on an Ubuntu Server virtual machine running inside VMware Workstation Pro.

---

# Lab Environment

| Component | Value |
|------------|-------|
| SIEM Platform | Wazuh |
| Operating System | Ubuntu Server 26.04.4 LTS |
| Hypervisor | VMware Workstation Pro 25 H1 |
| Installation Type | All-in-One |
| Deployment | Single Node |

---

# Wazuh Architecture

This deployment consists of the following components.

## Wazuh Manager

Responsible for:

- Agent management
- Rule evaluation
- Log analysis
- Alert generation
- Active Response

---

## Wazuh Indexer

Responsible for:

- Event indexing
- Event storage
- Search engine

---

## Wazuh Dashboard

Provides:

- Web interface
- Threat Hunting
- Agent Management
- Dashboards
- Reporting

---

# Verify Internet Connectivity

Before installation, verify internet access.

```bash
ping -c 4 google.com
```

Check DNS resolution.

```bash
nslookup packages.wazuh.com
```

---

# Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

---

# Download Installation Script

Download the latest installation script.

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
```

Verify the script exists.

```bash
ls
```

Example output

```
wazuh-install.sh
```

---

# Make Script Executable

```bash
chmod +x wazuh-install.sh
```

---

# Install Wazuh

Run the installer.

```bash
sudo ./wazuh-install.sh -a
```

The **-a** option performs an All-in-One installation including:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

---

# Installation Process

The installer automatically:

- Installs required packages
- Generates certificates
- Configures OpenSearch
- Configures Filebeat
- Starts services
- Generates administrator credentials

---

# Save Credentials

The installer generates dashboard credentials.

Example

```
Username:
admin

Password:
************
```

Store credentials securely.

---

# Verify Services

Manager

```bash
sudo systemctl status wazuh-manager
```

Indexer

```bash
sudo systemctl status wazuh-indexer
```

Dashboard

```bash
sudo systemctl status wazuh-dashboard
```

Expected

```
active (running)
```

---

# Enable Services

```bash
sudo systemctl enable wazuh-manager
sudo systemctl enable wazuh-indexer
sudo systemctl enable wazuh-dashboard
```

---

# Verify Listening Ports

```bash
sudo ss -tulpn
```

Expected important ports

| Port | Purpose |
|-------|----------|
| 443 | Wazuh Dashboard |
| 55000 | Wazuh API |
| 1514 | Agent Events |
| 1515 | Agent Enrollment |
| 9200 | OpenSearch |

---

# Configure Firewall

Allow SSH

```bash
sudo ufw allow ssh
```

Allow Dashboard

```bash
sudo ufw allow https
```

Allow Wazuh API

```bash
sudo ufw allow 55000/tcp
```

Allow Agent Communication

```bash
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp
```

Reload firewall

```bash
sudo ufw reload
```

---

# Access Dashboard

Open browser.

```
https://10.10.10.5
```

Example only.

Login using

```
Username

admin
```

```
Password

Generated during installation
```

---

# Verify API

```bash
curl -k https://localhost:55000
```

Expected

```
Unauthorized
```

This confirms the API is reachable.

---

# Verify Installed Version

```bash
/var/ossec/bin/wazuh-control info
```

or

```bash
sudo wazuh-managerd -V
```

---

# Verify Agent Status

Initially

```
No agents connected
```

After endpoint deployment

```
Windows 11

Active
```

```
Ubuntu Desktop

Active
```

---

# Directory Structure

```
/var/ossec/

├── etc/
├── logs/
├── queue/
├── ruleset/
├── active-response/
├── integrations/
└── framework/
```

---

# Useful Commands

Restart Manager

```bash
sudo systemctl restart wazuh-manager
```

Restart Dashboard

```bash
sudo systemctl restart wazuh-dashboard
```

Restart Indexer

```bash
sudo systemctl restart wazuh-indexer
```

Restart All

```bash
sudo systemctl restart wazuh-manager wazuh-dashboard wazuh-indexer
```

---

# Logs

Manager

```bash
sudo tail -f /var/ossec/logs/ossec.log
```

Dashboard

```bash
sudo journalctl -u wazuh-dashboard -f
```

Indexer

```bash
sudo journalctl -u wazuh-indexer -f
```

---

# Troubleshooting

## TLS Error

Issue

```
curl: (35) TLS connect error
```

Resolution

- Verified internet connectivity
- Checked system time
- Updated CA certificates
- Confirmed access to packages.wazuh.com

---

## Dashboard Not Accessible

Resolution

Verified

- Dashboard service
- HTTPS port
- Firewall rules
- VM network configuration

---

## SSH Connection Failed

Resolution

- Enabled SSH service
- Allowed SSH through UFW
- Verified VM IP address

---

## Agent Enrollment Failed

Resolution

- Opened ports 1514 and 1515
- Verified Manager IP
- Confirmed Wazuh Manager service

---

# Skills Demonstrated

- SIEM Deployment
- Linux Administration
- Wazuh Installation
- Firewall Configuration
- HTTPS Configuration
- Service Management
- Troubleshooting
- Security Monitoring
- Virtual Networking

---

# Lessons Learned

Through this deployment I gained hands-on experience with:

- Deploying an enterprise SIEM
- Configuring Linux services
- Managing firewall rules
- Troubleshooting connectivity
- Understanding SIEM architecture
- Preparing infrastructure for endpoint onboarding

---

# Next Step

Proceed to

```
04-windows-agent.md
```

to connect a Windows endpoint, install Sysmon, and begin collecting endpoint telemetry.
