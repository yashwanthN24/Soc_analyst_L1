# Splunk, SPL, Data Transformation and Anomaly Detection

## Overview

This repository contains my learning notes on Splunk, the Splunk Processing Language (SPL), log analysis, data transformation, data structuring, and anomaly detection.

Splunk is widely used by SOC analysts, security teams, DevOps engineers, and IT teams to collect, search, analyze, and visualize machine-generated data such as logs, events, alerts, and metrics.

---

## What Is Splunk?

Splunk is a platform used to collect and analyze data from different sources.

It can ingest data from:

* Windows Event Logs
* Linux system logs
* Firewall logs
* Web server logs
* Application logs
* Network device logs
* Cloud logs
* EDR alerts
* Authentication logs
* DNS logs

Splunk helps analysts search through large volumes of logs and identify suspicious activity, operational issues, and security incidents.

---

## Important Splunk Components

### Forwarder

A forwarder is installed on a machine to collect logs and send them to Splunk.

Examples:

* Universal Forwarder
* Heavy Forwarder

### Indexer

The indexer receives incoming data, processes it, stores it, and makes it searchable.

### Search Head

The search head is where users run searches, create dashboards, investigate alerts, and visualize data.

### Index

An index is a storage location where Splunk stores events.

Example indexes:

* `main`
* `windows`
* `firewall`
* `security`
* `web_logs`

### Sourcetype

A sourcetype tells Splunk what type of data is being ingested.

Examples:

* `WinEventLog:Security`
* `syslog`
* `access_combined`
* `json`
* `csv`

---

## What Is SPL?

SPL stands for **Search Processing Language**.

It is Splunk’s query language used to search, filter, transform, analyze, and visualize data.

A basic SPL search looks like this:

```spl
index=main
```

This searches all events stored in the `main` index.

Another example:

```spl
index=windows EventCode=4625
```

This searches for failed Windows logon events.

---

## Basic SPL Commands

| Command       | Purpose                                   |
| ------------- | ----------------------------------------- |
| `search`      | Searches for matching events              |
| `where`       | Filters results using conditions          |
| `table`       | Displays selected fields in a table       |
| `stats`       | Performs calculations and aggregation     |
| `eval`        | Creates or modifies fields                |
| `fields`      | Includes or removes fields                |
| `rename`      | Renames fields                            |
| `sort`        | Sorts results                             |
| `dedup`       | Removes duplicate events                  |
| `rex`         | Extracts fields using regular expressions |
| `timechart`   | Creates time-based charts                 |
| `transaction` | Groups related events together            |

---

## SPL Search Examples

### Search Failed Login Attempts

```spl
index=windows EventCode=4625
```

### Search Successful Logins

```spl
index=windows EventCode=4624
```

### Display Selected Fields

```spl
index=windows EventCode=4625
| table _time, user, src_ip, host, EventCode
```

### Count Failed Logins by User

```spl
index=windows EventCode=4625
| stats count by user
| sort - count
```

### Count Failed Logins by Source IP

```spl
index=windows EventCode=4625
| stats count by src_ip
| sort - count
```

### Find Events From a Specific IP Address

```spl
index=windows src_ip="192.168.1.10"
```

### Search for PowerShell Activity

```spl
index=windows process_name="powershell.exe"
```

---

## Data Structuring in Splunk

Raw logs are often difficult to read because they contain unstructured text.

Example raw log:

```text
2026-07-03 10:30:15 Failed login for user admin from 192.168.1.10
```

To make this useful, Splunk extracts fields such as:

| Field       | Value               |
| ----------- | ------------------- |
| `timestamp` | 2026-07-03 10:30:15 |
| `action`    | Failed login        |
| `user`      | admin               |
| `src_ip`    | 192.168.1.10        |

Once data is structured, analysts can search and filter it more easily.

Example:

```spl
index=security action="Failed login" user="admin"
```

---

## Field Extraction Using `rex`

The `rex` command extracts fields from raw log data using regular expressions.

Example:

```spl
index=security
| rex "Failed login for user (?<user>\w+) from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| table _time, user, src_ip
```

This extracts:

* `user`
* `src_ip`

from raw log events.

---

## Data Transformation Using SPL

Data transformation means converting raw data into a more useful format for analysis.

Splunk can transform data using commands such as:

* `eval`
* `stats`
* `chart`
* `timechart`
* `rename`
* `rex`
* `lookup`
* `fields`

### Example: Create a New Field With `eval`

```spl
index=windows EventCode=4625
| eval login_status="Failed"
| table _time, user, src_ip, login_status
```

### Example: Rename a Field

