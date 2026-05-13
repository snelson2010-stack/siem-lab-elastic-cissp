# Linux Authentication Log Ingestion

## Overview

This section documents how Linux authentication logs were collected and ingested into the Elastic Stack for security monitoring and detection engineering.

Authentication logs are important because they contain evidence of:

- Failed login attempts
- Successful SSH logins
- Privilege escalation activity
- Invalid user enumeration
- Brute force attacks

These logs were forwarded into Elasticsearch using Elastic Agent / Filebeat and visualized in Kibana dashboards.

---

# Log Source

Primary log file monitored:

```bash
/var/log/auth.log
```

Additional authentication-related logs may include:

```bash
/var/log/secure
```

---

# Environment

| Component | Purpose |
|---|---|
| Ubuntu Linux VM | Target system |
| Kali Linux VM | Attack simulation |
| Elastic Agent | Log collection |
| Elasticsearch | Log storage |
| Kibana | Visualization & analysis |

---

# Elastic Agent Configuration

The Linux integration was configured in Kibana Fleet to collect authentication logs.

Enabled datasets:

- system.auth
- system.syslog

The agent monitored:

```bash
/var/log/auth.log*
```

---

# Verifying Log Collection

Logs were verified locally using:

```bash
sudo tail -f /var/log/auth.log
```

Example authentication events:

```text
Failed password for invalid user fakeuser from 192.168.70.129 port 22 ssh2
Accepted password for analyst from 192.168.70.129 port 22 ssh2
```

---

# Verifying Data in Kibana

Authentication logs were confirmed inside Kibana using Discover.

Example KQL queries:

## All authentication logs

```text
data_stream.dataset : "system.auth"
```

## Failed SSH logins

```text
message : "Failed password"
```

## Invalid user attempts

```text
message : "invalid user"
```

---

# Security Value

Linux authentication logs provide visibility into:

- SSH brute force attacks
- Unauthorized access attempts
- Account misuse
- Privilege escalation activity
- Suspicious login behavior

These logs are critical for SOC monitoring and incident detection.

---

# Related Attack Simulations

The following attack simulations generated authentication events:

- SSH brute force attempts
- Invalid username enumeration
- Failed login testing
- Privilege escalation using sudo

These events were later used in custom detection rules and Kibana dashboards.

---

# Related Detection Engineering

Authentication logs were used to create:

- Failed login detections
- Brute force monitoring
- Top attacker IP dashboards
- Login anomaly visualizations

---

# Screenshots

## Kibana Discover View

_Add screenshot here_

## Failed Login Events

_Add screenshot here_

## Elastic Agent Status

_Add screenshot here_

---

# Lessons Learned

During ingestion testing, several issues were identified and resolved:

- Elastic Agent initially collected only syslog data
- Authentication datasets needed to be enabled manually
- Incorrect file paths prevented log ingestion
- Kibana field mappings required validation

Troubleshooting and validation improved understanding of SIEM data pipelines and Linux log analysis.
