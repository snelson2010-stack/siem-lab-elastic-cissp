# Troubleshooting

## Purpose

This document captures issues encountered while building the Elastic SIEM lab and the steps used to resolve them.

Troubleshooting is an important part of SOC engineering because detections, dashboards, and alerts depend on reliable network connectivity, log ingestion, field parsing, and rule execution.

---

## Troubleshooting Summary

| Issue | Area | Resolution |
|---|---|---|
| Target logs not visible in Kibana | Ingestion | Verified Elastic Agent health and Discover time range |
| Wrong IP shown in events | Networking / ECS fields | Confirmed attack-facing and log-forwarding interfaces |
| `/var/log/auth.log` missing | Linux logging | Used `journalctl` as fallback |
| Filebeat confusion | Ingestion | Standardized documentation to Elastic Agent / Fleet only |
| Prebuilt rule index warning | Detections | Created custom rule using available `system.auth` data |
| No alerts appearing | Detections | Built custom threshold rule using `system.auth.ssh.event` |
| Image links not rendering | GitHub documentation | Fixed file paths, spaces, and `.png.png` naming issues |

---

## Issue 1 — Logs Not Showing in Kibana

### Symptoms

- Authentication activity occurred on the target server.
- Logs did not immediately appear in Kibana Discover.
- Dashboard panels showed limited or missing data.

### Checks Performed

Verify Elastic Agent status on the target:

```bash
sudo elastic-agent status
```

Verify service status:

```bash
sudo systemctl status elastic-agent
```

Check Kibana Discover time range:

```text
Last 15 minutes
Last 24 hours
Last 30 days
```

### Resolution

The issue was addressed by validating Elastic Agent health, confirming logs were being indexed, and expanding the Kibana time range when needed.

---

## Issue 2 — Wrong or Unexpected IP Address in Kibana

### Symptoms

Kibana showed multiple IP addresses related to the target system.

Observed lab IPs included:

```text
192.168.70.128
192.168.56.30
192.168.70.130
```

### Cause

The Ubuntu target is dual-homed:

| Interface | Purpose | IP Address |
|---|---|---|
| Attack-facing interface | Receives Kali traffic | `192.168.70.128` |
| SIEM/log-forwarding interface | Sends logs to SIEM | `192.168.56.30` |

Kali uses:

```text
192.168.70.130
```

### Resolution

Documentation was updated to clearly separate:

- attacker IP
- target attack-facing IP
- target log-forwarding IP
- SIEM server IP

---

## Issue 3 — `/var/log/auth.log` Not Found

### Symptoms

The expected authentication log file was not present on the Ubuntu target.

Command:

```bash
sudo tail -f /var/log/auth.log
```

may not return results if the file does not exist.

### Resolution

Used journald as a fallback:

```bash
sudo journalctl | grep ssh
```

### Lesson Learned

Linux authentication logging can vary depending on distribution, configuration, and logging services.

---

## Issue 4 — Filebeat vs Elastic Agent Confusion

### Symptoms

Some documentation mentioned Filebeat even though standalone Filebeat was not installed separately on the target.

### Actual Configuration

The target uses:

```text
Elastic Agent enrolled in Fleet
```

Standalone Filebeat was not installed separately.

### Resolution

Documentation was standardized to say:

```text
Elastic Agent / Fleet
```

instead of:

```text
Filebeat / Elastic Agent
```

---

## Issue 5 — Prebuilt Rule Index Warning

### Symptoms

A prebuilt Elastic Security rule generated a warning similar to:

```text
No index matching: logs-endpoint.events.process*
```

### Cause

The rule expected Elastic Defend endpoint process telemetry.

The lab was collecting Linux authentication logs from:

```text
logs-system.auth-default
```

not endpoint process events.

### Resolution

The prebuilt rule was not used for final validation. A custom threshold rule was created using fields available in the lab data.

Custom rule query:

```kql
system.auth.ssh.event : "Failed"
```

