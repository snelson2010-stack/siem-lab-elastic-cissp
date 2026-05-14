# Failed SSH Login Detection

## Purpose

This detection identifies failed SSH authentication attempts on the Ubuntu target server.

Failed login monitoring helps identify:

- invalid username attempts
- password guessing
- brute force behavior
- unauthorized access attempts

---

## Detection Logic

### KQL Query

```kql
message : "Failed password"
```

Recommended scoped query:

```kql
data_stream.dataset : "system.auth" and message : "Failed password"
```

---

## Expected Events

Typical SSH authentication failures include:

```text
Failed password for invalid user
Failed password for root
Failed password for admin
```

These events are generated when attackers attempt invalid SSH logins against the target system.

---

## Attack Simulation Source

The failed login events were generated from Kali Linux using:

- SSH login attempts
- Hydra brute force testing

Target system:

```text
192.168.70.128
```

Attacker system:

```text
192.168.70.130
```

---

## Screenshot Evidence

### Failed SSH Login Events

This screenshot shows failed SSH login events successfully ingested into Kibana.

![Failed SSH Logins](screenshots/failed-ssh-logins.png.png)

---

## Investigation Workflow

```text
Failed login detected
        ↓
Review source IP
        ↓
Review username targeted
        ↓
Identify repeated attempts
        ↓
Determine possible brute force activity
```

---

## MITRE ATT&CK Mapping

| Technique | Description |
|---|---|
| T1110 | Brute Force |

---

## CISSP Domain Alignment

| CISSP Domain | Relevance |
|---|---|
| Domain 5: Identity and Access Management | Authentication monitoring |
| Domain 6: Security Assessment and Testing | Attack validation |
| Domain 7: Security Operations | Detection and investigation |

---

## Notes

- Elastic Agent collects Linux authentication logs from the target server.
- Authentication events are searchable in Kibana Discover.
- Failed login activity was intentionally generated inside the isolated VMware lab.
