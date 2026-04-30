# 01 - Setup: Installation (VMware Included)

## Overview
This lab runs inside VMware Workstation / VMware Player.

## Required Virtual Machines

### 1. SIEM Server (Ubuntu Server / Debian)
- Elasticsearch
- Kibana
- IP: 192.168.56.10

### 2. Target Machine (Ubuntu)
- Filebeat / Elastic Agent
- IP: 192.168.70.128

### 3. Attacker Machine (Kali Linux)
- Tools: nmap, hydra, metasploit
- Used for attack simulation

---

## VMware Installation Steps

### Step 1: Install VMware
- Download VMware Workstation Player
- Install with default settings

### Step 2: Create Virtual Networks

#### Network 1 (SIEM Network)
- Type: Host-Only
- Subnet: 192.168.56.0/24
- SIEM Server connects here

#### Network 2 (Lab Network)
- Type: Host-Only or NAT
- Subnet: 192.168.70.0/24
- Target + Kali connect here

---

## VM Configuration

### SIEM Server VM
- 4 CPU / 8GB RAM recommended
- 100GB disk
- 2 NICs (optional advanced setup)

### Target VM
- 2 CPU / 4GB RAM
- 1 NIC (192.168.70.128)

### Kali VM
- 2–4 CPU / 4–8GB RAM
- 1 NIC (same network as target)

---

## Validation
- Ping between Kali → Target
- Kibana reachable at:
  http://192.168.56.10:5601