Threshold field:

```text
source.ip
```

---

## Issue 6 — No Alerts Appearing

### Symptoms

Events appeared in Kibana Discover, but no alerts appeared under Security Alerts.

### Checks Performed

- Confirmed events existed in Discover
- Confirmed KQL query returned results
- Checked rule time range
- Checked alert page filters
- Checked rule index/data view
- Reviewed rule preview errors

### Resolution

A custom threshold rule was created using the field confirmed in Discover:

```kql
system.auth.ssh.event : "Failed"
```

Rule configuration:

| Setting | Value |
|---|---|
| Rule type | `threshold` |
| Query | `system.auth.ssh.event : "Failed"` |
| Group by | `source.ip` |
| Threshold | `5` |
| Source IP observed | `192.168.70.130` |
| Alert count observed | `8` |

### Result

A high-severity alert was successfully generated in Elastic Security.

---

## Issue 7 — Rule Preview Error

### Symptoms

Kibana showed:

```text
Failed to preview rule
[object Object] (500)
```

### Resolution

The rule configuration was simplified and validated using Discover before final alert testing.

The working rule used:

```kql
system.auth.ssh.event : "Failed"
```

against:

```text
logs-system.auth-default
```

---

## Issue 8 — GitHub Images Not Rendering

### Symptoms

Markdown displayed image syntax instead of rendering screenshots, or images appeared broken.

Examples:

```md
![Elastic Agent Status](image/Elastic Agent Status.png)
```

### Causes

Common causes included:

- wrong folder path
- filename capitalization mismatch
- spaces in filenames
- duplicate extensions such as `.png.png`
- folder named `image` instead of `images`

### Resolution

Image paths were corrected using exact filenames and URL encoding when needed.

Example with spaces:

```md
![Elastic Agent Status](image/Elastic%20Agent%20Status.png)
```

Preferred naming convention going forward:

```text
lowercase-filename.png
```

---

## Issue 9 — SSH Port Validation

### Symptoms

Before running Hydra, SSH reachability needed to be verified.

### Validation

From Kali Linux:

```bash
nc -vz 192.168.70.128 22
```

or:

```bash
nmap -p 22 192.168.70.128
```

### Resolution

SSH port validation was added to the attack simulation documentation.

---

## Final Working Validation Chain

```text
Kali Hydra activity
        ↓
Ubuntu target records failed SSH logs
        ↓
Elastic Agent forwards system.auth events
        ↓
Kibana Discover shows system.auth.ssh.event = Failed
        ↓
Custom threshold rule groups by source.ip
        ↓
High-severity alert generated
        ↓
Dashboard confirms attacker activity over time
```

---

## Key Commands

### Check Elastic Agent

```bash
sudo elastic-agent status
sudo systemctl status elastic-agent
```

### Check SSH logs

```bash
sudo journalctl | grep ssh
```

### Validate SSH port from Kali

```bash
nc -vz 192.168.70.128 22
```

### Run controlled Hydra test

```bash
hydra -l analyst -P passwords.txt ssh://192.168.70.128 -t 2
```

---

## Lessons Learned

- Always validate ingestion before building dashboards or detections.
- Discover is useful for confirming which fields actually exist.
- Custom rules should be based on fields confirmed in the lab data.
- Prebuilt rules may require integrations not enabled in a small lab.
- Image filenames should be simple, lowercase, and consistent.
- Dual-homed systems must be clearly documented to avoid IP confusion.

---

## CISSP Domain Alignment

| CISSP Domain | Troubleshooting Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | Validating secure architecture and data flow |
| Domain 4: Communication and Network Security | Confirming network reachability and segmentation |
| Domain 5: Identity and Access Management | Troubleshooting authentication visibility |
| Domain 6: Security Assessment and Testing | Validating attack simulation and controls |
| Domain 7: Security Operations | SIEM troubleshooting, alert validation, and monitoring |
