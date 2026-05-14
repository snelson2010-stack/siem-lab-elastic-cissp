# Ingestion Overview

## Purpose

This document defines how security telemetry is collected, processed, normalized, and prepared for analysis within the Elastic SIEM lab.

The goal is to ensure that Linux authentication events are collected consistently, searchable in Kibana, and usable for detection engineering, dashboards, and incident investigation.

---

## Ingestion Architecture

### Flow Diagram

![Ingestion Overview](screenshots/ingestion-overview.png)

```text
Kali Linux Attack Activity
        ↓
Ubuntu Target Server
        ↓
Linux authentication and system logs
        ↓
Elastic Agent / Fleet
        ↓
Elasticsearch
        ↓
Kibana Discover / Dashboards / Detections
```

---

## Data Sources

The current ingestion focus is Linux authentication telemetry from the Ubuntu target server.

Collected event types include:

- SSH failed login attempts
- SSH successful login attempts
- Invalid user attempts
- Sudo activity
- Linux system security events

Future expansion may include:

- Windows event logs
- Firewall logs
- DNS logs
- Endpoint security telemetry

---

## Collection Method

The target server uses **Elastic Agent enrolled in Fleet**.

Standalone Filebeat was not installed separately on the target system.

Elastic Agent collects Linux system and authentication logs and forwards them to Elasticsearch for indexing and analysis.

---

## Ingestion Pipeline

```text
Raw Linux log
        ↓
Elastic Agent collection
        ↓
Parsing and field extraction
        ↓
ECS normalization
        ↓
Elasticsearch indexing
        ↓
Kibana search and visualization
```

The ingestion pipeline helps:

- parse unstructured Linux logs
- extract useful security fields
- normalize events into Elastic Common Schema
- make events searchable for detections and dashboards

---

## SSH Event Field Validation

The lab uses the parsed SSH event field shown in Kibana:

```text
system.auth.ssh.event
```

This field is used to identify SSH authentication outcomes.

### Failed SSH events

```kql
system.auth.ssh.event : "Failed"
```

### Accepted SSH events

```kql
system.auth.ssh.event : "Accepted"
```

---

## Additional Validation Queries

Parsed SSH fields are the preferred validation method when available. Raw message queries are also useful as a fallback.

### Failed password messages

```kql
message : "Failed password"
```

### Invalid user attempts

```kql
message : "invalid user"
```

### Authentication dataset

```kql
data_stream.dataset : "system.auth"
```

---

## ECS Field Mapping Example

| Raw Log Element | ECS / Elastic Field | Purpose |
|---|---|---|
| Timestamp | `@timestamp` | Time of event |
| Source IP | `source.ip` | Attacking or connecting host |
| Username | `user.name` | Account involved in authentication |
| SSH result | `system.auth.ssh.event` | SSH event result such as Failed or Accepted |
| Raw event text | `message` | Original log message |
| Hostname | `host.name` | System that generated the event |

---

## Pipeline Validation Checks

Validation confirms that:

- authentication logs are collected from the target server
- events are indexed in Elasticsearch
- SSH fields are visible in Kibana
- `system.auth.ssh.event` is populated
- timestamps are accurate
- events can support dashboards and detection rules

---

## Index Output

Authentication data may appear in Elastic data streams or indices related to Linux system logs, depending on the integration and version.

Useful searches include:

```kql
data_stream.dataset : "system.auth"
```

or:

```kql
system.auth.ssh.event : "Failed"
```

---

## SOC Use Cases

This ingestion pipeline supports:

- authentication monitoring
- brute force detection
- failed login investigation
- centralized log analysis
- incident response
- compliance auditing
- dashboard creation

---

## CISSP Domain Alignment

| CISSP Domain | Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | Ingestion pipeline design and ECS normalization |
| Domain 5: Identity and Access Management | SSH authentication monitoring and user activity tracking |
| Domain 6: Security Assessment and Testing | Pipeline validation using KQL and controlled test activity |
| Domain 7: Security Operations | Continuous monitoring, alerting, dashboards, and incident response support |
| Domain 8: Software Development Security | Structured parsing, field extraction, and data integrity validation |

---

## Lessons Learned

- ECS consistency is critical for correlation.
- Misconfigured fields can break detections silently.
- SSH logs provide high-value attack signals.
- Parsed fields such as `system.auth.ssh.event` are useful for cleaner detections.
- Raw `message` searches are helpful fallback queries.
- Ingestion should be validated before building dashboards or detection rules.
