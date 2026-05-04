# 🧱 SIEM Lab Installation Guide — Elastic Stack

## 🎯 Overview
This guide provides the installation steps for setting up a SIEM lab using the Elastic Stack with a target machine and attacker machine in a VMware environment.

---

# 🖥️ VMware Network Setup

## 🌐 Host-Only Network 1 — SIEM Network
- Subnet: 192.168.56.0/24
- Purpose: SIEM server (Elastic Stack)

## 🌐 Host-Only Network 2 — Attack Network
- Subnet: 192.168.70.0/24
- Purpose: Target + Attacker systems

---

# 🖥️ Virtual Machines

- 🧠 SIEM Server (Ubuntu) → 192.168.56.10  
- 🛡️ Target Server (Ubuntu) → 192.168.70.128  
- ⚔️ Kali Linux (Attacker) → 192.168.70.50  

---

# 🧠 Step 1 — Install SIEM Server (Elastic Stack)

## Install Elasticsearch
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.19.0-amd64.deb
sudo dpkg -i elasticsearch-8.19.0-amd64.deb
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

#Install Kibana 

wget https://artifacts.elastic.co/downloads/kibana/kibana-8.19.0-amd64.deb
sudo dpkg -i kibana-8.19.0-amd64.deb
sudo systemctl enable kibana
sudo systemctl start kibana

https://192.168.56.10:5601

# 🧠 Step 2 — Install Target Server (Ubuntu)

Install Elastic Agent
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.19.0-linux-x86_64.tar.gz
tar xzvf elastic-agent-8.19.0-linux-x86_64.tar.gz
cd elastic-agent-8.19.0-linux-x86_64

Enroll Agent to Fleet
sudo ./elastic-agent install \
  --url=https://192.168.56.10:8220 \
  --enrollment-token=<YOUR_TOKEN>


Verify Agent Status

On SIEM server (Kibana → Fleet):

Ensure agent is Active
Confirm logs are flowing

⚔️ Step 3 — Attacker Machine (Kali Linux)
sudo apt update
sudo apt install nmap hydra netcat metasploit-framework -y

Example Attacks
Network Scan
nmap -sS 192.168.70.128

SSH Brute Force
hydra -l root -P rockyou.txt ssh://192.168.70.128

Step 4 — Verify SIEM Data
Check Elasticsearch Indices
curl -k -u elastic https://192.168.56.10:9200/_cat/indices?v


Kibana Validation

Open:

https://192.168.56.10:5601

