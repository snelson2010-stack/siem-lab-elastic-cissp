# Privilege Escalation Incident Response Playbook

## Purpose

This playbook documents how to investigate suspected privilege escalation activity on monitored Linux or Windows systems.

## Scenario

A user account gains elevated privileges or executes administrative actions that may indicate misuse, compromise, or unauthorized access.

## Detection Sources

- Linux sudo logs
- Windows Security Event Logs
- Elastic Agent endpoint telemetry
- Kibana Discover
- Elastic Security alerts

## Detection Indicators

- Unexpected sudo usage
- Administrative logons outside normal activity
- Privileged process execution
- User added to administrative groups
- Failed logons followed by privileged activity

## Example Kibana Searches

```text
message: sudo
```

```text
event.code: 4672
```

```text
user.name: administrator
```

## Triage Steps

1. Identify the affected host.
2. Identify the account involved.
3. Review the timeline of events.
4. Determine whether the activity was authorized.
5. Check for account compromise indicators.
6. Capture evidence screenshots.

## Containment Steps

- Disable compromised accounts if necessary.
- Remove unauthorized privileged access.
- Restrict administrative access paths.

## Recovery Steps

- Reset credentials.
- Validate group memberships.
- Review security logs for persistence activity.
- Confirm normal operations.

## CISSP Domain Mapping

- Domain 5: Identity and Access Management
- Domain 6: Security Assessment and Testing
- Domain 7: Security Operations
