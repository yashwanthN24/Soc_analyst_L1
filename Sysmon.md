# Sysmon: Windows Endpoint Monitoring and SIEM Log Forwarding

## Overview

This repository contains my learning notes on Sysmon, Windows Event Logs, Event Viewer, and forwarding Sysmon logs to a SIEM.

Sysmon helps security teams collect detailed endpoint activity from Windows devices. It is commonly used by SOC analysts, threat hunters, and blue team professionals.

---

## What Is Sysmon?

Sysmon stands for **System Monitor**.

It is a Windows system-monitoring tool from Microsoft Sysinternals. Sysmon runs as a Windows service and records detailed security-relevant activity from a Windows device.

In cybersecurity, a laptop, desktop, server, or virtual machine is called an **endpoint**.

Sysmon creates detailed events and writes them into the Windows Event Log.

---

## Sysmon and Event Viewer

Sysmon does not replace Event Viewer.

Event Viewer is a Windows application used to view logs. Sysmon is a tool that creates additional, detailed security events that Event Viewer can display.

Sysmon logs are usually available at:

```text
Applications and Services Logs
└── Microsoft
    └── Windows
        └── Sysmon
            └── Operational
```

---

## Why Install Sysmon When Windows Already Has Event Logs?

Windows already records useful logs such as:

* Successful and failed logons
* Account changes
* Service changes
* System events
* Application events
* Security audit events

However, Windows logs may not provide enough detail for security investigations by default.

Sysmon adds deeper visibility into endpoint activity, including process execution, command lines, parent-child process relationships, file activity, DNS queries, and network connections.

This helps analysts understand what happened before, during, and after suspicious activity.

---

## Important Sysmon Events

| Event ID | Event Name         | Purpose                                                            |
| -------: | ------------------ | ------------------------------------------------------------------ |
|        1 | Process Create     | Logs a new process, command line, parent process, user, and hashes |
|        3 | Network Connection | Logs network connections made by processes                         |
|        6 | Driver Loaded      | Logs drivers loaded into the system                                |
|        7 | Image Loaded       | Logs DLLs and modules loaded by processes                          |
|        8 | CreateRemoteThread | Can help identify process injection activity                       |
|       10 | Process Access     | Logs when one process accesses another process                     |
|       11 | File Create        | Logs file creation activity                                        |
|    12–14 | Registry Events    | Logs registry creation, deletion, and value changes                |
|    17–18 | Named Pipe Events  | Logs named pipe creation and connections                           |
|    19–21 | WMI Events         | Logs WMI filters, consumers, and bindings                          |
|       22 | DNS Query          | Logs DNS queries made by processes                                 |
|       23 | File Delete        | Logs file deletion activity                                        |

---

## Example: Investigating Suspicious PowerShell

If an attacker uses PowerShell, Sysmon Event ID 1 can help show:

* The process name, such as `powershell.exe`
* The full command line
* The parent process that launched it
* The user account involved
* The process hash
* The hostname of the endpoint

Sysmon Event ID 3 may show whether the PowerShell process made a network connection.

Sysmon Event ID 22 may show which domain the process attempted to resolve.

This gives a SOC analyst more investigation context than basic Windows logs alone.

---

## Sysmon Log Flow

Sysmon creates logs locally. It does not normally send logs directly to a SIEM.

A log-forwarding agent is needed to read Sysmon events and send them to a centralized platform.

```text
Sysmon
  ↓
Windows Event Log
  ↓
Log Forwarding Agent
  ↓
SIEM
```

Example with Splunk:

```text
Sysmon → Windows Event Logs → Splunk Universal Forwarder → Splunk Enterprise
```

Example with Elastic:

```text
Sysmon → Windows Event Logs → Elastic Agent / Winlogbeat → Elasticsearch → Kibana
```

---

## Common Log Forwarding Tools

| Tool                           | Purpose                                                            |
| ------------------------------ | ------------------------------------------------------------------ |
| Splunk Universal Forwarder     | Sends Windows Event Logs to Splunk                                 |
| Elastic Agent                  | Sends logs and endpoint data to Elastic                            |
| Winlogbeat                     | Sends Windows Event Logs to Elasticsearch                          |
| NXLog                          | Forwards Windows logs to different SIEM platforms                  |
| Windows Event Forwarding (WEF) | Built-in Windows method for forwarding logs to a central collector |

---

## Forwarding Sysmon Logs to a SIEM

A forwarding agent is configured with:

1. The log channel to collect.
2. The SIEM destination.
3. Connection details such as server address, port, certificates, or credentials.
4. The index, data stream, or destination where logs should be stored.

The Sysmon log channel is commonly:

```text
Microsoft-Windows-Sysmon/Operational
```

Example Splunk Universal Forwarder input:

```ini
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = windows
```

This tells the Splunk forwarder to collect Sysmon Operational logs and send them to the `windows` index.

---

## Searching Sysmon Logs in Splunk

Example SPL search:

```spl
index=windows source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
```

Search for PowerShell process creation:

```spl
index=windows EventCode=1 Image="*powershell.exe"
| table _time, Computer, User, ParentImage, Image, CommandLine, Hashes
```

Search for DNS queries:

```spl
index=windows EventCode=22
| table _time, Computer, Image, QueryName, QueryStatus
```

Search for network connections:

```spl
index=windows EventCode=3
| table _time, Computer, Image, DestinationIp, DestinationPort, Protocol
```

---

## Learning Setup

For a basic home lab, one Windows laptop is enough.

```text
Windows Laptop
├── Sysmon
├── Event Viewer
├── Log Forwarding Agent
└── SIEM Platform
```

Possible learning setup:

```text
Windows Laptop + Sysmon + Splunk Universal Forwarder
                           ↓
                    Splunk Enterprise
```

Splunk Enterprise can run on the same laptop for basic practice or on a separate virtual machine.

---

## Important Note About Sysmon Configuration

Sysmon can generate a large number of logs.

A configuration file is used to decide which events should be collected, included, or excluded. A well-tuned configuration helps reduce unnecessary noise while keeping useful security telemetry.

A commonly used community baseline is the SwiftOnSecurity Sysmon configuration.

---

## Key Takeaways

* Sysmon is a Windows endpoint-monitoring tool from Microsoft Sysinternals.
* It creates detailed security events and stores them in Windows Event Logs.
* Event Viewer displays logs; Sysmon creates additional endpoint-security logs.
* Sysmon does not replace antivirus, EDR, or a SIEM.
* Sysmon does not normally forward logs directly to a SIEM.
* A forwarding agent such as Splunk Universal Forwarder, Elastic Agent, Winlogbeat, NXLog, or WEF sends Sysmon logs to a SIEM.
* Sysmon is useful for SOC investigations, threat hunting, and detecting suspicious endpoint activity.

---

## Learning Resources

* Sysmon: https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
* Sysmon Configuration: https://github.com/SwiftOnSecurity/sysmon-config
* Splunk Universal Forwarder: https://www.splunk.com/en_us/download/universal-forwarder.html
* Elastic Agent: https://www.elastic.co/docs/reference/fleet/install-elastic-agents

---

> This repository is created for educational purposes and contains my personal learning notes on Sysmon, Windows Event Logs, and SIEM log forwarding.
