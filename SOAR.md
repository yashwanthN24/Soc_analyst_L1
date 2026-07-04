# SOAR: Security Orchestration, Automation, and Response

## Overview

This repository contains my learning notes on **SOAR**, which stands for **Security Orchestration, Automation, and Response**.

SOAR platforms help security teams and SOC analysts manage security alerts more efficiently by connecting different security tools, automating repetitive tasks, and guiding incident-response workflows.

---

## What Is SOAR?

SOAR is a security solution designed to help organizations respond to threats faster and more consistently.

It combines three main capabilities:

* **Security Orchestration**
* **Security Automation**
* **Security Response**

SOAR platforms integrate with security tools such as SIEM, EDR, firewalls, email-security gateways, threat-intelligence platforms, ticketing systems, and cloud-security tools.

---

## SOAR Components

### Security Orchestration

Security orchestration means connecting multiple security tools so they can work together.

For example, a SOAR platform can receive an alert from a SIEM, check an IP address using a threat-intelligence service, query an EDR platform for endpoint activity, and create a ticket in an incident-management system.

This reduces the need for analysts to manually switch between many different tools.

---

### Security Automation

Security automation means using predefined actions to complete repetitive security tasks automatically.

Examples include:

* Enriching an IP address with threat-intelligence data
* Checking a file hash against reputation databases
* Looking up a suspicious domain
* Collecting endpoint details from EDR
* Creating incident tickets
* Sending notifications to analysts
* Blocking a malicious IP address
* Disabling a compromised user account
* Isolating an infected endpoint

Automation helps analysts save time and focus on complex investigations.

---

### Security Response

Security response refers to the actions taken after a threat is detected.

SOAR platforms can help analysts respond by:

* Assigning incidents to analysts
* Prioritizing alerts
* Collecting investigation evidence
* Running response actions
* Tracking incident status
* Documenting actions taken
* Escalating high-severity incidents
* Closing resolved incidents

---

## Why Is SOAR Important?

SOC teams receive a large number of alerts every day. Many alerts are repetitive or require the same initial investigation steps.

SOAR helps reduce manual work, improve response speed, and ensure analysts follow consistent procedures.

Benefits of SOAR include:

* Faster alert triage
* Reduced analyst workload
* Consistent incident-response processes
* Better documentation
* Faster threat containment
* Improved collaboration between security teams
* Reduced mean time to respond (MTTR)

---

## SOAR Workflow

A typical SOAR workflow may look like this:

1. A SIEM generates an alert for suspicious activity.
2. The alert is sent to the SOAR platform.
3. SOAR enriches the alert using threat-intelligence sources.
4. SOAR checks related endpoint activity in EDR.
5. SOAR creates an incident ticket.
6. Based on rules and analyst approval, SOAR may block the malicious IP, isolate the endpoint, or disable the affected account.
7. The analyst reviews the investigation results.
8. The incident is documented and closed or escalated.

---

## What Is a Playbook?

A **playbook** is a predefined workflow used by a SOAR platform to investigate and respond to a specific type of alert.

Playbooks ensure that analysts follow the same response steps every time.

Examples of SOAR playbooks include:

* Phishing email investigation
* Malware alert investigation
* Brute-force login investigation
* Suspicious IP address investigation
* Ransomware response
* Compromised user account response
* Endpoint malware detection response
* Data-exfiltration investigation

---

## Example: Phishing Email Playbook

A phishing-email playbook may perform the following actions:

1. Receive a suspicious-email alert.
2. Extract the sender address, URLs, attachments, and file hashes.
3. Check URLs and hashes using threat-intelligence services.
4. Search whether other users received the same email.
5. Check whether users clicked the malicious link.
6. Quarantine the email from affected mailboxes.
7. Block malicious domains or IP addresses.
8. Create an incident ticket.
9. Notify the SOC team.
10. Document the investigation results.

---

## Example: Brute-Force Login Playbook

A brute-force detection playbook may perform the following actions:

