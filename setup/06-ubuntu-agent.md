# Ubuntu Desktop Agent

## Install

```bash
sudo apt install wazuh-agent
```

Configure manager in:

/var/ossec/etc/ossec.conf

Enable:

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

## Validate

Agent becomes Active in Wazuh.
