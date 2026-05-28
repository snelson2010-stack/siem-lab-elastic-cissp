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

- SSH Brute Force Playbook
- Port Scan Response Playbook
- Privilege Escalation Response Playbook

## Risk Management

- Risk Register
- Risk Analysis Documentation

## Security Governance

- Acceptable Use Policy
- Logging & Monitoring Policy
- Access Control Policy
- Incident Response Policy

## CISSP Mapping

- Detection-to-Control Matrix
- CISSP Domain Mapping Matrix

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

# Suggested Screenshot Evidence

Add screenshots under this phase showing:

- Windows Enterprise VM network configuration
- Elastic Agent installed on Windows
- Windows endpoint enrolled in Fleet
- Windows security events visible in Kibana
- Failed login event or other Windows security event

---

# Conclusion

This project demonstrates how a Security Information and Event Management (SIEM) platform supports enterprise security operations, incident response, governance, and risk management activities. The addition of a Windows Enterprise endpoint expands monitoring capabilities and provides a more realistic enterprise security environment aligned with CISSP principles.