1. Receive an alert for multiple failed login attempts.
2. Identify the affected user account and source IP address.
3. Check the IP address using threat intelligence.
4. Search for successful logins after failed attempts.
5. Check whether the user account accessed unusual systems.
6. Temporarily block the suspicious IP address.
7. Disable or reset the affected account if compromise is suspected.
8. Create an incident ticket.
9. Notify the security team.
10. Escalate the incident if necessary.

---

## SOAR vs SIEM

| Feature          | SIEM                                 | SOAR                                                       |
| ---------------- | ------------------------------------ | ---------------------------------------------------------- |
| Main purpose     | Collect, correlate, and analyze logs | Automate and coordinate incident response                  |
| Data sources     | Logs, events, alerts, and telemetry  | SIEM, EDR, firewalls, threat intelligence, ticketing tools |
| Alert generation | Yes                                  | Can receive alerts from SIEM and other tools               |
| Investigation    | Log searching and correlation        | Automated enrichment and guided investigation              |
| Response         | Usually manual                       | Automated or analyst-approved actions                      |
| Playbooks        | Limited or not available             | Core feature                                               |
| Case management  | May be basic                         | Often includes detailed case management                    |

---

## SOAR vs EDR

| Feature            | EDR                                                  | SOAR                                                          |
| ------------------ | ---------------------------------------------------- | ------------------------------------------------------------- |
| Main focus         | Monitoring and responding to endpoint threats        | Coordinating security tools and automating response workflows |
| Coverage           | Endpoints such as laptops, servers, and workstations | Multiple security tools across the organization               |
| Endpoint telemetry | Yes                                                  | Uses data from integrated tools                               |
| Endpoint isolation | Yes                                                  | Can trigger isolation through EDR integration                 |
| Playbooks          | May have limited workflows                           | Uses detailed automated playbooks                             |
| Case management    | May be limited                                       | Often includes incident and case management                   |

---

## Common SOAR Integrations

SOAR platforms can integrate with:

* SIEM platforms
* EDR platforms
* Firewalls
* Email-security gateways
* Threat-intelligence platforms
* Identity and access-management tools
* Cloud-security tools
* Vulnerability-management platforms
* Ticketing systems
* Chat and notification platforms

---

## SOC Analyst Use of SOAR

SOC analysts use SOAR to:

1. Receive and prioritize alerts.
2. Enrich alerts with additional context.
3. Investigate users, IP addresses, domains, files, and endpoints.
4. Follow incident-response playbooks.
5. Automate repetitive investigation tasks.
6. Perform containment actions.
7. Document investigation findings.
8. Escalate serious incidents to senior analysts or incident-response teams.

---

## Key Terms

### Orchestration

Connecting different security tools so they can share information and perform coordinated actions.

### Automation

Performing repetitive security tasks automatically based on rules or playbooks.

### Response

Taking action to contain, investigate, and resolve a security incident.

### Playbook

A predefined workflow that defines investigation and response steps for a specific alert type.

### Enrichment

Adding useful context to an alert, such as IP reputation, domain reputation, file hash details, user information, or endpoint details.

### Case Management

Tracking an incident from alert creation to investigation, response, documentation, and closure.

### MTTR

**Mean Time to Respond** is the average time taken by a security team to respond to and resolve security incidents.

---

## Key Takeaways

* SOAR stands for Security Orchestration, Automation, and Response.
* SOAR connects security tools such as SIEM, EDR, firewalls, and threat-intelligence platforms.
* It automates repetitive SOC tasks and improves incident-response speed.
* SOAR uses playbooks to standardize investigation and response workflows.
* Analysts can use SOAR to enrich alerts, contain threats, create tickets, and document incidents.
* SOAR works alongside SIEM and EDR rather than replacing them.

---

## Learning Platform

* Platform: TryHackMe
* Topic: SOAR
* Focus Areas: Security automation, playbooks, alert enrichment, incident response, and SOC workflows

---

> This repository is created for educational purposes and contains my personal learning notes on Security Orchestration, Automation, and Response (SOAR).
