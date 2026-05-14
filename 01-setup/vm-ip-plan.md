# VM IP Plan

## Overview

This document defines the virtual machine IP addressing plan for the Elastic SIEM lab.

The lab uses segmented virtual networks to separate SIEM infrastructure from attack simulation traffic. This design helps demonstrate network segmentation, centralized monitoring, and controlled attack testing in a safe lab environment.

---

## Network Segments

| Network | Subnet | Purpose |
|---|---|---|
| VMnet1 | `192.168.56.0/24` | SIEM management and log ingestion network |
| VMnet2 | `192.168.70.0/24` | Attack simulation network |

---

## Virtual Machine IP Assignments

| System | Role | Network Interface | IP Address | Purpose |
|---|---|---|---|---|
| SIEM Server | Elasticsearch / Kibana | VMnet1 | `192.168.56.10` | Receives, stores, and visualizes logs |
| Target Server | Ubuntu log source / victim | VMnet1 | `192.168.56.30` | Sends logs to SIEM |
| Target Server | Ubuntu log source / victim | VMnet2 | `192.168.70.128` | Receives simulated attacks |
| Kali Linux | Attacker | VMnet2 | `192.168.70.x` | Runs attack simulations |

---

## Traffic Flow

```text
Kali Linux Attacker
192.168.70.x
        |
        | Attack traffic: SSH, Nmap, Hydra
        v
Ubuntu Target Server
192.168.70.128
        |
        | Log forwarding through Elastic Agent
        v
SIEM Server
192.168.56.10
        |
        v
Kibana Dashboards and Alerts
```

---

## Which IP Address to Use

### Use `192.168.70.128` for attacks

This is the target server interface on the attack network.

Use this IP when running:

```bash
ssh fakeuser@192.168.70.128
```

```bash
nmap 192.168.70.128
```

```bash
hydra -l analyst -P passwords.txt ssh://192.168.70.128 -t 2
```

---

### Use `192.168.56.10` for SIEM access

This is the Elastic Stack server.

Use this IP when accessing:

```text
https://192.168.56.10:5601
```

or querying Elasticsearch:

```text
https://192.168.56.10:9200
```

---

### Use `192.168.56.30` for log forwarding

This is the target server interface used to communicate with the SIEM server.

Elastic Agent on the target sends logs toward the SIEM server through this side of the network.

---

## Design Principles

### Network Segmentation

- VMnet1 isolates SIEM infrastructure.
- VMnet2 simulates a real-world attack surface.
- Attack traffic and SIEM management traffic are separated.

### Security Monitoring

- Logs are centralized in Elasticsearch.
- Kibana is used for visualization, dashboards, and alerting.
- Elastic Agent collects endpoint telemetry from the target system.

### Least Privilege Design

- Target server only generates and forwards logs.
- SIEM server only receives, stores, and analyzes data.
- Attacker system is isolated on a separate subnet.

---

## Validation Checklist

| Test | Expected Result |
|---|---|
| Kali can reach target attack IP | `192.168.70.128` responds |
| Target can reach SIEM IP | `192.168.56.10` responds |
| Kibana opens in browser | `https://192.168.56.10:5601` loads |
| Elastic Agent is healthy | Target appears healthy in Fleet |
| Failed SSH attempts appear in Kibana | Authentication logs are searchable |

---

## CISSP Domain Mapping

### Security Operations

- Centralized logging and monitoring
- Incident detection and response simulation
- SIEM dashboard analysis

### Security Architecture & Engineering

- Network segmentation design
- Secure data flow between zones
- Separation of attack, target, and monitoring systems

### Communication & Network Security

- Isolated attack network
- Controlled traffic flow between VMnets
- Segmented communication paths

### Security Assessment & Testing

- Simulated brute force activity
- Simulated network scanning
- Validation of monitoring controls

---

## Notes

- Do not mix VMnet1 and VMnet2 IP ranges.
- Attacks should target `192.168.70.128`, not the SIEM server.
- Kibana should be accessed through `192.168.56.10`.
- Elastic Agent should be installed on the target server/log source.
- The SIEM server should not be used as the attack target.
- Keep screenshots of IP configuration for documentation evidence.
