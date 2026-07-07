# Sysmon Installation

## Objective
Collect enhanced Windows telemetry.

## Install

```cmd
sysmon64.exe -accepteula -i sysmonconfig.xml
```

## Recommended Configurations

- SwiftOnSecurity
- Olaf Hartong (Advanced)

## Verify

Event Viewer

Microsoft-Windows-Sysmon/Operational

## Investigate

PowerShell

Process Creation

Network Connections
