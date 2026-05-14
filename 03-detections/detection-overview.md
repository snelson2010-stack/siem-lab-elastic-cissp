# Detection Overview

## Purpose

This section documents the detection engineering work performed in the Elastic SIEM lab.

The goal is to identify suspicious authentication activity, validate that logs are searchable in Kibana, and map observed behavior to security operations use cases.

---

## Detection Sources

| Source | Collection Method | Purpose |
|---|---|---|
| Linux authentication events | Elastic Agent / Fleet | SSH login monitoring |
| Linux system logs | Elastic Agent / Fleet | System activity monitoring |
| Kali attack activity | Lab-generated traffic | Produces security events |
| Kibana Discover | Investigation interface | Search and validate events |

---

## Current Detection Focus

| Detection | Purpose | MITRE ATT&CK |
|---|---|---|
| Failed SSH login detection | Identify unsuccessful authentication attempts | T1110 - Brute Force |
| SSH brute force detection | Identify repeated failed SSH attempts | T1110 - Brute Force |
| Sudo activity monitoring | Track privileged command usage | T1548 - Abuse Elevation Control Mechanism |
| Nmap scan detection | Identify network scanning behavior | T1046 - Network Service Discovery |

---

## Detection Workflow

```text
Attack Simulation
        ↓
Target System Logs
        ↓
Elastic Agent Collection
        ↓
Elasticsearch Indexing
        ↓
Kibana Search / Detection Logic
        ↓
Dashboard or Alert Review
```

---

## Common KQL Queries

### Failed SSH logins

```kql
message : "Failed password"
```

### Invalid user attempts

```kql
message : "invalid user"
```

### Successful SSH logins

```kql
message : "Accepted password"
```

### Sudo activity

```kql
message : "sudo"
```

### Authentication dataset

```kql
data_stream.dataset : "system.auth"
```

---

## Screenshot Evidence

Detection screenshots should be stored in:

```text
03-detections/screenshots/
```

Recommended evidence:

```text
failed-ssh-logins.png
ssh-bruteforce-events.png
sudo-activity.png
nmap-scan-events.png
```

---

## CISSP Domain Alignment

| CISSP Domain | Relevance |
|---|---|
| Domain 5: Identity and Access Management | Authentication monitoring |
| Domain 6: Security Assessment and Testing | Validation through simulated attacks |
| Domain 7: Security Operations | Detection, alerting, and investigation |

---

## Notes

- Elastic Agent is used for collection in this lab.
- Standalone Filebeat was not installed separately on the target system.
- Detection logic is validated using lab-generated activity from Kali Linux.
