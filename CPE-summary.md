# ISC2 CPE Summary

## Activity Title

Elastic Stack SIEM Lab: SOC Monitoring, Detection Engineering, and Incident Analysis

---

## Activity Type

Self-study / professional development / cybersecurity lab project

---

## Date Range

| Item | Date |
|---|---|
| Start date | April 8, 2026 |
| End date | May 14, 2026 |

---

## Suggested CPE Hours

| Activity Area | Estimated Hours |
|---|---:|
| VMware lab setup and network segmentation | 4 |
| Elastic Stack installation and configuration | 3 |
| Elastic Agent / Fleet ingestion setup | 3 |
| Linux authentication log validation | 2 |
| SSH brute force attack simulation | 2 |
| Custom detection rule and alert validation | 3 |
| Kibana dashboard creation and documentation | 1 |
| Total | 18 |

Suggested reported total:

```text
18 CPE hours
```

> Note: CPE hours should reflect the actual time spent completing and documenting the activity.

---

## Project Summary

This project built and documented a home SIEM lab using Elastic Stack, VMware, Kali Linux, and Ubuntu.

The lab included segmented network architecture, Elastic Agent log ingestion, Linux SSH authentication monitoring, Hydra-based brute force simulation, custom Elastic Security threshold detection, alert validation, Kibana dashboards, and troubleshooting documentation.

The project reinforced hands-on skills related to SOC monitoring, detection engineering, log analysis, incident investigation, and security architecture.

---

## Learning Objectives

The objectives of this project were to:

- Build a segmented virtual SOC lab environment
- Deploy and validate Elastic Stack components
- Collect Linux authentication logs using Elastic Agent
- Validate log ingestion in Kibana Discover
- Simulate SSH authentication attacks in an isolated lab
- Create and test custom Elastic Security detection rules
- Generate and validate SIEM alerts
- Build dashboards for SOC-style monitoring
- Document troubleshooting steps and lessons learned
- Map the project to CISSP domains and MITRE ATT&CK techniques

---

## Evidence Produced

The GitHub repository includes evidence of:

- VMware network segmentation
- IP assignment planning
- Elastic Stack installation
- Elastic Agent status validation
- Linux authentication log ingestion
- Pipeline validation
- Hydra SSH brute force simulation
- SSH port validation
- Kibana Discover search results
- Custom threshold rule configuration
- High-severity alert generation
- Alert JSON validation
- Authentication dashboard screenshots
- SOC architecture documentation
- Troubleshooting documentation

---

## CISSP Domain Mapping

| CISSP Domain | Project Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | VMware network segmentation, SOC architecture, secure lab design |
| Domain 4: Communication and Network Security | Segmented VMnet traffic, SSH communication, controlled traffic flow |
| Domain 5: Identity and Access Management | SSH authentication monitoring, failed login analysis, user activity review |
| Domain 6: Security Assessment and Testing | Controlled Hydra simulation, validation testing, detection verification |
| Domain 7: Security Operations | SIEM monitoring, alerting, dashboards, investigation workflow, troubleshooting |

---

## MITRE ATT&CK Mapping

| Technique | Description | Lab Evidence |
|---|---|---|
| T1110 | Brute Force | Hydra SSH brute force simulation and failed login alert |
| T1046 | Network Service Discovery | SSH port validation using Nmap/Netcat |
| T1078 | Valid Accounts | Monitoring successful and failed SSH authentication activity |

---

## Skills Demonstrated

- Elastic Stack deployment
- VMware network segmentation
- Elastic Agent and Fleet configuration
- Linux authentication log monitoring
- SIEM ingestion validation
- Kibana Discover investigation
- Kibana dashboard development
- Elastic Security custom threshold rule creation
- Alert validation and triage
- Detection engineering
- SOC-style investigation workflow
- Cybersecurity troubleshooting documentation

---

## Suggested ISC2 Submission Description

Built and documented a home SIEM lab using Elastic Stack, VMware, Kali Linux, and Ubuntu. The lab included segmented network architecture, Elastic Agent log ingestion, Linux SSH authentication monitoring, Hydra-based brute force simulation, custom Elastic Security threshold detection, alert validation, Kibana dashboards, and troubleshooting documentation. The project reinforced CISSP domains related to Security Architecture, Communication and Network Security, Identity and Access Management, Security Assessment and Testing, and Security Operations.

---

## Supporting Repository

Repository:

```text
siem-lab-elastic-cissp
```

Primary documentation files:

```text
README.md
02-ingestion/pipeline-validation.md
03-detections/ssh-bruteforce-detection.md
04-attack-simulations/ssh-bruteforce.md
05-dashboards/authentication-dashboard.md
06-architecture/soc-architecture.md
07-troubleshooting/troubleshooting.md
```

---

## Disclaimer

This activity was performed in an isolated VMware lab environment owned and controlled by the lab operator. The attack simulation was conducted only against lab systems for educational and professional development purposes.
