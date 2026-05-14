# SSH Brute Force Attack Simulation

## Purpose

This document describes the controlled SSH brute force simulation performed in the Elastic SIEM lab.

The goal of this attack simulation is to generate realistic Linux authentication events that can be ingested by Elastic Agent, searched in Kibana, and used to validate detection rules.

---

## Lab Systems

| System | Role | IP Address |
|---|---|---|
| Kali Linux | Attacker | `192.168.70.130` |
| Ubuntu Target | Victim / SSH Server | `192.168.70.128` |
| Ubuntu Target | Log Forwarding Interface | `192.168.56.30` |
| SIEM Server | Elasticsearch / Kibana | `192.168.56.10` |

---

## Scope

This simulation was performed only inside an isolated VMware lab environment owned and controlled by the lab operator.

The target of the simulation was the Ubuntu target server at:

```text
192.168.70.128
```

The SIEM server was not attacked.

---

## Attack Tool

Hydra was used from Kali Linux to generate repeated SSH authentication attempts.

Hydra is commonly used in labs to simulate password guessing behavior against authentication services.

---

## Preconditions

Before running the simulation:

- SSH service was running on the Ubuntu target
- Kali could reach the target on VMnet2
- Elastic Agent was healthy on the target server
- Authentication logs were visible in Kibana
- A small lab password list was created on Kali

---

## Connectivity Validation

From Kali Linux:

```bash
ping 192.168.70.128
```

Optional SSH test:

```bash
ssh fakeuser@192.168.70.128
```

---

## Hydra Command

From Kali Linux:

```bash
hydra -l analyst -P passwords.txt ssh://192.168.70.128 -t 2
```

Command breakdown:

| Option | Meaning |
|---|---|
| `-l analyst` | Username tested during the simulation |
| `-P passwords.txt` | Password list used for controlled testing |
| `ssh://192.168.70.128` | SSH target on the Ubuntu victim server |
| `-t 2` | Limits parallel tasks to reduce unnecessary noise |

---

## Expected Result

The attack simulation should generate repeated failed SSH authentication events from Kali Linux.

Expected source IP:

```text
192.168.70.130
```

Expected target IP:

```text
192.168.70.128
```

Expected SSH event type:

```text
Failed
```

---

## Log Validation on Target

On the Ubuntu target, authentication events can be reviewed with journald or local authentication logs.

```bash
sudo journalctl | grep ssh
```

If available:

```bash
sudo tail -f /var/log/auth.log
```

Expected log behavior:

```text
Failed password for analyst from 192.168.70.130
```

---

## Kibana Validation

In Kibana Discover, use:

```kql
system.auth.ssh.event : "Failed"
```

Additional useful queries:

```kql
source.ip : "192.168.70.130"
```

```kql
data_stream.dataset : "system.auth"
```

```kql
message : "Failed password"
```

---

## Detection Validation

The SSH brute force simulation successfully triggered the custom threshold rule documented in:

```text
03-detections/ssh-bruteforce-detection.md
```

Rule behavior:

| Field | Value |
|---|---|
| Rule type | `threshold` |
| Query | `system.auth.ssh.event : "Failed"` |
| Group by | `source.ip` |
| Threshold | `5` |
| Alert count observed | `8` |
| Source IP | `192.168.70.130` |
| Severity | `high` |

---

## Screenshot Evidence

Add screenshots for evidence after upload.

Recommended screenshots:

```text
04-attack-simulations/image/hydra-bruteforce.png
02-ingestion/image/linux-authlog-terminal.png
02-ingestion/image/failed-ssh-logins.png.png
03-detections/image/custom-ssh-bruteforce-alert.png.png
```

### Hydra Output

![Hydra Brute Force Output](image/hydra-bruteforce.png)

### Failed SSH Events in Kibana

![Failed SSH Logins](../02-ingestion/image/failed-ssh-logins.png.png)

### Detection Alert

![Custom SSH Brute Force Alert](../03-detections/image/custom-ssh-bruteforce-alert.png.png)

---

## Attack Flow

```text
Kali Linux
192.168.70.130
        ↓
Hydra SSH authentication attempts
        ↓
Ubuntu Target SSH Service
192.168.70.128
        ↓
Failed authentication logs generated
        ↓
Elastic Agent forwards logs
        ↓
Elasticsearch indexes events
        ↓
Kibana detection rule creates alert
```

---

## MITRE ATT&CK Mapping

| Technique | Description | Lab Evidence |
|---|---|---|
| T1110 | Brute Force | Repeated failed SSH login attempts |

---

## CISSP Domain Alignment

| CISSP Domain | Relevance |
|---|---|
| Domain 5: Identity and Access Management | Authentication control testing |
| Domain 6: Security Assessment and Testing | Controlled attack simulation and validation |
| Domain 7: Security Operations | SIEM monitoring, alerting, and investigation |

---

## Lessons Learned

- Controlled attack simulations are useful for validating SIEM visibility.
- Hydra activity generated clear failed SSH authentication events.
- Elastic Agent successfully forwarded authentication telemetry.
- Kibana Discover confirmed event visibility.
- A custom threshold rule successfully generated a high-severity alert.
- Detection logic should be based on fields confirmed in Discover.

---

## Safety Note

This simulation was conducted only against systems owned and controlled inside the isolated VMware lab. Do not run brute force tools against systems without explicit authorization.
