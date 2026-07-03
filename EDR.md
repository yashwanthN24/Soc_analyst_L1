# Introduction to EDR

## Overview

This repository contains my notes and key learnings from the **Introduction to Endpoint Detection and Response (EDR)** room on TryHackMe.

EDR is a cybersecurity solution that helps organizations monitor, detect, investigate, and respond to threats on their devices.

In cybersecurity, a device such as a laptop, desktop, server, or virtual machine is called an **endpoint**.

---

## What Is EDR?

**Endpoint Detection and Response (EDR)** is a security solution that continuously monitors endpoints for suspicious activity.

Unlike traditional antivirus software, which mainly detects known malware using signatures, EDR provides deeper visibility into what is happening on an endpoint.

EDR collects endpoint activity, analyzes it for suspicious behavior, alerts security teams, and helps them respond to threats.

---

## Examples of Endpoints

Endpoints can include:

* Employee laptops
* Desktop computers
* Servers
* Virtual machines
* Mobile devices
* Workstations connected to an organization’s network

---

## Why Is EDR Needed?

Traditional antivirus software is useful, but it may not detect every threat.

For example, attackers may use:

* New or unknown malware
* Fileless attacks
* Malicious PowerShell commands
* Stolen credentials
* Suspicious processes
* Exploited vulnerabilities
* Lateral movement inside a network

EDR helps security teams detect suspicious behavior even when a threat does not match a known malware signature.

---

## How EDR Works

EDR usually works through an agent installed on each endpoint.

The EDR agent collects telemetry from the device and sends it to a centralized EDR console.

### Common Endpoint Telemetry

An EDR agent may collect information such as:

* Running processes
* Parent and child process relationships
* File creation, deletion, and modification
* Registry changes
* User logins and actions
* Network connections
* Command-line activity
* PowerShell execution
* Security alerts
* Suspicious behavior indicators

---

## Centralized EDR Console

The collected endpoint data is sent to a centralized EDR console.

Security analysts can use the console to:

* View alerts from multiple endpoints
* Investigate suspicious activity
* Search endpoint telemetry
* Analyze process trees
* Review file and network activity
* Identify indicators of compromise (IOCs)
* Investigate activity related to known CVEs
* Track affected devices
* Respond to incidents

---

## EDR Detection Methods

EDR can detect threats using multiple techniques:

* Signature-based detection
* Behavioral analysis
* Detection rules
* Threat intelligence feeds
* Indicators of compromise (IOCs)
* Known malicious file hashes
* Suspicious command-line activity
* Anomalous process behavior
* Activity associated with commonly exploited CVEs

---

## EDR Response Actions

When suspicious activity is detected, security teams can respond directly from the EDR console.

Common response actions include:

* Isolating an endpoint from the network
* Terminating malicious processes
* Quarantining suspicious files
* Collecting forensic evidence
* Running remote response commands
* Blocking malicious hashes or domains
* Investigating user activity
* Removing persistence mechanisms

---

## Antivirus vs EDR

| Feature                | Traditional Antivirus       | EDR                                                                      |
| ---------------------- | --------------------------- | ------------------------------------------------------------------------ |
| Main focus             | Prevent known malware       | Detect, investigate, and respond to threats                              |
| Detection method       | Mostly signatures           | Signatures, behavior, rules, and threat intelligence                     |
| Visibility             | Limited                     | Detailed endpoint telemetry                                              |
| Investigation          | Limited                     | Process trees, logs, file activity, and network activity                 |
| Response capability    | Basic removal or quarantine | Isolation, remote response, process termination, and forensic collection |
| Centralized management | May be limited              | Centralized console for all endpoints                                    |

---

## Key Terms

### Endpoint

A device connected to an organization’s network, such as a laptop, desktop, server, or virtual machine.

### EDR Agent

Software installed on an endpoint that collects security telemetry and sends it to the EDR platform.

### Telemetry

Activity and event data collected from an endpoint, such as process execution, file changes, and network connections.

### IOC

An **Indicator of Compromise** is evidence that may indicate malicious activity, such as a malicious IP address, file hash, domain, or process.

### CVE

A **Common Vulnerabilities and Exposures (CVE)** identifier is a public reference for a known security vulnerability.

### Process Tree

A visual relationship between processes that shows which process started another process.

---

## SOC Analyst Use of EDR

Security Operations Center (SOC) analysts use EDR to:

1. Receive and triage security alerts.
2. Investigate suspicious endpoint activity.
3. Analyze processes, files, users, and network connections.
4. Identify whether the activity is malicious or benign.
5. Contain affected endpoints.
6. Collect evidence for incident response.
7. Document findings and close or escalate the incident.

---

## Key Takeaways

* EDR protects and monitors organization devices known as endpoints.
* It provides more visibility than traditional antivirus software.
* EDR agents collect endpoint telemetry and send it to a centralized console.
* Security teams use EDR to detect, investigate, contain, and respond to threats.
* EDR is an important tool for SOC analysts and blue team professionals.

---

## Learning Platform

* Platform: TryHackMe
* Room: Introduction to EDR
* Topic: Endpoint Detection and Response

---

> This repository is created for educational purposes and contains my personal learning notes from TryHackMe.
