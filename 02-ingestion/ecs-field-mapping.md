# ECS Field Mapping

## Overview

This document identifies important Elastic Common Schema (ECS) fields used to analyze Linux authentication events within the SIEM lab.

Elastic Common Schema provides a standardized structure for event data, making security analysis, detection engineering, and dashboard creation easier across different log sources.

---

## Purpose

The goal of ECS mapping is to:

- Normalize authentication events
- Improve search consistency
- Simplify dashboard creation
- Support detection rule development
- Enable scalable SIEM analysis

---

## Authentication Event Flow

```text
Linux Authentication Event
        ↓
Elastic Agent Collection
        ↓
ECS Field Parsing
        ↓
Elasticsearch Indexing
        ↓
Kibana Analysis
```

---

## Important ECS Fields

| ECS Field | Purpose |
|---|---|
| `@timestamp` | Time the event occurred |
| `host.name` | System generating the event |
| `agent.name` | Elastic Agent hostname |
| `data_stream.dataset` | Dataset type such as system.auth |
| `message` | Raw authentication log message |
| `source.ip` | Source IP address if parsed |
| `user.name` | Username involved in authentication |
| `event.dataset` | Event dataset category |
| `event.action` | Specific event activity |
| `event.outcome` | Success or failure result |
| `process.name` | Process responsible for event |

---

## Example Authentication Event

```text
Failed password for invalid user admin from 192.168.70.129 port 22 ssh2
```

Potential ECS mappings:

| Log Element | ECS Field |
|---|---|
| Failed password | event.outcome |
| admin | user.name |
| 192.168.70.129 | source.ip |
| ssh2 | network.protocol |

---

## Useful KQL Queries

### Authentication dataset events

```kql
data_stream.dataset : "system.auth"
```

### Failed SSH logins

```kql
message : "Failed password"
```

### Successful SSH logins

```kql
message : "Accepted password"
```

### Invalid user enumeration

```kql
message : "invalid user"
```

### Sudo privilege escalation activity

```kql
message : "sudo"
```

---

## Example Security Analysis

### Identify repeated failed logins

Use:

```kql
message : "Failed password"
```

This helps identify:

- Brute force attacks
- Password spraying
- Unauthorized access attempts

---

### Identify successful logins after failures

Correlate:

```kql
message : "Accepted password"
```

with previous failed authentication attempts.

This may indicate:

- Compromised credentials
- Successful brute force attacks
- Suspicious login behavior

---

## Screenshots

Add screenshots demonstrating ECS fields inside Kibana.

```md
![Kibana ECS Fields](./screenshots/kibana-ecs-fields.png)

![Authentication Event Fields](./screenshots/authentication-event-fields.png)
```

---

## Security Value

ECS normalization improves SIEM effectiveness by:

- Standardizing security telemetry
- Improving search efficiency
- Supporting cross-platform analysis
- Simplifying detection engineering
- Improving dashboard consistency

Understanding ECS is critical for effective Elastic SIEM operations.

---

## Lessons Learned

During ingestion testing:

- Some authentication fields required manual validation
- Journald and auth.log formats varied by distribution
- Certain fields required parsing improvements
- Data view configuration impacted field visibility

Field mapping validation improved understanding of Elastic SIEM normalization and search behavior.

---

## CISSP Domain Alignment

| CISSP Domain | Relevance |
|---|---|
| Domain 7: Security Operations | Security monitoring and event analysis |
| Domain 6: Security Assessment and Testing | Validation of logging and monitoring controls |
| Domain 5: Identity and Access Management | Authentication and user activity monitoring |
