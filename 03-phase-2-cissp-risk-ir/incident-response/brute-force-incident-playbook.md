# Brute Force Incident Response Playbook

## Purpose

This playbook documents how to detect, triage, contain, eradicate, recover from, and document a suspected brute force authentication attack using the Elastic SIEM lab.

## Scenario

An attacker attempts repeated SSH or Windows login attempts against a monitored endpoint. The SIEM detects multiple failed authentication events from the same source IP address or against the same user account.

## Detection Sources

- Linux authentication logs
- Windows Security Event Logs
- Elastic Agent data
- Kibana Discover
- Kibana Security alerts
- Failed login dashboards

## Detection Indicators

- Multiple failed logins from one source IP
- Repeated attempts against one username
- Failed logins followed by a successful login
- Login attempts outside normal lab activity
- High volume of authentication failures in a short time window

## Example Kibana Searches

```text
message: "Failed password"
```

```text
event.action: "logon-failed"
```

```text
winlog.event_id: 4625
```

```text
user.name: root OR user.name: administrator
```

## Triage Steps

1. Identify the affected host.
2. Identify the source IP address.
3. Determine which user account was targeted.
4. Count the number of failed attempts.
5. Check whether any successful login occurred after the failures.
6. Review related network or process activity.
7. Capture screenshots from Kibana as evidence.

## Containment Steps

- Disable or lock the targeted account if compromise is suspected.
- Block the attacking source IP at the firewall or host level.
- Restrict SSH or RDP access to trusted systems only.
- Require strong passwords or key-based authentication.
- Increase monitoring on the affected host.

## Eradication Steps

- Remove unauthorized accounts if discovered.
- Reset affected account passwords.
- Review privileged group membership.
- Remove persistence mechanisms if found.
- Patch exposed services if outdated.

## Recovery Steps

- Re-enable accounts only after validation.
- Confirm legitimate users can authenticate.
- Verify Elastic Agent is still reporting logs.
- Confirm no continued failed login activity exists.
- Document lessons learned.

## Evidence to Capture

- Failed login events in Kibana
- Source IP address
- Target username
- Timeline of activity
- Any successful login after failed attempts
- Screenshots of dashboard panels or Discover results

## CISSP Domain Mapping

| CISSP Domain | Relevance |
|---|---|
| Domain 1: Security and Risk Management | Risk identification and response documentation |
| Domain 5: Identity and Access Management | Authentication monitoring and account protection |
| Domain 6: Security Assessment and Testing | Validation of detection capability |
| Domain 7: Security Operations | Incident detection, triage, containment, and evidence collection |

## Lessons Learned

This activity demonstrates how SIEM monitoring supports authentication security, access control, incident response, and operational security decision-making.
