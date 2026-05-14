# VMware Configuration – SIEM Lab

## Objective

Create a segmented virtual network that simulates a real-world SOC environment while maintaining proper connectivity for log ingestion, monitoring, and attack simulation.

---

## Virtual Network Design

| Network | Type | Subnet | Purpose |
|---|---|---|---|
| VMnet1 | Host-Only | `192.168.56.0/24` | SIEM infrastructure and log forwarding |
| VMnet2 | Host-Only or NAT | `192.168.70.0/24` | Attack simulation and target access |

---

## Virtual Machine Configuration

### SIEM Server — Elastic Stack

| Adapter | Network | IP Address | Purpose |
|---|---|---|---|
| Adapter 1 | VMnet1 | `192.168.56.10` | Elasticsearch and Kibana access |

The SIEM server hosts:

- Elasticsearch
- Kibana
- Fleet Server, if used

![SIEM VM Elastic](snapshots/02-siem-vm-elastic.jpg)

---

### Target Server — Log Source / Victim

| Adapter | Network | IP Address | Purpose |
|---|---|---|---|
| Adapter 1 | VMnet2 | `192.168.70.128` | Receives simulated attacks |
| Adapter 2 | VMnet1 | `192.168.56.30` | Sends logs to SIEM |

The target server is dual-homed so it can receive attacks on the attack network while forwarding telemetry to the SIEM network.

![Target VM Configuration](snapshots/03-target-vm-config.png)

---

### Kali Linux — Attacker

| Adapter | Network | IP Address | Purpose |
|---|---|---|---|
| Adapter 1 | VMnet2 | `192.168.70.20` | Runs attack simulations |

Kali is isolated to the attack network and is used to generate controlled security events.

![Kali VM](snapshots/01-kali-vm.jpg)

---

## IP Configuration Evidence

### SIEM IP Configuration

The SIEM server is configured on the SIEM network using `192.168.56.10`.

![SIEM IP Configuration](snapshots/04-siem-ip-config.jpg)

---

### Target IP Configuration

The target server contains network connectivity for both attack traffic and SIEM log forwarding.

![Target IP Configuration](snapshots/05-target-ip-config.png)

---

### Kali IP Configuration

The Kali attacker machine is configured on the attack network.

![Kali IP Configuration](snapshots/06-kali-ip-config.jpg)

---

## Traffic Flow

```text
Kali Linux Attacker
192.168.70.20
        |
        | SSH / Hydra / Nmap traffic
        v
Target Server
192.168.70.128
        |
        | Elastic Agent log forwarding
        v
SIEM Server
192.168.56.10
        |
        v
Kibana Dashboards and Alerts
```

---

## Connectivity Tests

### Kali to Target

Run from Kali:

```bash
ping 192.168.70.128
```

Expected result:

```text
Target responds successfully.
```

![Kali to Target Connectivity](snapshots/07-kali-target-connectivity.png)

---

### Target to SIEM Server

Run from the target server:

```bash
ping 192.168.56.10
```

Expected result:

```text
SIEM server responds successfully.
```

![Target to SIEM Connectivity](snapshots/08-target-to-siem-connectivity.png)

---

### Access Kibana

From the host machine browser:

```text
https://192.168.56.10:5601
```

Expected result:

```text
Kibana login page loads.
```

![Kibana Login Page](snapshots/09-kibana-login-page.png)

---

### Verify Fleet Agents

In Kibana:

```text
Management → Fleet → Agents
```

Expected result:

```text
Elastic Agent appears connected and healthy.
```

![Fleet Agents Connected](snapshots/10-fleet-agents-connected.png)

---

### Verify Elasticsearch Health

From the SIEM server:

```bash
curl -k -u elastic https://192.168.56.10:9200
```

Expected result:

```text
Elasticsearch responds with cluster information or health status.
```

![Elasticsearch Health](snapshots/11-elasticsearch-health.png)

---

## Resource Considerations

| System | Recommended RAM | Notes |
|---|---:|---|
| SIEM Server | 8 GB or more | Elasticsearch and Kibana are memory-intensive |
| Target Server | 2–4 GB | Generates Linux logs and runs Elastic Agent |
| Kali Linux | 2–4 GB | Used for scanning and attack simulation |

Additional notes:

- Elasticsearch performs best with enough memory available.
- Insufficient memory can cause slow indexing or failed services.
- Disk space should allow for log growth during attack simulations.
- Snapshots are recommended before major Elastic or network changes.

---

## Screenshot Evidence Summary

| Evidence | File |
|---|---|
| Kali VM | `snapshots/01-kali-vm.jpg` |
| SIEM VM Elastic | `snapshots/02-siem-vm-elastic.jpg` |
| Target VM Config | `snapshots/03-target-vm-config.png` |
| SIEM IP Config | `snapshots/04-siem-ip-config.jpg` |
| Target IP Config | `snapshots/05-target-ip-config.png` |
| Kali IP Config | `snapshots/06-kali-ip-config.jpg` |
| Kali to Target Connectivity | `snapshots/07-kali-target-connectivity.png` |
| Target to SIEM Connectivity | `snapshots/08-target-to-siem-connectivity.png` |
| Kibana Login Page | `snapshots/09-kibana-login-page.png` |
| Fleet Agents Connected | `snapshots/10-fleet-agents-connected.png` |
| Elasticsearch Health | `snapshots/11-elasticsearch-health.png` |

---

## CISSP Domain Alignment

| CISSP Domain | Relevance |
|---|---|
| Domain 3: Security Architecture and Engineering | Network segmentation and system design |
| Domain 4: Communication and Network Security | Segmented network communication |
| Domain 6: Security Assessment and Testing | Connectivity and control validation |
| Domain 7: Security Operations | SIEM monitoring and log collection |

---

## Notes

- Attack traffic should target `192.168.70.128`.
- Kibana should be accessed through `192.168.56.10`.
- Elastic Agent should send logs from the target server to the SIEM server.
- VMnet1 and VMnet2 should not be mixed unless intentionally routing traffic.
- The SIEM server should not be used as the attack target.
