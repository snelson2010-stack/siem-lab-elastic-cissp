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

## CISSP Domain Mapping

| CISSP Domain | Relevance |
|---|---|
| Domain 4: Communication and Network Security | Network traffic monitoring and segmentation review |
| Domain 6: Security Assessment and Testing | Detection validation and control testing |
| Domain 7: Security Operations | Monitoring, triage, containment, and evidence collection |

## Lessons Learned

Port scanning is often an early reconnaissance step before exploitation. Detecting this activity helps security teams identify potential threats before an attacker moves to credential attacks or service exploitation.
