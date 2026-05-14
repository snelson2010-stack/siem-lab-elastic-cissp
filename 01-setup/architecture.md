# Elastic SIEM Lab Architecture — SOC Simulation Project

## Overview

This project builds a simulated Security Operations Center (SOC) environment using the Elastic Stack.

The lab demonstrates:

- VMware network segmentation
- Linux log ingestion
- Elastic Agent / Fleet management
- Attack simulation
- Detection engineering
- Kibana dashboard analysis

The environment is designed for cybersecurity learning and aligns with CISSP domains including Security Operations, Communication and Network Security, Security Architecture, and Security Assessment and Testing.

---

## VMware Network Design

### VMnet1 — SIEM Network

| Item | Details |
|---|---|
| Subnet | `192.168.56.0/24` |
| Purpose | SIEM infrastructure, Elasticsearch, Kibana, log forwarding |
| SIEM Server | `192.168.56.10` |
| Target SIEM Interface | `192.168.56.30` |

### VMnet2 — Attack Network

| Item | Details |
|---|---|
| Subnet | `192.168.70.0/24` |
| Purpose | Attack simulation, target access, security event generation |
| Kali Linux Attacker | `192.168.70.130` |
| Target Attack Interface | `192.168.70.128` |

---

## Why VMware?

VMware provides:

- isolated lab networks
- safe attack simulation
- repeatable VM snapshots
- clear network segmentation
- realistic SOC-style infrastructure
- separation between attacker, target, and monitoring systems

---

## IP Assignment Plan

| VM Name | Role | Network | IP Address | Purpose |
|---|---|---|---|---|
| SIEM Server | Elasticsearch + Kibana | VMnet1 | `192.168.56.10` | Stores and visualizes logs |
| Target Server | Log Source / Victim | VMnet1 | `192.168.56.30` | Sends logs to SIEM |
| Target Server | Log Source / Victim | VMnet2 | `192.168.70.128` | Receives attacks |
| Kali Linux | Attacker Machine | VMnet2 | `192.168.70.130` | Runs Nmap, Hydra, and SSH tests |

---

## SIEM Architecture

```text
Kali Linux VM — Attacker
192.168.70.130
        |
        | Attack traffic: SSH, Nmap, Hydra
        v
Ubuntu Target VM — Victim / Log Source
Attack interface: 192.168.70.128
        |
        | Local logs generated: SSH, auth, syslog, sudo
        v
Elastic Agent
Log-forwarding interface: 192.168.56.30
        |
        | Log forwarding
        v
SIEM Server VM
192.168.56.10
Elasticsearch + Kibana + Fleet
```

---

## Data Flow

```text
Attacker
192.168.70.130
        ↓
Target Server attack interface
192.168.70.128
        ↓
Linux authentication and system logs generated
        ↓
Elastic Agent on Target Server
192.168.56.30
        ↓
SIEM Server
192.168.56.10
        ↓
Elasticsearch → Kibana Dashboards → Detection Rules
```

---

## Security Purpose

This architecture supports:

- centralized log collection
- attack simulation
- detection validation
- authentication monitoring
- network segmentation testing
- incident response practice

---

## CISSP Domain Mapping

| CISSP Domain | Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | Segmented lab architecture and secure design |
| Domain 4: Communication and Network Security | VMnet separation and controlled network traffic |
| Domain 5: Identity and Access Management | Authentication log monitoring |
| Domain 6: Security Assessment and Testing | Simulated attacks and validation testing |
| Domain 7: Security Operations | SIEM monitoring, dashboards, and detections |

---

## Related Documentation

| Document | Purpose |
|---|---|
| `01-setup/vmware-setup.md` | VMware network and VM configuration |
| `01-setup/ip-assignment-plan.md` | Static IP address reference |
| `01-setup/installation.md` | Elastic Stack and agent installation |
| `02-ingestion/ingestion-overview.md` | Log ingestion process |
| `02-ingestion/linux-auth-log.md` | Linux authentication log collection |
| `03-detections/` | Detection engineering documentation |
| `04-attack-simulations/` | Attack scenario documentation |
| `05-dashboards/` | Kibana dashboard documentation |

---

## Notes

- Kali Linux uses `192.168.70.130`.
- The target server has two IP addresses because it is dual-homed.
- `192.168.70.128` is the target attack-facing IP.
- `192.168.56.30` is the target log-forwarding IP.
- `192.168.56.10` is the SIEM server IP.
- Do not attack the SIEM server.
