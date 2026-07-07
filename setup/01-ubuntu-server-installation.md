# Ubuntu Server Installation

## Overview
Install Ubuntu Server 26.04.4 LTS as the dedicated Wazuh server.

## VM Specs
- 2 vCPU
- 4 GB RAM
- 60 GB Disk
- NAT Networking

## Steps
1. Boot from ISO.
2. Install OpenSSH Server.
3. Create an admin user.
4. Update packages.

```bash
sudo apt update && sudo apt upgrade -y
hostnamectl
hostname -I
ip a
```

## Configure Firewall

```bash
sudo ufw allow ssh
sudo ufw enable
```

## Verify
- SSH works
- Internet works
- System updated

## Lessons Learned
Ubuntu Server provides a lightweight platform for hosting Wazuh.
