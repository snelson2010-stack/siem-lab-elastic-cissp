# Logging and Monitoring Policy

## Purpose

This policy establishes requirements for collecting, monitoring, retaining, and reviewing security-relevant logs within the Elastic SIEM lab.

This Phase 2 policy supports the CISSP Risk Management and Incident Response Expansion by defining what logs should be collected, what activity should be monitored, how evidence should be retained, and how logging gaps should be documented.

---

## Scope

This policy applies to all monitored systems including:

- Elastic SIEM Server
- Ubuntu Linux Target VM
- Windows Enterprise VM
- Elastic Agent managed endpoints
- Fleet-managed endpoint policies
- Kibana Discover investigations
- Screenshot evidence stored in GitHub

---

## Policy Requirements

### Log Collection

Systems must generate and forward security-relevant logs to Elastic when technically possible.

Required Phase 2 Windows monitoring includes:

| Event Type | Event ID / Field | Purpose | Evidence |
|---|---|---|---|
| Failed Windows logon | Event ID 4625 | Detect failed authentication attempts | [Failed Logon 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png) |
| Successful Windows logon | Event ID 4624 | Review successful account access | [Successful Logon 4624](../screenshots/phase-2-windows-successful-logon-4624-discover.png) |
| Endpoint network activity | `event.category: network` | Review reconnaissance or connection activity | [Nmap-Related Network Events](../screenshots/phase-2-nmap-related-windows-network-events.png) |
| Elastic Agent health | Agent status / Fleet status | Confirm endpoint visibility | [Agent Healthy PowerShell](../screenshots/windows-agent-powershell-healthy.png), [Fleet Healthy](../screenshots/fleet-windows-agent-healthy.png) |

---

### Monitoring Requirements

The following activities should be monitored:

- Failed authentication attempts
- Successful authentication events
- Failed logons followed by successful logons
- Administrative activity
- Network reconnaissance activity
- Agent health and connectivity
- Endpoint security events
- Missed check-ins or stopped Elastic Agent service

---

### Required Kibana Searches

Analysts should be able to use or adapt the following searches during review:

```text
host.os.type: "windows" AND event.code: 4625
```

```text
host.os.type: "windows" AND event.code: 4624
```

```text
event.category: network AND source.ip: "192.168.70.130"
```

```text
host.name: "desktop-i2csclu"
```

---

### Agent Health Monitoring

Elastic Agent health must be checked during testing and after missed log events.

Minimum validation methods:

1. Confirm the Windows endpoint appears in Fleet.
2. Confirm the endpoint status is healthy.
3. Confirm the Elastic Agent service is running on the Windows VM.
4. Confirm recent Windows events appear in Kibana Discover.

Evidence:

![Elastic Agent healthy in PowerShell](../screenshots/windows-agent-powershell-healthy.png)

![Windows endpoint healthy in Fleet](../screenshots/fleet-windows-agent-healthy.png)

If the agent stops or logs stop updating, the issue should be documented as an operational monitoring risk.

---

### Evidence Collection

Security evidence should be captured during each detection validation activity.

Evidence should include:

- Screenshot of the query used
- Timestamp or visible time range
- Relevant fields such as `host.name`, `event.code`, `event.action`, `user.name`, `source.ip`, `destination.ip`, `destination.port`, and `message`
- Short explanation of what the screenshot proves
- CISSP domain mapping where applicable

Evidence should be stored under:

```text
03-phase-2-cissp-risk-ir/screenshots/
```

Evidence should also be documented in:

```text
03-phase-2-cissp-risk-ir/screenshots/evidence-index.md
```

---

## Evidence Screenshots

### Failed Windows Logon Event ID 4625

![Failed Windows logon Event ID 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png)

### Successful Windows Logon Event ID 4624

![Successful Windows logon Event ID 4624](../screenshots/phase-2-windows-successful-logon-4624-discover.png)

### Nmap-Related Windows Network Events

![Nmap-related Windows network events](../screenshots/phase-2-nmap-related-windows-network-events.png)

---

## Review Requirements

Logs should be reviewed after:

- Security testing activities
- Incident simulations
- Detection validation exercises
- New endpoint deployments
- Agent restart or recovery events
- Major system changes

Review should answer:

1. Did the expected event appear in Kibana?
2. Were the expected fields populated?
3. Was the event searchable using a repeatable query?
4. Was the evidence captured and stored in GitHub?
5. Was any visibility gap identified?

---

## Visibility Gap Handling

If expected logs are missing or incomplete, document the gap.

Examples from Phase 2:

- Not all Nmap probes appeared as separate Windows endpoint network events.
- `host.ip`, `source.ip`, or `destination.port` may not always be populated depending on the event type.
- Elastic Agent service failure can interrupt log collection.

Recommended actions:

- Enable additional Windows Firewall logging.
- Add network-flow telemetry if available.
- Validate Elastic Defend event coverage.
- Monitor Fleet agent health.
- Document gaps in the risk register.

---

## Retention

Logs and evidence should be retained long enough to support:

- Lab review
- Incident response documentation
- CISSP CPE/CEU evidence
- GitHub portfolio review
- Lessons learned documentation

Screenshots and Markdown evidence should be retained in GitHub as project artifacts.

---

## Related Documents

| Document | Purpose |
|---|---|
| [Evidence Index](../screenshots/evidence-index.md) | Lists and explains Phase 2 screenshot evidence |
| [Windows Enterprise VM Evidence](../windows-enterprise-vm-evidence.md) | Documents the Windows endpoint expansion |
| [Brute Force Playbook](../incident-response/brute-force-incident-playbook.md) | Documents authentication attack response |
| [Nmap Scan Playbook](../incident-response/nmap-scan-incident-playbook.md) | Documents reconnaissance testing and network visibility gaps |
| [Risk Register](../risk-management/risk-register.md) | Tracks risks and mitigations identified during Phase 2 |
| [Detection-Control Matrix](../cissp-mapping/detection-control-matrix.md) | Maps detections to CISSP domains and controls |

---

## CISSP Alignment

| CISSP Domain | Policy Relevance |
|---|---|
| Domain 1: Security and Risk Management | Evidence retention, governance, risk documentation, visibility gap tracking |
| Domain 4: Communication and Network Security | Network telemetry monitoring and reconnaissance review |
| Domain 5: Identity and Access Management | Failed and successful authentication monitoring |
| Domain 6: Security Assessment and Testing | Detection validation and control testing |
| Domain 7: Security Operations | SIEM monitoring, log review, incident response, agent health monitoring |

---

## Summary

This policy supports the Phase 2 CISSP expansion by defining what should be logged, how monitoring should be validated, how evidence should be preserved, and how visibility gaps should be handled. It connects Elastic SIEM monitoring to governance, incident response, and CISSP-aligned security operations.
