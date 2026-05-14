# SOC Architecture

## Purpose

This document summarizes the final architecture of the Free SIEM Lab using the Elastic Stack.

The lab simulates a small Security Operations Center environment where attack activity is generated from Kali Linux, logged by an Ubuntu target server, forwarded by Elastic Agent, indexed in Elasticsearch, and analyzed in Kibana.

---

## Architecture Overview

```text
Kali Linux Attacker
192.168.70.130
        ↓
Attack traffic over VMnet2
        ↓
Ubuntu Target Server
192.168.70.128
        ↓
Linux authentication logs
        ↓
Elastic Agent / Fleet
192.168.56.30
        ↓
Log forwarding over VMnet1
        ↓
Elastic SIEM Server
192.168.56.10
        ↓
Elasticsearch + Kibana
        ↓
Dashboards + Detection Rules + Alerts
```

---

## Lab Systems

| System | Role | Network | IP Address | Purpose |
|---|---|---|---|---|
| Kali Linux | Attacker | VMnet2 | `192.168.70.130` | Generates controlled attack traffic |
| Ubuntu Target | Victim / Log Source | VMnet2 | `192.168.70.128` | Receives SSH attack activity |
| Ubuntu Target | Log Forwarding Interface | VMnet1 | `192.168.56.30` | Sends logs to SIEM server |
| Elastic SIEM Server | Elasticsearch / Kibana | VMnet1 | `192.168.56.10` | Stores, searches, visualizes, and detects events |

---

## Network Segmentation

The lab uses segmented VMware host-only networks to separate attack traffic from SIEM management and log forwarding.

| Network | Subnet | Purpose |
|---|---|---|
| VMnet1 | `192.168.56.0/24` | SIEM network and log forwarding |
| VMnet2 | `192.168.70.0/24` | Attack simulation and target-facing traffic |

---

## System Roles

### Kali Linux

Kali Linux acts as the attacker system. It is used to generate controlled security events such as SSH login attempts and Hydra brute force activity.

### Ubuntu Target Server

The Ubuntu target server is the monitored endpoint. It receives SSH authentication attempts and generates Linux authentication logs.

The target has two network interfaces:

- `192.168.70.128` for attack traffic
- `192.168.56.30` for log forwarding to the SIEM server

### Elastic SIEM Server

The SIEM server runs Elasticsearch and Kibana. It stores log data, supports investigation in Discover, powers dashboards, and runs detection rules.

---

## Telemetry Flow

```text
SSH authentication attempt
        ↓
Linux authentication event generated
        ↓
Elastic Agent collects event
        ↓
Event forwarded to Elasticsearch
        ↓
Kibana Discover validates event
        ↓
Dashboard visualizes event
        ↓
Detection rule evaluates event
        ↓
Alert generated when threshold is exceeded
```

---

## Detection Flow

The SSH brute force detection uses a custom threshold rule.

| Detection Element | Value |
|---|---|
| Rule query | `system.auth.ssh.event : "Failed"` |
| Threshold field | `source.ip` |
| Threshold value | `5` |
| Source index | `logs-system.auth-default` |
| Alert source IP | `192.168.70.130` |
| Alert severity | `high` |

---

## Dashboard Flow

The authentication dashboard provides SOC visibility into:

- failed SSH events over time
- source IP activity
- usernames targeted
- authentication trends
- attack simulation validation

Dashboard evidence is documented in:

```text
05-dashboards/authentication-dashboard.md
```

---

## Repository Workflow Mapping

| Repository Section | Purpose |
|---|---|
| `01-setup/` | Documents VMware setup, IP assignments, and installation steps |
| `02-ingestion/` | Documents log collection, Elastic Agent status, and pipeline validation |
| `03-detections/` | Documents custom detection rules and alert validation |
| `04-attack-simulations/` | Documents controlled attack activity used to generate logs |
| `05-dashboards/` | Documents Kibana dashboard visibility and SOC analysis |
| `06-architecture/` | Summarizes the final SOC architecture and data flow |
| `07-troubleshooting/` | Captures issues, fixes, and lessons learned |

---

## Security Design Principles

This lab demonstrates:

- network segmentation
- centralized log collection
- isolated attack simulation
- controlled monitoring environment
- detection engineering
- dashboard-driven investigation
- repeatable SOC workflow validation

---

## CISSP Domain Alignment

| CISSP Domain | Architecture Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | Secure lab architecture and segmented design |
| Domain 4: Communication and Network Security | VMnet segmentation and controlled traffic flow |
| Domain 5: Identity and Access Management | SSH authentication monitoring |
| Domain 6: Security Assessment and Testing | Controlled attack simulation and validation |
| Domain 7: Security Operations | SIEM monitoring, alerting, dashboards, and investigation |

---

## Lessons Learned

- A dual-homed target system allows attack traffic and log forwarding to remain separated.
- Elastic Agent health is critical for reliable SIEM visibility.
- Parsed fields such as `system.auth.ssh.event` simplify detection logic.
- Prebuilt Elastic rules may require data sources not present in a small lab.
- Custom threshold rules can validate detections using available telemetry.
- Dashboards and detections should be built only after ingestion is validated.

---

## Conclusion

This architecture provides an end-to-end SOC simulation using Elastic Stack, VMware networking, Linux authentication telemetry, controlled attack simulation, Kibana dashboards, and custom detection rules.

The lab demonstrates how security events move from attack generation to log ingestion, detection, visualization, and alert validation.