```spl
index=windows
| rename src_ip as source_ip
| table _time, user, source_ip
```

### Example: Convert Bytes to Megabytes

```spl
index=network
| eval transferred_mb=round(bytes/1024/1024, 2)
| table src_ip, dest_ip, transferred_mb
```

### Example: Categorize Login Activity

```spl
index=windows EventCode=4624 OR EventCode=4625
| eval login_status=if(EventCode=4624, "Success", "Failure")
| stats count by user, login_status
```

---

## Anomaly Detection

An anomaly is activity that is unusual compared to normal behavior.

In cybersecurity, anomaly detection helps identify suspicious behavior that may indicate an attack, misconfiguration, or policy violation.

Examples include:

* A user logging in from a new country
* Hundreds of failed login attempts
* A sudden increase in network traffic
* A device communicating with an unknown IP address
* A user accessing systems outside normal working hours
* A large number of files being deleted
* A process running from an unusual directory
* A sudden spike in PowerShell activity

---

## SPL Use Cases for Anomaly Detection

### 1. Detect Multiple Failed Login Attempts

```spl
index=windows EventCode=4625
| stats count by user, src_ip
| where count > 10
| sort - count
```

This can help identify brute-force login attempts.

---

### 2. Detect Failed Login Spikes Over Time

```spl
index=windows EventCode=4625
| timechart span=1h count as failed_logins
```

This helps identify unusual increases in failed logins.

---

### 3. Detect Successful Login After Multiple Failures

```spl
index=windows EventCode=4625 OR EventCode=4624
| stats count by user, src_ip, EventCode
| sort user, src_ip
```

This can be used as a starting point to investigate possible password guessing or brute-force activity.

---

### 4. Detect Users With Unusually High Login Activity

```spl
index=windows EventCode=4624
| stats count by user
| eventstats avg(count) as average_logins stdev(count) as standard_deviation
| eval threshold=average_logins+(2*standard_deviation)
| where count > threshold
| table user, count, average_logins, threshold
```

This identifies users whose login count is significantly higher than the average.

---

### 5. Detect Logins Outside Business Hours

```spl
index=windows EventCode=4624
| eval hour=strftime(_time, "%H")
| where hour < 8 OR hour > 19
| table _time, user, src_ip, host
```

This can help identify unusual access outside expected working hours.

---

### 6. Detect Suspicious PowerShell Usage

```spl
index=windows process_name="powershell.exe"
| stats count by user, host, CommandLine
| sort - count
```

This helps analysts review frequently executed PowerShell commands.

---

### 7. Detect High Network Traffic From a Source IP

```spl
index=network
| stats sum(bytes) as total_bytes by src_ip
| eval total_mb=round(total_bytes/1024/1024, 2)
| sort - total_mb
```

This can help identify possible data exfiltration or abnormal traffic volume.

---

### 8. Detect Connections to Multiple Destination IPs

```spl
index=network
| stats dc(dest_ip) as unique_destinations by src_ip
| where unique_destinations > 50
| sort - unique_destinations
```

This can help identify possible network scanning or suspicious outbound activity.

---

### 9. Detect Rare Processes

```spl
index=windows
| stats count by process_name
| where count < 5
| sort count
```

Rare processes should be investigated because they may be suspicious or newly introduced.

---

### 10. Detect New User Accounts

```spl
index=windows EventCode=4720
| table _time, user, TargetUserName, host
```

Windows Event ID `4720` indicates that a user account was created.

---

## SOC Investigation Workflow Using Splunk

A SOC analyst may use Splunk in the following workflow:

1. Receive an alert.
2. Identify the affected user, host, IP address, or process.
3. Search related events in Splunk.
4. Check the timeline of activity.
5. Investigate failed and successful logins.
6. Review process execution and command-line activity.
7. Check network connections.
8. Determine whether the activity is malicious or benign.
9. Document findings.
10. Escalate or close the alert.

---

## Key Takeaways

* Splunk collects and analyzes logs from multiple sources.
* SPL is used to search, filter, transform, and investigate data.
* Structured data is easier to analyze than raw logs.
* Commands such as `rex`, `eval`, `stats`, and `timechart` help transform data.
* Anomaly detection identifies unusual behavior compared to normal activity.
* Splunk is commonly used by SOC analysts for log analysis, threat detection, and incident investigation.

---

## Learning Resources

* Platform: TryHackMe
* Topic: Splunk, SPL, Log Analysis, and Anomaly Detection
* Focus Areas: Data structuring, data transformation, investigation, and security monitoring

---

> This repository is created for educational purposes and contains my personal learning notes on Splunk and SPL.
