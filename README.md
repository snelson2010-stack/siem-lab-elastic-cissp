# Free SIEM Lab – Elastic Stack SOC Simulation

## Overview

This project demonstrates a Security Information and Event Management (SIEM) lab using the Elastic Stack.

The lab simulates a small Security Operations Center (SOC) environment where attack activity is generated from Kali Linux, logged by an Ubuntu target server, collected by Elastic Agent, indexed in Elasticsearch, visualized in Kibana, and validated through custom detection rules.

This project is designed for cybersecurity learning, SOC analyst practice, detection engineering, and CISSP domain reinforcement.

---

## Key Results

This lab successfully demonstrates:

- segmented VMware SOC lab architecture
- Elastic Agent / Fleet log collection
- Linux SSH authentication log ingestion
- Kibana Discover validation
- SSH brute force attack simulation using Hydra
- custom Elastic Security threshold rule creation
- high-severity alert generation
- Kibana dashboard visualization
- SOC-style investigation workflow
- troubleshooting and lessons learned documentation

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
Linux authentication logs
        ↓
Elastic Agent / Fleet
192.168.56.30
        ↓
Elastic SIEM Server
192.168.56.10
        ↓
Elasticsearch → Kibana → Dashboards → Detection Rules → Alerts
```

---

## Attack Scenarios Simulated

| Attack Scenario | Tool / Method | Outcome |
|---|---|---|
| SSH failed login attempts | Manual SSH testing | Failed authentication events generated |
| SSH brute force simulation | Hydra | Repeated failed SSH events triggered custom alert |
| SSH port validation | Nmap / Netcat | Confirmed target SSH service was reachable |

---

## Detection Capabilities

| Detection | Field / Logic | Status |
|---|---|---|
| Failed SSH login detection | `system.auth.ssh.event : "Failed"` | Completed |
| SSH brute force threshold rule | Group by `source.ip`, threshold `5` | Completed |
| Alert validation | High-severity alert from `192.168.70.130` | Completed |
| Dashboard-based investigation | Authentication trends and source IP analysis | Completed |

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
├── linux-auth-logs.md
├── pipeline-validation.md
├── elastic-agent-status.md
└── image/

03-detections/
├── detection-overview.md
├── failed-login-detection.md
├── ssh-bruteforce-detection.md
└── image/

04-attack-simulations/
├── ssh-bruteforce.md
└── image/

05-dashboards/
├── authentication-dashboard.md
└── image/

06-architecture/
└── soc-architecture.md

07-troubleshooting/
└── troubleshooting.md
```

---

## Main Documentation

| Section | File | Purpose |
|---|---|---|
| Setup | `01-setup/` | VMware, IP addressing, installation, and snapshots |
| Ingestion | `02-ingestion/pipeline-validation.md` | Validates end-to-end log ingestion |
| Detection | `03-detections/ssh-bruteforce-detection.md` | Documents working SSH brute force threshold alert |
| Attack Simulation | `04-attack-simulations/ssh-bruteforce.md` | Documents Hydra simulation and SSH reachability |
| Dashboards | `05-dashboards/authentication-dashboard.md` | Shows authentication activity and attacker visibility |
| Architecture | `06-architecture/soc-architecture.md` | Summarizes final SOC architecture and data flow |
| Troubleshooting | `07-troubleshooting/troubleshooting.md` | Captures problems, fixes, and lessons learned |

---

## CISSP Domain Alignment

| CISSP Domain | Lab Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | Network segmentation and secure lab design |
| Domain 4: Communication and Network Security | Segmented VMnet traffic and controlled communication |
| Domain 5: Identity and Access Management | SSH authentication monitoring |
| Domain 6: Security Assessment and Testing | Simulated attacks and validation testing |
| Domain 7: Security Operations | SIEM monitoring, dashboards, alerts, and investigation workflows |

---

## MITRE ATT&CK Mapping

| Technique | Description | Lab Example |
|---|---|---|
| T1110 | Brute Force | Hydra SSH brute force simulation |
| T1046 | Network Service Discovery | SSH port validation using Nmap/Netcat |
| T1078 | Valid Accounts | Monitoring successful and failed SSH authentication activity |

---

## Skills Demonstrated

- Elastic Stack deployment
- VMware network segmentation
- Elastic Agent / Fleet configuration
- Linux authentication log monitoring
- SIEM log ingestion validation
- Kibana Discover investigation
- Kibana dashboard development
- Elastic Security custom rule creation
- Threshold-based detection engineering
- Alert validation and triage
- SOC-style investigation workflow
- Troubleshooting documentation
- CISSP domain application

---

## Project Status

Completed areas:

- VMware lab setup
- IP assignment documentation
- Elastic Stack installation documentation
- setup screenshots
- Linux authentication log ingestion documentation
- Elastic Agent / Fleet validation
- ingestion pipeline validation
- custom SSH brute force detection rule
- Elastic Security alert validation
- SSH brute force attack simulation documentation
- Kibana authentication dashboard documentation
- SOC architecture summary
- troubleshooting playbook

Future expansion ideas:

- add firewall or UFW log visibility
- add Nmap scan detection if logs support it
- add sudo privilege monitoring dashboard
- add additional Elastic Security rules
- add Windows or Sysmon telemetry in a future lab phase

---

## Disclaimer

This lab is for educational and cybersecurity training purposes only. All testing is performed in an isolated VMware environment owned and controlled by the lab operator. Do not run attack tools against systems without explicit authorization.
