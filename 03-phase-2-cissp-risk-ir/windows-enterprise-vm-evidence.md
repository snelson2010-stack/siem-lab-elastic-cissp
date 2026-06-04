# Windows Enterprise VM Evidence

## Purpose

This document explains the Windows Enterprise VM added during Phase 2 of the CISSP Risk Management and Incident Response Expansion.

The Windows Enterprise VM was added as a new monitored endpoint to expand the original Elastic SIEM lab beyond Linux-focused monitoring. This gives the lab a more realistic enterprise-style environment with Windows endpoint visibility, centralized agent management, Windows security event detection, and network telemetry collection.

---

## New Phase 2 Asset

| Asset | Role |
|---|---|
| Windows Enterprise VM | New monitored endpoint |
| Elastic Agent | Log collection and endpoint telemetry |
| Fleet | Centralized agent management |
| Elasticsearch | Event storage and indexing |
| Kibana Discover | Event search and investigation |

---

## What This Adds to the Original Project

The original SIEM project focused on building the Elastic Stack lab, collecting Linux logs, and creating basic visibility into lab systems.

Phase 2 adds a Windows Enterprise VM to demonstrate:

- Windows endpoint monitoring
- Elastic Agent enrollment
- Fleet-based centralized management
- Windows event ingestion
- Failed Windows logon detection
- Windows network telemetry review
- Evidence collection for incident response
- CISSP domain mapping

This makes Phase 2 a new project expansion rather than a repeat of the original SIEM setup.

---

## Evidence Collected

| Evidence | Screenshot | What It Proves |
|---|---|---|
| Elastic Agent healthy in PowerShell | [windows-agent-powershell-healthy.png](screenshots/windows-agent-powershell-healthy.png) | Elastic Agent is installed, running, and connected on the Windows VM |
| Windows endpoint healthy in Fleet | [fleet-windows-agent-healthy.png](screenshots/fleet-windows-agent-healthy.png) | The Windows VM is enrolled and centrally managed through Fleet |
| Windows failed logon detection | [phase-2-windows-failed-logon-4625-discover.png](screenshots/phase-2-windows-failed-logon-4625-discover.png) | Failed Windows logon events are visible in Elastic and can support incident response |
| Windows network telemetry during Nmap-related testing | [phase-2-nmap-related-windows-network-events.png](screenshots/phase-2-nmap-related-windows-network-events.png) | Windows endpoint network events are visible during reconnaissance testing |
| Windows agent details and policy evidence | [windows-agent-policy-or-agent-details.png](screenshots/windows-agent-policy-or-agent-details.png) | Shows the Windows endpoint policy and configuration details |

---

## Evidence Screenshots

### Elastic Agent Healthy in PowerShell

![Elastic Agent healthy in PowerShell](screenshots/windows-agent-powershell-healthy.png)

### Windows Endpoint Healthy in Fleet

![Windows endpoint healthy in Fleet](screenshots/fleet-windows-agent-healthy.png)

### Windows Failed Logon Detection

![Windows failed logon Event ID 4625 in Kibana Discover](screenshots/phase-2-windows-failed-logon-4625-discover.png)

### Windows Network Telemetry During Nmap-Related Testing

![Windows network telemetry during Nmap-related testing](screenshots/phase-2-nmap-related-windows-network-events.png)

### Windows Agent Details and Policy Evidence

![Windows agent details and policy evidence](screenshots/windows-agent-policy-or-agent-details.png)

---

## Failed Windows Logon Detection

A failed Windows logon test was performed by entering the wrong password multiple times on the Windows Enterprise VM.

The resulting events were found in Kibana Discover using a query similar to:

```text
host.os.type: "windows" AND winlog.event_id: 4625
```

The screenshot shows multiple failed logon events with the message:

```text
An account failed to log on.
```

This confirms that the lab can detect Windows authentication failures and use them as evidence for a security investigation.

---

## Network Telemetry and Reconnaissance Testing

Additional testing was performed using Nmap from the Kali attacker VM.

Windows endpoint network telemetry was reviewed in Kibana Discover to identify activity associated with the testing window.

The collected evidence demonstrates:

- Windows endpoint network visibility
- Correlation between attacker activity and endpoint telemetry
- Investigation of network-related events within Kibana
- Identification of visibility gaps where additional firewall or network-flow logging could improve detection coverage

This provides a practical example of detection validation and security operations analysis.

---

## CISSP Domain Alignment

| CISSP Domain | How the Windows VM Supports It |
|---|---|
| Domain 1: Security and Risk Management | Supports risk documentation for weak authentication and endpoint visibility |
| Domain 4: Communication and Network Security | Supports network monitoring and reconnaissance analysis |
| Domain 5: Identity and Access Management | Provides evidence of failed authentication monitoring |
| Domain 6: Security Assessment and Testing | Validates that Windows security events can be generated, collected, and reviewed |
| Domain 7: Security Operations | Supports monitoring, detection, investigation, and evidence collection |

---

## Incident Response Value

The Windows VM gives the lab practical incident response use cases.

Examples include:

1. Failed Windows authentication events (Event ID 4625)
2. Network reconnaissance review and validation
3. Endpoint investigation using Kibana Discover
4. Evidence collection through screenshots and timelines
5. Mapping findings to CISSP domains and security controls

---

## Conclusion

The Windows Enterprise VM is the primary new technical asset added during Phase 2. It expands the SIEM lab into Windows endpoint monitoring and provides direct evidence for authentication monitoring, network telemetry analysis, security operations, incident response, and CISSP-aligned documentation.