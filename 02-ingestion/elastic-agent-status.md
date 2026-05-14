# Elastic Agent Status

## Overview

This document verifies that Elastic Agent is installed, enrolled, operational, and successfully forwarding telemetry from the Linux target system into the Elastic Stack.

Elastic Agent serves as the primary data collection mechanism within the SIEM lab environment.

---

## Purpose

The purpose of this validation is to confirm:

- Elastic Agent is installed correctly
- The service is running
- The target host is enrolled in Fleet
- Authentication logs are being forwarded to Elasticsearch
- The ingestion pipeline is operational

---

## Environment

| Component | Purpose |
|---|---|
| Ubuntu Target VM | Endpoint generating logs |
| Elastic Agent | Telemetry collection |
| Elasticsearch | Event indexing and storage |
| Kibana Fleet | Agent management and monitoring |

---

## Verify Elastic Agent Service

On the Ubuntu target machine:

```bash
sudo systemctl status elastic-agent
```

Expected result:

```text
active (running)
```

---

## Verify Elastic Agent Health

Check detailed status:

```bash
sudo elastic-agent status
```

Expected result:

```text
Status: HEALTHY
```

This confirms:

- The agent is enrolled
- Fleet communication is operational
- Integrations are functioning
- Telemetry collection is active

---

## Verify Fleet Status in Kibana

In Kibana:

```text
Management → Fleet → Agents
```

The Ubuntu target should appear as:

```text
Healthy
```

Additional details may include:

- Hostname
- Agent version
- Policy name
- Last check-in time

---

## Verify Authentication Dataset Collection

Inside Kibana Discover, validate authentication events.

Recommended query:

```kql
data_stream.dataset : "system.auth"
```

Alternative queries:

```kql
message : "Failed password"
```

```kql
message : "Accepted password"
```

---

## Screenshot Evidence

### Elastic Agent Status

This screenshot confirms that the target server is enrolled, connected, and reporting through Elastic Agent/Fleet.

![Elastic Agent Status](screenshots/Elastic Agent Status.png)

---

## Security Value

Elastic Agent provides centralized telemetry collection for the SIEM platform.

Benefits include:

- Endpoint visibility
- Authentication monitoring
- Centralized log analysis
- Detection engineering support
- Faster incident investigation

Without a healthy ingestion agent, detection rules and dashboards cannot function correctly.

---

## Troubleshooting Notes

During lab setup, several ingestion issues were encountered and resolved.

Common issues included:

- Incorrect Fleet enrollment tokens
- Connectivity problems between target and SIEM server
- Authentication datasets not enabled
- Kibana data views not configured correctly
- Time filters preventing log visibility

Validation steps improved understanding of Elastic ingestion workflows and SIEM operations.

---

## CISSP Domain Alignment

| CISSP Domain | Relevance |
|---|---|
| Domain 7: Security Operations | Continuous monitoring and telemetry collection |
| Domain 6: Security Assessment and Testing | Validation of security controls and visibility |
| Domain 4: Communication and Network Security | Secure agent communication |
