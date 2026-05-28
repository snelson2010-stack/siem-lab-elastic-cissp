# Phase 2 Screenshot Evidence Index

## Purpose

This document explains the screenshots collected for Phase 2 of the CISSP Risk Management and Incident Response Expansion. The screenshots provide evidence that a Windows Enterprise VM was added as a new monitored endpoint and that the Elastic SIEM lab is collecting Windows endpoint data.

---

## Evidence Summary

| Evidence Item | Screenshot File | What It Proves | CISSP Domain Alignment |
|---|---|---|---|
| Windows Elastic Agent status from PowerShell | `windows-agent-powershell-healthy.png` | Shows the Windows Enterprise VM has Elastic Agent installed, running, and connected to Fleet | Domain 7: Security Operations |
| Windows endpoint healthy in Fleet | `fleet-windows-agent-healthy.png` | Shows the Windows VM is enrolled in Fleet and reporting as healthy under the Windows target policy | Domain 7: Security Operations |
| Windows events visible in Kibana Discover | `phase-2-windows-events-visible-in-kibana-discover.png` | Shows Windows endpoint data is being ingested and searchable in Kibana | Domain 5: Identity and Access Management; Domain 7: Security Operations |
| Windows agent details or policy evidence | `windows-agent-policy-or-agent-details.png` | Shows the Windows endpoint policy, integrations, or agent details used for log collection | Domain 6: Security Assessment and Testing; Domain 7: Security Operations |

---

## Evidence 1: Windows Elastic Agent Healthy in PowerShell

This screenshot shows the Elastic Agent status from PowerShell on the Windows Enterprise VM. The output confirms that the agent is running and connected to Fleet.

### Security Value

This proves the Windows VM is no longer just a standalone virtual machine. It is now part of the monitored SIEM environment.

### CISSP Relevance

- Domain 7: Security Operations
- Monitoring and logging
- Endpoint visibility
- Operational security validation

---

## Evidence 2: Windows Endpoint Healthy in Fleet

This screenshot shows the Windows Enterprise VM listed in Kibana Fleet with a healthy status. It also shows the assigned Windows agent policy.

### Security Value

This proves centralized management is working and that the Windows endpoint can be monitored through Fleet.

### CISSP Relevance

- Domain 7: Security Operations
- Centralized monitoring
- Asset visibility
- Endpoint management

---

## Evidence 3: Windows Events Visible in Kibana Discover

This screenshot shows a Kibana Discover search using:

```text
host.os.type: "windows"
```

The results show Windows endpoint events arriving in Elastic.

### Security Value

This is the strongest evidence that Windows logs are being collected and indexed. It confirms that the new Phase 2 Windows endpoint is producing searchable SIEM data.

### CISSP Relevance

- Domain 5: Identity and Access Management
- Domain 7: Security Operations
- Log collection
- Event monitoring
- Evidence collection

---

## Evidence 4: Windows Agent Policy or Agent Details

This screenshot should show the Windows endpoint details, agent policy, integrations, last check-in, or other Fleet details.

### Security Value

This supports the configuration side of the project and shows how the Windows endpoint is managed within the SIEM environment.

### CISSP Relevance

- Domain 6: Security Assessment and Testing
- Domain 7: Security Operations
- Configuration validation
- Control testing

---

## Phase 2 Justification

These screenshots demonstrate that Phase 2 added new capabilities to the original SIEM lab. The original project focused on building the Elastic SIEM environment. Phase 2 expands the environment by adding a Windows Enterprise endpoint and using it for monitoring, evidence collection, incident response, and CISSP domain mapping.

This evidence supports the claim that Phase 2 is a new project expansion rather than a repeat of the original SIEM build.

---

## Suggested Next Evidence

Additional evidence that would strengthen the project:

- Failed Windows logon event, such as Event ID 4625
- Successful Windows logon event, such as Event ID 4624
- Windows process event from Elastic Defend
- Dashboard showing Windows endpoint activity
- Incident response notes tied to a Windows authentication event
