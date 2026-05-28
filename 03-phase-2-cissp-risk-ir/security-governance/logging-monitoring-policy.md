# Logging and Monitoring Policy

## Purpose

This policy establishes requirements for collecting, monitoring, retaining, and reviewing security-relevant logs within the Elastic SIEM lab.

## Scope

This policy applies to all monitored systems including:

- Elastic SIEM Server
- Ubuntu Linux Target VM
- Windows Enterprise VM
- Elastic Agent managed endpoints

## Policy Requirements

### Log Collection

- Systems must generate security-relevant logs.
- Elastic Agent must be installed on monitored endpoints where applicable.
- Authentication events should be collected and retained.

### Monitoring

The following activities should be monitored:

- Failed authentication attempts
- Administrative activity
- Network reconnaissance activity
- Agent health and connectivity
- Endpoint security events

### Review

Logs should be reviewed after:

- Security testing activities
- Incident simulations
- Detection validation exercises
- Major system changes

### Retention

Logs should be retained according to available storage capacity and lab requirements.

## CISSP Alignment

- Domain 1: Security and Risk Management
- Domain 7: Security Operations
