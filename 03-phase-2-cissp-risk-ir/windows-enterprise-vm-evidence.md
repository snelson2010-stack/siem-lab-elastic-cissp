# Windows Enterprise VM Evidence

## Purpose

This document explains the Windows Enterprise VM added during Phase 2 of the CISSP Risk Management and Incident Response Expansion.

The Windows Enterprise VM was added as a new monitored endpoint to expand the original Elastic SIEM lab beyond Linux-focused monitoring. This gives the lab a more realistic enterprise-style environment with Windows endpoint visibility, centralized agent management, and Windows security event detection.

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

The original SIEM project focused on building the Elastic Stack lab, collecting logs, and creating basic visibility into lab systems.

Phase 2 adds a Windows Enterprise VM to demonstrate:

- Windows endpoint monitoring
- Elastic Agent enrollment
- Fleet-based centralized management
- Windows event ingestion
- Failed Windows logon detection
- Evidence collection for incident response
- CISSP domain mapping

This makes Phase 2 a new project expansion rather than a repeat of the original SIEM setup.

---

## Evidence Collected

| Evidence | Screenshot | What It Proves |
|---|---|---|
| Elastic Agent healthy in PowerShell | `screenshots/windows-agent-powershell-healthy.png` | Elastic Agent is installed, running, and connected on the Windows VM |
| Windows endpoint healthy in Fleet | `screenshots/fleet-windows-agent-healthy.png` | The Windows VM is enrolled and centrally managed through Fleet |
| Windows events visible in Kibana Discover | `screenshots/phase-2-windows-events-visible-in-kibana-discover.png` | Windows endpoint events are being indexed and searched in Kibana |
| Windows failed logon detection | `screenshots/phase-2-windows-failed-logon-4625-discover.png` | Failed Windows logon events are visible in Elastic and can support incident response |

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

## CISSP Domain Alignment

| CISSP Domain | How the Windows VM Supports It |
|---|---|
| Domain 1: Security and Risk Management | Supports risk documentation for weak authentication and endpoint visibility |
| Domain 5: Identity and Access Management | Provides evidence of failed authentication monitoring |
| Domain 6: Security Assessment and Testing | Validates that Windows security events can be generated, collected, and reviewed |
| Domain 7: Security Operations | Supports monitoring, detection, investigation, and evidence collection |

---

## Incident Response Value

The Windows VM gives the lab a practical Windows incident response use case. Failed logon events can be investigated using the brute force incident response playbook and mapped to the risk register.

This supports the following incident response steps:

1. Detection of failed authentication events
2. Triage of affected host and account
3. Review of event timeline
4. Evidence collection through Kibana screenshots
5. Documentation of lessons learned
6. Mapping to CISSP domains and security controls

---

## Conclusion

The Windows Enterprise VM is the primary new technical asset added during Phase 2. It expands the SIEM lab into Windows endpoint monitoring and provides direct evidence for authentication monitoring, security operations, incident response, and CISSP-aligned documentation.
