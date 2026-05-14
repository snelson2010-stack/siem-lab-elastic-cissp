# SIEM Lab Installation Guide — Elastic Stack

## Overview

This guide documents the installation process for the Elastic SIEM lab using VMware, Ubuntu, Kali Linux, Elasticsearch, Kibana, Fleet, and Elastic Agent.

The lab contains three main systems:

| System | Role | IP Address |
|---|---|---|
| SIEM Server | Elasticsearch / Kibana | `192.168.56.10` |
| Target Server | Victim / Log Source | `192.168.70.128` |
| Target Server | Log Forwarding Interface | `192.168.56.30` |
| Kali Linux | Attacker | `192.168.70.130` |

---

## Network Overview

| Network | Subnet | Purpose |
|---|---|---|
| VMnet1 | `192.168.56.0/24` | SIEM/log forwarding network |
| VMnet2 | `192.168.70.0/24` | Attack simulation network |

---

## Step 1 — Prepare VMware Network

Create two VMware networks.

### VMnet1 — SIEM Network

```text
Subnet: 192.168.56.0/24
Purpose: SIEM server and log forwarding
```

### VMnet2 — Attack Network

```text
Subnet: 192.168.70.0/24
Purpose: Kali attacking target server
```

---

## Step 2 — Configure Virtual Machines

### SIEM Server

```text
Adapter 1: VMnet1
IP Address: 192.168.56.10
```

### Target Server

```text
Adapter 1: VMnet2
IP Address: 192.168.70.128

Adapter 2: VMnet1
IP Address: 192.168.56.30
```

### Kali Linux

```text
Adapter 1: VMnet2
IP Address: 192.168.70.130
```

---

## Step 3 — Install Elasticsearch on SIEM Server

Download and install Elasticsearch:

```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.19.0-amd64.deb
sudo dpkg -i elasticsearch-8.19.0-amd64.deb
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

Verify Elasticsearch service status:

```bash
sudo systemctl status elasticsearch
```

Test local Elasticsearch access:

```bash
curl -k -u elastic https://localhost:9200
```

---

## Step 4 — Install Kibana on SIEM Server

Download and install Kibana:

```bash
wget https://artifacts.elastic.co/downloads/kibana/kibana-8.19.0-amd64.deb
sudo dpkg -i kibana-8.19.0-amd64.deb
sudo systemctl enable kibana
sudo systemctl start kibana
```

Verify Kibana service status:

```bash
sudo systemctl status kibana
```

Access Kibana from the host machine:

```text
https://192.168.56.10:5601
```

---

## Step 5 — Install Elastic Agent on Target Server

On the target server, download Elastic Agent:

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.19.0-linux-x86_64.tar.gz
tar xzvf elastic-agent-8.19.0-linux-x86_64.tar.gz
cd elastic-agent-8.19.0-linux-x86_64
```

Enroll the agent into Fleet:

```bash
sudo ./elastic-agent install \
  --url=https://192.168.56.10:8220 \
  --enrollment-token=<YOUR_ENROLLMENT_TOKEN>
```

Verify agent status:

```bash
sudo elastic-agent status
```

---

## Step 6 — Install Attack Tools on Kali

On Kali Linux:

```bash
sudo apt update
sudo apt install nmap hydra netcat-traditional -y
```

Verify Kali can reach the target:

```bash
ping 192.168.70.128
```

---

## Step 7 — Generate Lab Test Events

### SSH failed login test

From Kali:

```bash
ssh fakeuser@192.168.70.128
```

### Nmap scan test

From Kali:

```bash
nmap 192.168.70.128
```

### Controlled Hydra test

Use only in the isolated lab environment:

```bash
hydra -l analyst -P passwords.txt ssh://192.168.70.128 -t 2
```

---

## Step 8 — Verify SIEM Data

Check Elasticsearch indices:

```bash
curl -k -u elastic https://192.168.56.10:9200/_cat/indices?v
```

In Kibana Discover, use:

```kql
message : "Failed password"
```

or:

```kql
data_stream.dataset : "system.auth"
```

---

## Validation Checklist

| Check | Expected Result |
|---|---|
| SIEM server reachable | `192.168.56.10` responds |
| Target reachable from Kali | `192.168.70.128` responds |
| Target reaches SIEM | `192.168.56.10` responds |
| Kibana accessible | Login page loads |
| Elastic Agent healthy | Agent appears healthy in Fleet |
| Logs searchable | Authentication events appear in Discover |

---

## Notes

- Attack traffic should target `192.168.70.128`.
- Kali IP is `192.168.70.130`.
- Target log-forwarding IP is `192.168.56.30`.
- Kibana and Elasticsearch are accessed through `192.168.56.10`.
- Do not attack the SIEM server.
- Replace version numbers if a different Elastic version is used in the lab.
