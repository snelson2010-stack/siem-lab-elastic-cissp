# IP Assignment Plan — SIEM Lab

## Overview

This document defines the static IP assignments used in the Elastic SIEM lab.

The lab uses two VMware networks:

| Network | Subnet | Purpose |
|---|---|---|
| VMnet1 | `192.168.56.0/24` | SIEM infrastructure and log forwarding |
| VMnet2 | `192.168.70.0/24` | Attack simulation network |

---

## Virtual Machine IP Assignments

| VM Name | Function | Network | IP Address | Notes |
|---|---|---|---|---|
| SIEM Server | Elastic Stack | VMnet1 | `192.168.56.10` | Kibana and Elasticsearch |
| Target Server | Log Source / Victim | VMnet1 | `192.168.56.30` | Sends logs to SIEM |
| Target Server | Log Source / Victim | VMnet2 | `192.168.70.128` | Receives simulated attacks |
| Kali Linux | Attacker | VMnet2 | `192.168.70.130` | Runs attack simulations |

---

## Which IP to Use

### Attack Target

Use the target server's VMnet2 address when running attacks from Kali:

```text
192.168.70.128
```

Examples:

```bash
ssh fakeuser@192.168.70.128
```

```bash
nmap 192.168.70.128
```

```bash
hydra -l analyst -P passwords.txt ssh://192.168.70.128 -t 2
```

---

### SIEM Access

Use the SIEM server's VMnet1 address for Kibana and Elasticsearch:

```text
192.168.56.10
```

Examples:

```text
https://192.168.56.10:5601
```

```text
https://192.168.56.10:9200
```

---

### Log Forwarding Path

The target server forwards logs to the SIEM server over VMnet1:

```text
Target Server 192.168.56.30 → SIEM Server 192.168.56.10
```

---

## Network Segmentation

- VMnet1 is the security monitoring zone.
- VMnet2 is the attack and target simulation zone.
- Kali should attack the target on `192.168.70.128`.
- Kibana should be accessed on `192.168.56.10`.
- Elastic Agent should run on the target server and forward logs to the SIEM server.

---

## Validation Commands

### Kali to Target

Run from Kali:

```bash
ping 192.168.70.128
```

### Target to SIEM

Run from the target server:

```bash
ping 192.168.56.10
```

### Confirm Kali IP

Run from Kali:

```bash
ip a
```

Expected Kali attack-network IP:

```text
192.168.70.130
```

---

## Notes

- Do not attack the SIEM server.
- Do not mix VMnet1 and VMnet2 IP addresses.
- Attack traffic should stay on VMnet2.
- SIEM log forwarding should use VMnet1.
