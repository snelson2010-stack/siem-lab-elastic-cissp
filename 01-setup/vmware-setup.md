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

---

### Target Server — Log Source / Victim

| Adapter | Network | IP Address | Purpose |
|---|---|---|---|
| Adapter 1 | VMnet2 | `192.168.70.128` | Receives simulated attacks |
| Adapter 2 | VMnet1 | `192.168.56.30` | Sends logs to SIEM |

The target server is dual-homed so it can receive attacks on the attack network while forwarding telemetry to the SIEM network.

---

### Kali Linux — Attacker

| Adapter | Network | IP Address | Purpose |
|---|---|---|---|
| Adapter 1 | VMnet2 | `192.168.70.20` | Runs attack simulations |

Kali is isolated to the attack network and is used to generate controlled security events.

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

---

### Verify Elasticsearch

From the SIEM server:

```bash
curl -k -u elastic https://192.168.56.10:9200
```

Expected result:

```text
Elasticsearch responds with cluster information or an authentication response.
```

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

## Screenshot Evidence

Recommended screenshots for this section:

```text
images/
├── vmware-vmnet1-config.png
├── vmware-vmnet2-config.png
├── siem-vm-network-adapter.png
├── target-vm-network-adapters.png
├── kali-vm-network-adapter.png
├── target-to-siem-ping.png
└── kali-to-target-ping.png
```

Example markdown:

```md
![VMnet1 Configuration](../images/vmware-vmnet1-config.png)

![VMnet2 Configuration](../images/vmware-vmnet2-config.png)

![Kali to Target Connectivity](../images/kali-to-target-ping.png)

![Target to SIEM Connectivity](../images/target-to-siem-ping.png)
```

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
