# Detection to Control Matrix

## Purpose

This matrix maps Phase 2 SIEM detections to CISSP domains, security objectives, evidence artifacts, and related response documentation.

The goal is to show how technical monitoring in Elastic supports governance, risk management, incident response, access control monitoring, and security operations.

---

## Detection and Control Matrix

| Detection / Control | Data Source | Tool | Evidence | Related Documentation | CISSP Domain |
|---|---|---|---|---|---|
| Windows failed logon detection | Windows Security Events, Event ID 4625 | Elastic Agent, Kibana Discover | [Failed Logon 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png) | [Brute Force Playbook](../incident-response/brute-force-incident-playbook.md), [Risk Register](../risk-management/risk-register.md) | Domain 5, Domain 6, Domain 7 |
| Windows successful logon monitoring | Windows Security Events, Event ID 4624 | Elastic Agent, Kibana Discover | [Successful Logon 4624](../screenshots/phase-2-windows-successful-logon-4624-discover.png) | [Brute Force Playbook](../incident-response/brute-force-incident-playbook.md), [Windows VM Evidence](../windows-enterprise-vm-evidence.md) | Domain 5, Domain 7 |
| Failed logons followed by successful logon | Windows Security Events, Event ID 4625 and 4624 | Kibana Discover, Elastic SIEM | [Failed Logon 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png), [Successful Logon 4624](../screenshots/phase-2-windows-successful-logon-4624-discover.png) | [Brute Force Playbook](../incident-response/brute-force-incident-playbook.md), [Risk Register](../risk-management/risk-register.md) | Domain 1, Domain 5, Domain 7 |
| Nmap-related network telemetry review | Windows endpoint network events | Elastic Agent, Kibana Discover | [Nmap-Related Network Events](../screenshots/phase-2-nmap-related-windows-network-events.png) | [Nmap Playbook](../incident-response/nmap-scan-incident-playbook.md), [Risk Register](../risk-management/risk-register.md) | Domain 4, Domain 6, Domain 7 |
| Network visibility gap identification | Endpoint network telemetry review | Kibana Discover, Elastic SIEM | [Nmap-Related Network Events](../screenshots/phase-2-nmap-related-windows-network-events.png) | [Nmap Playbook](../incident-response/nmap-scan-incident-playbook.md), [Risk Register](../risk-management/risk-register.md) | Domain 1, Domain 4, Domain 6, Domain 7 |
| Elastic Agent health monitoring | Elastic Agent status and Fleet check-in | PowerShell, Fleet | [Agent Healthy PowerShell](../screenshots/windows-agent-powershell-healthy.png), [Fleet Healthy](../screenshots/fleet-windows-agent-healthy.png) | [Windows VM Evidence](../windows-enterprise-vm-evidence.md), [Risk Register](../risk-management/risk-register.md) | Domain 7 |
| Windows endpoint enrollment validation | Fleet agent enrollment and policy assignment | Fleet | [Fleet Healthy](../screenshots/fleet-windows-agent-healthy.png), [Agent Policy Details](../screenshots/windows-agent-policy-or-agent-details.png) | [Windows VM Evidence](../windows-enterprise-vm-evidence.md) | Domain 6, Domain 7 |
| Log ingestion validation | Windows endpoint events indexed in Elasticsearch | Kibana Discover | [Evidence Index](../screenshots/evidence-index.md) | [Windows VM Evidence](../windows-enterprise-vm-evidence.md) | Domain 6, Domain 7 |
| Privileged activity monitoring | Windows Security Events and Linux sudo logs | Elastic SIEM, Kibana Discover | Future evidence to be added | [Privilege Escalation Playbook](../incident-response/privilege-escalation-playbook.md) | Domain 5, Domain 7 |
| Logging and monitoring governance | SIEM policies and monitoring requirements | Documentation, Elastic SIEM | [Logging Policy](../security-governance/logging-monitoring-policy.md) | [Risk Register](../risk-management/risk-register.md) | Domain 1, Domain 7 |

---

## Evidence Screenshots

### Windows Failed Logon Detection

![Windows failed logon Event ID 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png)

### Windows Successful Logon Monitoring

![Windows successful logon Event ID 4624](../screenshots/phase-2-windows-successful-logon-4624-discover.png)

### Nmap-Related Network Telemetry

![Nmap-related Windows network events](../screenshots/phase-2-nmap-related-windows-network-events.png)

### Elastic Agent Health

![Elastic Agent healthy in PowerShell](../screenshots/windows-agent-powershell-healthy.png)

### Fleet Enrollment and Health

![Windows endpoint healthy in Fleet](../screenshots/fleet-windows-agent-healthy.png)

---

## CISSP Domain Summary

| CISSP Domain | Phase 2 Control Examples |
|---|---|
| Domain 1: Security and Risk Management | Risk register, visibility gap documentation, governance policy |
| Domain 4: Communication and Network Security | Network telemetry review, Nmap-related reconnaissance analysis |
| Domain 5: Identity and Access Management | Failed logon monitoring, successful logon monitoring, account activity review |
| Domain 6: Security Assessment and Testing | Detection validation, control testing, evidence collection |
| Domain 7: Security Operations | SIEM monitoring, incident response, Fleet health, endpoint monitoring |

---

## Notes

This matrix shows that Phase 2 is not only a technical lab expansion. It connects Windows endpoint telemetry, authentication monitoring, agent health, and reconnaissance testing to CISSP-aligned controls and documentation.

The Nmap testing also identified a visibility gap: not every probe appeared as a separate endpoint event in Discover. This finding was documented as a risk and used to recommend additional firewall or network-flow logging.
