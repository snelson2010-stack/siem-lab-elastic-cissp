# Free SIEM Lab – Elastic Stack SOC Simulation

## Overview

This project demonstrates a Security Information and Event Management (SIEM) lab using the Elastic Stack.

The lab simulates real-world SOC operations including:

- VMware network segmentation
- Linux log ingestion
- Elastic Agent / Fleet enrollment
- SSH authentication monitoring
- Attack simulation
- Detection engineering
- Kibana dashboard creation

This project is designed for cybersecurity learning, SOC analyst practice, and CISSP domain reinforcement.

---

## Lab Architecture

| System | Role | IP Address |
|---|---|---|
| SIEM Server | Elasticsearch / Kibana | `192.168.56.10` |
| Target Server | Log Source / Victim | `192.168.70.128` |
| Target Server | Log Forwarding Interface | `192.168.56.30` |
| Kali Linux | Attacker | `192.168.70.130` |

---

## Network Design

| Network | Subnet | Purpose |
|---|---|---|
| VMnet1 | `192.168.56.0/24` | SIEM infrastructure and log forwarding |
| VMnet2 | `192.168.70.0/24` | Attack simulation network |

---

## Data Flow

```text
Kali Linux Attacker
192.168.70.130
        ↓
Ubuntu Target Server
192.168.70.128
        ↓
Elastic Agent
192.168.56.30
        ↓
Elastic SIEM Server
192.168.56.10
        ↓
Elasticsearch → Kibana → Dashboards → Detections
```

---

## Attack Scenarios Simulated

- SSH failed login attempts
- SSH brute force simulation using Hydra
- Network scanning using Nmap
- Privilege escalation monitoring
- Suspicious authentication activity

---

## Detection Capabilities

- Failed login detection
- SSH brute force monitoring
- Authentication anomaly tracking
- Source IP analysis
- Linux system log monitoring
- Dashboard-based investigation

---

## Repository Structure

```text
01-setup/
├── architecture.md
├── installation.md
├── ip-assignment-plan.md
├── vm-ip-plan.md
├── vmware-setup.md
└── snapshots/

02-ingestion/
├── ingestion-overview.md
├── linux-auth-log.md
├── pipeline-validation.md
├── elastic-agent-status.md
└── ecs-field-mapping.md

03-detections/
04-attack-simulations/
05-dashboards/
06-architecture/
07-troubleshooting/
```

---

## CISSP Domain Alignment

| CISSP Domain | Lab Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | Network segmentation and secure lab design |
| Domain 4: Communication and Network Security | Segmented VMnet traffic and controlled communication |
| Domain 5: Identity and Access Management | SSH authentication monitoring |
| Domain 6: Security Assessment and Testing | Simulated attacks and validation testing |
| Domain 7: Security Operations | SIEM monitoring, dashboards, and detection workflows |

---

## MITRE ATT&CK Mapping

| Technique | Description | Lab Example |
|---|---|---|
| T1110 | Brute Force | Hydra SSH brute force simulation |
| T1046 | Network Service Discovery | Nmap scan against target server |
| T1078 | Valid Accounts | Monitoring successful SSH logins |
| T1059 | Command and Scripting Interpreter | Linux command activity and sudo monitoring |

---

## Skills Demonstrated

- Elastic Stack deployment
- VMware network segmentation
- Elastic Agent / Fleet configuration
- Linux authentication log monitoring
- SIEM log ingestion validation
- Kibana dashboard development
- Threat detection engineering
- SOC-style investigation workflow
- CISSP domain application

---

## Project Status

Completed areas:

- VMware lab setup
- IP assignment documentation
- Elastic Stack installation documentation
- Setup screenshots
- Linux authentication log ingestion documentation
- Elastic Agent status documentation
- ECS field mapping documentation

In progress:

- Detection rules
- Attack simulation documentation
- Kibana dashboards
- Troubleshooting playbooks

---

## Disclaimer

This lab is for educational and cybersecurity training purposes only. All testing is performed in an isolated VMware environment owned and controlled by the lab operator.
