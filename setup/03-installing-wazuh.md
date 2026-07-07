# Installing Wazuh

## Overview
Deploy the Wazuh Manager, Indexer and Dashboard.

## Install

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

## Verify Services

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-dashboard
sudo systemctl status wazuh-indexer
```

## Required Ports

- 22
- 443
- 1514
- 1515
- 55000

## Validation

Open:

https://<example-manager-ip>

Confirm dashboard loads and services are active.
