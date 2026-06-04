# Phase 2: CISSP Risk Management & Incident Response Expansion

## Overview

This phase expands the original Elastic SIEM Lab into a CISSP-aligned Security Operations, Risk Management, and Incident Response project.

The original project focused on:

- Elastic Stack deployment
- Log ingestion
- Dashboard creation
- Attack simulation
- Security monitoring

This phase builds upon that foundation by demonstrating how security monitoring supports governance, risk management, incident response, and access control concepts found throughout the CISSP Common Body of Knowledge (CBK).

---

# New Phase 2 Asset: Windows Enterprise VM

## Purpose

A Windows Enterprise virtual machine was added to expand the lab beyond Linux monitoring and provide visibility into Windows security events.

This allows the SIEM environment to collect and analyze:

- Windows Security Logs
- Authentication Events
- Failed Logon Attempts
- Successful Logon Events
- Account Management Events
- Privilege Use Events
- Endpoint Activity

---

## Windows Enterprise VM Details

| Asset | Purpose |
|---|---|
| Windows Enterprise VM | Monitored Endpoint |
| Elastic Agent | Log Collection |
| Fleet Management | Centralized Agent Management |
| Kibana | Security Monitoring |
| Elasticsearch | Log Storage & Analysis |

---

## Updated Lab Architecture

```text
                   ┌────────────────────┐
                   │     Kali Linux     │
                   │     Attacker VM    │
                   └─────────┬──────────┘
                             │
                             ▼
┌──────────────────────────────────────────────┐
│               Target Systems                 │
├──────────────────────────────────────────────┤
│ Ubuntu Linux Target VM                       │
│ Windows Enterprise VM                        │
└───────────────────┬──────────────────────────┘
                    │
                    ▼
          ┌─────────────────────┐
          │    Elastic Agent    │
          └─────────┬───────────┘
                    │
                    ▼
          ┌─────────────────────┐
          │   Elasticsearch     │
          └─────────┬───────────┘
                    │
                    ▼
          ┌─────────────────────┐
          │      Kibana         │
          │ Security Dashboard  │
          └─────────────────────┘
```

---

# What Makes This Phase New

This phase is a separate project expansion and does not repeat the original SIEM installation.

New work completed during Phase 2 includes:

- Windows Enterprise endpoint deployment
- Elastic Agent enrollment
- Windows event monitoring
- Incident response documentation
- Risk management documentation
- Governance policy development
- CISSP control mapping
- Detection-to-control matrix creation
- Windows authentication and network monitoring dashboards

---

# Evidence

| Evidence | Link |
|---|---|
| Windows Enterprise VM Evidence Summary | [windows-enterprise-vm-evidence.md](windows-enterprise-vm-evidence.md) |
| Phase 2 Summary Report | [phase-2-summary-report.md](phase-2-summary-report.md) |
| Screenshot Evidence Index | [screenshots/evidence-index.md](screenshots/evidence-index.md) |
| Elastic Agent Healthy in PowerShell | [windows-agent-powershell-healthy.png](screenshots/windows-agent-powershell-healthy.png) |
| Windows Endpoint Healthy in Fleet | [fleet-windows-agent-healthy.png](screenshots/fleet-windows-agent-healthy.png) |
| Windows Failed Logon Event ID 4625 | [phase-2-windows-failed-logon-4625-discover.png](screenshots/phase-2-windows-failed-logon-4625-discover.png) |
| Windows Successful Logon Event ID 4624 | [phase-2-windows-successful-logon-4624-discover.png](screenshots/phase-2-windows-successful-logon-4624-discover.png) |
| Nmap-Related Windows Network Events | [phase-2-nmap-related-windows-network-events.png](screenshots/phase-2-nmap-related-windows-network-events.png) |
| Windows Authentication Dashboard | [phase-2-windows-authentication-dashboard.png](screenshots/phase-2-windows-authentication-dashboard.png) |
| Windows Network Monitoring Dashboard | [phase-2-windows-network-monitoring-dashboard.png](screenshots/phase-2-windows-network-monitoring-dashboard.png) |
| Windows Agent Policy or Details | [windows-agent-policy-or-agent-details.png](screenshots/windows-agent-policy-or-agent-details.png) |

## Key Evidence Screenshots

### Windows Endpoint Healthy in Fleet

![Windows endpoint healthy in Fleet](screenshots/fleet-windows-agent-healthy.png)

### Failed Windows Logon Event ID 4625

![Windows failed logon Event ID 4625](screenshots/phase-2-windows-failed-logon-4625-discover.png)

### Successful Windows Logon Event ID 4624

![Windows successful logon Event ID 4624](screenshots/phase-2-windows-successful-logon-4624-discover.png)

### Windows Authentication Monitoring Dashboard

![Windows authentication monitoring dashboard](screenshots/phase-2-windows-authentication-dashboard.png)

### Nmap-Related Windows Network Events

![Nmap-related Windows network events](screenshots/phase-2-nmap-related-windows-network-events.png)

### Windows Network Monitoring Dashboard

![Windows network monitoring dashboard](screenshots/phase-2-windows-network-monitoring-dashboard.png)

---

# CISSP Domain Alignment

## Domain 1 – Security & Risk Management

- Risk Register
- Security Policies
- Governance Documentation

## Domain 4 – Communication & Network Security

- Network Monitoring
- Attack Detection
- Scan Detection

## Domain 5 – Identity & Access Management

- Failed Login Monitoring
- Successful Login Monitoring
- Account Activity Monitoring
- Privileged Access Monitoring

## Domain 6 – Security Assessment & Testing

- Attack Simulations
- Detection Validation
- Control Testing

## Domain 7 – Security Operations

- SIEM Monitoring
- Incident Response
- Event Analysis
- Evidence Collection

---

# Phase 2 Deliverables

## Incident Response

- [SSH Brute Force Playbook](incident-response/brute-force-incident-playbook.md)
- [Port Scan / Nmap Response Playbook](incident-response/nmap-scan-incident-playbook.md)
- [Privilege Escalation Response Playbook](incident-response/privilege-escalation-playbook.md)

## Risk Management

- [Risk Register](risk-management/risk-register.md)

## Security Governance

- [Logging & Monitoring Policy](security-governance/logging-monitoring-policy.md)

## CISSP Mapping

- [Detection-to-Control Matrix](cissp-mapping/detection-control-matrix.md)

---

# Skills Demonstrated

- SIEM Administration
- Elastic Stack
- Windows Security Monitoring
- Linux Security Monitoring
- Incident Response
- Risk Management
- Governance Documentation
- Threat Detection
- Security Operations
- CISSP Domain Mapping

---

# Conclusion

This project demonstrates how a Security Information and Event Management (SIEM) platform supports enterprise security operations, incident response, governance, and risk management activities. The addition of a Windows Enterprise endpoint expands monitoring capabilities and provides a more realistic enterprise security environment aligned with CISSP principles.
