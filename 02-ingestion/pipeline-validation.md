# Ingestion Pipeline Validation

## Purpose

This document validates that Linux authentication events are successfully collected from the target machine, forwarded by Elastic Agent, indexed in Elasticsearch, and searchable in Kibana.

The goal is to prove that the SIEM ingestion pipeline is working from end to end.

---

## Validation Flow

```text
Kali Linux Attack VM
        ↓
Ubuntu Target VM
        ↓
Elastic Agent
        ↓
Elasticsearch
        ↓
Kibana Discover
```

---

## Lab Systems

| System | Role | Purpose |
|---|---|---|
| Kali Linux | Attacker | Generates SSH login attempts |
| Ubuntu Target | Victim / Log Source | Records authentication activity |
| Elastic Agent | Collector | Ships logs to Elastic |
| Elasticsearch | Storage | Stores indexed events |
| Kibana | Analysis | Searches and visualizes events |

---

## Step 1: Generate Authentication Activity

From the Kali Linux machine, generate failed SSH login attempts against the Ubuntu target.

Example:

```bash
ssh fakeuser@192.168.70.128
```

Or generate multiple test attempts using Hydra in the controlled lab environment:

```bash
hydra -l analyst -P passwords.txt ssh://192.168.70.128 -t 2
```

> Note: This activity is performed only against systems owned and controlled in the lab environment.

---

## Step 2: Validate Logs on the Target

On the Ubuntu target machine, verify that authentication events are being generated.

Depending on the Linux distribution and logging configuration, logs may be available through journald or a local log file.

### Check SSH logs with journalctl

```bash
sudo journalctl | grep ssh
```

### Check auth.log if available

```bash
sudo tail -f /var/log/auth.log
```

### Check secure log if available

```bash
sudo tail -f /var/log/secure
```

Expected events may include:

```text
Failed password for invalid user fakeuser from 192.168.70.x
Failed password for analyst from 192.168.70.x
Accepted password for analyst from 192.168.70.x
```

---

## Step 3: Validate Elastic Agent Status

On the target machine:

```bash
sudo elastic-agent status
```

Also verify the service:

```bash
sudo systemctl status elastic-agent
```

In Kibana, navigate to:

```text
Fleet → Agents
```

The target host should show as healthy or online.

---

## Step 4: Validate Events in Kibana Discover

In Kibana, open:

```text
Analytics → Discover
```

Recommended time range:

```text
Last 15 minutes
```

If no logs appear, increase the time range:

```text
Last 30 days
```

---

## Useful KQL Queries

### All authentication dataset events

```kql
data_stream.dataset : "system.auth"
```

### Failed SSH logins

```kql
message : "Failed password"
```

### Invalid user attempts

```kql
message : "invalid user"
```

### Successful SSH logins

```kql
message : "Accepted password"
```

### Sudo activity

```kql
message : "sudo"
```

---

## Evidence Screenshots

Add screenshots after validation is complete.

```md
![Kibana Auth Discover](./screenshots/kibana-auth-discover.png)

![Failed SSH Logins](./screenshots/failed-ssh-logins.png)

![Linux Auth Logs Terminal](./screenshots/linux-authlog-terminal.png)
```

---

## Validation Result

Authentication events were successfully generated on the target system and verified in Kibana Discover.

This confirms that the pipeline from Linux log generation to Elastic search and analysis is operational.

---

## Security Value

Pipeline validation is important because detection rules and dashboards are only useful if the underlying telemetry is reliable.

This validation confirms:

- Linux authentication events are generated
- Elastic Agent is collecting endpoint telemetry
- Events are indexed in Elasticsearch
- Kibana can search and display authentication activity
- The lab can support detection engineering and SOC analysis

---

## CISSP Domain Alignment

| CISSP Domain | Relevance |
|---|---|
| Domain 7: Security Operations | Log monitoring and SIEM validation |
| Domain 6: Security Assessment and Testing | Testing security controls through simulated activity |
| Domain 5: Identity and Access Management | Monitoring authentication attempts |
| Domain 4: Communication and Network Security | Observing SSH access over the lab network |
