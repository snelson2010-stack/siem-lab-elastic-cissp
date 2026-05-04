# 🧱 Free SIEM Lab — Elastic Stack (SOC Simulation Project)

## 🎯 Overview
This project builds a simulated Security Operations Center (SOC) using the Elastic Stack. It demonstrates end-to-end security monitoring, log ingestion, attack simulation, and detection engineering.

The environment is designed for cybersecurity learning and aligns with CISSP domains including Security Operations, Network Security, and Security Architecture.

---

# 🖥️ VMware Network Design

## 🌐 Host-Only Network 1 (SIEM Network)
- Subnet: 192.168.56.0/24
- Purpose:
  - SIEM infrastructure
  - Elasticsearch + Kibana
  - Centralized security monitoring

### Hosts
- SIEM Server → 192.168.56.10

---

## 🌐 Host-Only Network 2 (Attack & Target Network)
- Subnet: 192.168.70.0/24
- Purpose:
  - Attack simulation environment
  - Victim system logging
  - Security event generation

### Hosts
- Kali Linux (Attacker) → 192.168.70.50
- Ubuntu Target Server → 192.168.70.128

---

## 🧠 Why VMware?

- 🔒 Full isolation of attack environment from production systems  
- 🧪 Safe penetration testing practice  
- 🏢 Realistic SOC simulation environment  
- 🔁 Repeatable and resettable lab setup  

---

# 🖥️ IP Assignment Plan

| VM Name        | Role                | Network | IP Address       |
|----------------|---------------------|---------|------------------|
| SIEM Server    | Elasticsearch + Kibana | VMnet1  | 192.168.56.10    |
| Target Server  | Log Source (Ubuntu) | VMnet2  | 192.168.70.128   |
| Kali Linux     | Attacker Machine    | VMnet2  | 192.168.70.50    |

---

# 🧱 SIEM Architecture

```text id="arch_final_01"
                🖥️ Kali Linux VM (Attacker)
                        |
                        |  Attacks (SSH, Nmap, Hydra)
                        v
        🛡️ Ubuntu Target VM — 192.168.70.128
                        |
                        |  Logs (auth.log, syslog, auditd)
                        v
                📦 Filebeat / Elastic Agent
                        |
                        |  Log Forwarding
                        v
        🧠 SIEM Server VM — 192.168.56.10
           (Elasticsearch + Kibana + SIEM Rules)


🔁 Data Flow
Attacker (192.168.70.50)
        ↓
Target Server (192.168.70.128)
        ↓ logs generated (system + security logs)
Filebeat / Elastic Agent
        ↓
SIEM Server (192.168.56.10)
        ↓
Elasticsearch → Kibana Dashboards → Alerts




