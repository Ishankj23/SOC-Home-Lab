# Windows 11 Wazuh Agent

## Purpose
Connect the Windows endpoint to Wazuh.

## Install

```powershell
msiexec /i wazuh-agent.msi /q WAZUH_MANAGER="<example-manager-ip>"
```

## Start

```powershell
Start-Service wazuhsvc
```

## Validation

Agent appears as **Active** in the dashboard.

## Generate Events

- Failed Login
- PowerShell
- User Creation
