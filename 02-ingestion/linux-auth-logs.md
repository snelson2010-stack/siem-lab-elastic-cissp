# Linux Authentication Log Ingestion

## Overview

This document explains how Linux authentication events were generated, collected, and validated in the Elastic SIEM lab.

The Ubuntu target server acts as the Linux log source. Authentication activity is collected by Elastic Agent enrolled in Fleet and forwarded to Elasticsearch for analysis in Kibana.

This file focuses on log ingestion and validation. Full attack procedure documentation belongs in `04-attack-simulations/`.

---

## Objective

The goal of this lab section is to validate:

- Linux authentication logging
- SSH failed login visibility
- Elastic Agent log collection
- Kibana searchability
- SIEM ingestion pipeline reliability

---

## Lab Systems

| System | Role | IP Address |
|---|---|---|
| Kali Linux | Attacker | `192.168.70.130` |
| Ubuntu Target | Victim / Log Source | `192.168.70.128` |
| Ubuntu Target | Log Forwarding Interface | `192.168.56.30` |
| SIEM Server | Elasticsearch / Kibana | `192.168.56.10` |

---

## Log Source

Linux authentication events may be available through journald or authentication log files depending on the Linux distribution and logging configuration.

Common validation commands on the target system:

```bash
sudo journalctl | grep ssh
```

```bash
sudo tail -f /var/log/auth.log
```

If `/var/log/auth.log` is not present, journald may still contain SSH authentication events.

---

## Authentication Event Generation

Authentication activity was generated from Kali Linux against the Ubuntu target server.

Example failed SSH login test:

```bash
ssh fakeuser@192.168.70.128
```

Controlled brute force testing was also used in the isolated lab to generate repeated failed authentication events.

---

## Host-Level Authentication Logging

The Linux endpoint recorded authentication activity during testing.

### Evidence

![Linux Auth Log Terminal](image/linux-authlog-terminal.png)

### Observed behavior

- Failed SSH authentication attempts were recorded
- Invalid user attempts were visible
- SSH activity was logged on the target server
- Events could be used for later detection engineering

---

## Elastic Agent Collection

Elastic Agent is the collection method used in this lab.

Standalone Filebeat was not installed separately on the target server.

Elastic Agent collects authentication and system events from the Linux target and forwards them to Elasticsearch.

### Evidence

![Elastic Agent Status](image/Elastic%20Agent%20Status.png)

### Validation points

- Elastic Agent is enrolled in Fleet
- Target system is reporting successfully
- Authentication telemetry is forwarded to the SIEM server
- Logs are available for search in Kibana

---

## Kibana Validation

Authentication logs were validated in Kibana Discover.

Preferred parsed field query:

```kql
system.auth.ssh.event : "Failed"
```

Accepted login query:

```kql
system.auth.ssh.event : "Accepted"
```

Fallback raw message query:

```kql
message : "Failed password"
```

Dataset validation query:

```kql
data_stream.dataset : "system.auth"
```

### Evidence

![Failed SSH Logins](image/failed-ssh-logins.png.png)

---

## Log Flow Architecture

```text
Kali Linux
192.168.70.130
        ↓
SSH authentication attempts
        ↓
Ubuntu Target Server
192.168.70.128
        ↓
Linux authentication logs / journald
        ↓
Elastic Agent enrolled in Fleet
192.168.56.30
        ↓
Elasticsearch
192.168.56.10
        ↓
Kibana Discover / Dashboards / Detections
```

---

## Security Value

Linux authentication logs support:

- failed login monitoring
- brute force detection
- invalid user investigation
- account misuse detection
- incident response analysis
- authentication audit trails

---

## Key Findings

- Controlled SSH authentication testing generated useful security events.
- The Linux target recorded authentication failures.
- Elastic Agent successfully forwarded authentication telemetry.
- Kibana provided visibility into failed SSH login activity.
- Parsed fields such as `system.auth.ssh.event` support cleaner queries.

---

## Lessons Learned

- Linux logging can vary by distribution and configuration.
- `/var/log/auth.log` may not always be present.
- Journald can still provide authentication visibility.
- Elastic Agent health directly affects SIEM visibility.
- Ingestion should be validated before detections and dashboards are built.
- Raw message searches are useful fallback queries when parsed fields are unavailable.

---

## CISSP Domain Mapping

| CISSP Domain | Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | Centralized telemetry pipeline design |
| Domain 4: Communication and Network Security | SSH authentication visibility over the lab network |
| Domain 5: Identity and Access Management | Authentication monitoring and account activity review |
| Domain 6: Security Assessment and Testing | Controlled testing to validate logging and monitoring |
| Domain 7: Security Operations | SIEM ingestion, monitoring, and investigation |

---

## Conclusion

This lab demonstrates successful Linux authentication log ingestion using Elastic Agent and the Elastic Stack.

Authentication events generated on the Ubuntu target were collected, forwarded, indexed, and validated in Kibana. This confirms the ingestion pipeline is ready to support detection engineering, dashboards, and SOC-style investigation workflows.
