# Nmap Scan Incident Response Playbook

## Purpose

This playbook documents how to identify, investigate, contain, and document suspected port scanning or network reconnaissance activity using the Elastic SIEM lab.

## Scenario

A Kali Linux attacker system performs network reconnaissance against a monitored endpoint. The activity may include TCP port scans, service discovery, or repeated connection attempts to multiple ports.

## Detection Sources

- Elastic Agent endpoint events
- Linux system logs
- Windows endpoint events
- Firewall logs, if enabled
- Kibana Discover
- Elastic Security alerts

## Detection Indicators

- Multiple connection attempts from one source IP
- Connections to many destination ports in a short time period
- Nmap-like scan behavior
- Unexpected inbound traffic to monitored endpoints
- Repeated failed connection attempts

## Example Kibana Searches

```text
source.ip: * AND destination.port: *
```

```text
event.category: network
```

```text
process.name: nmap
```

```text
host.os.type: "windows" AND event.category: network
```

```text
event.category: network AND source.ip: "192.168.70.130"
```

```text
event.category: network AND source.ip: "192.168.70.130" AND destination.ip: "192.168.70.140"
```

## Triage Steps

1. Identify the source IP address.
2. Identify the destination host.
3. Review destination ports targeted.
4. Determine whether the activity is expected lab testing or unauthorized scanning.
5. Check for follow-on activity such as failed logons or exploit attempts.
6. Capture Kibana screenshots as evidence.

## Containment Steps

- Block or isolate the scanning source if unauthorized.
- Restrict unnecessary open ports.
- Confirm firewall rules are correct.
- Increase monitoring on targeted systems.

## Eradication Steps

- Remove unauthorized scanning tools if found on an internal system.
- Review endpoint security alerts.
- Validate that no services were exploited after reconnaissance.

## Recovery Steps

- Confirm monitored systems remain stable.
- Validate that Elastic Agent is still reporting.
- Review exposed services and reduce unnecessary attack surface.
- Document lessons learned.

## Evidence to Capture

- Timeline of scan activity
- Source IP address
- Destination IP address
- Destination ports
- Kibana Discover results
- Dashboard screenshots

## Lab Evidence Collected

During Phase 2 testing, Nmap reconnaissance was performed from the Kali attacker VM against the Windows Enterprise VM.

The following evidence files support this activity:

| Evidence File | Description |
|---|---|
| `../screenshots/phase-2-nmap-kali-scan-command.png` | Shows the Nmap command executed from the Kali attacker VM with timestamp context |
| `../screenshots/phase-2-nmap-related-windows-network-events.png` | Shows Windows endpoint network telemetry in Kibana Discover associated with the Kali source IP |

The Discover evidence was reviewed using fields such as:

- `@timestamp`
- `host.name`
- `host.os.type`
- `event.category`
- `event.action`
- `source.ip`
- `destination.ip`
- `destination.port`
- `network.transport`
- `process.name`
- `message`

The observed network events showed TCP activity from the Kali source IP to the Windows endpoint. In the lab, only limited endpoint network events were visible, which indicates that additional firewall logging or network-flow collection would improve scan visibility.

## Visibility Gap Identified

The Windows endpoint produced network telemetry, but not every Nmap probe appeared as a separate event in Kibana Discover. This suggests a detection visibility gap.

Recommended improvements:

- Enable Windows Firewall logging.
- Add additional network telemetry collection.
- Validate whether Elastic Defend network events include all desired connection attempts.
- Consider adding a dedicated network sensor or firewall log source.
- Correlate Kali command output, timestamps, and endpoint events for stronger attribution.

## CISSP Domain Mapping

| CISSP Domain | Relevance |
|---|---|
| Domain 4: Communication and Network Security | Network traffic monitoring and segmentation review |
| Domain 6: Security Assessment and Testing | Detection validation and control testing |
| Domain 7: Security Operations | Monitoring, triage, containment, and evidence collection |

## Lessons Learned

Port scanning is often an early reconnaissance step before exploitation. Detecting this activity helps security teams identify potential threats before an attacker moves to credential attacks or service exploitation.

This lab also demonstrates that detection testing should include validation of logging coverage. If scan traffic is only partially visible, that finding should be documented as a visibility gap and used to improve monitoring controls.
