# 🔐 Free SIEM Lab – Elastic Stack (CISSP Aligned)

## 🎯 Overview
This project demonstrates a full Security Information and Event Management (SIEM) lab using the Elastic Stack.  
It simulates real-world SOC operations including log ingestion, threat detection, attack simulation, and dashboard creation.

The lab is mapped to CISSP domains:
- Security Operations
- Security Assessment & Testing
- Identity & Access Management
- Communication & Network Security

---

## 🧱 Architecture

- Kali Linux (Attacker VM)
- Ubuntu Server (Target / Log Source)
- Elastic Stack (SIEM Server: Elasticsearch + Kibana)
- Filebeat / Elastic Agent for log shipping

---

## ⚔️ Attack Scenarios Simulated

- SSH brute force (Hydra)
- Network scanning (Nmap)
- Failed login attempts
- Privilege escalation attempts

---

## 📊 Detection Capabilities

- Failed login detection
- Port scan detection
- Authentication anomaly tracking
- System log monitoring

---

## 📁 Repository Structure
See folders for full documentation of setup, ingestion, detections, and dashboards.

---

## 🧠 Skills Demonstrated

- SIEM deployment (Elastic Stack)
- Log ingestion and parsing
- Security monitoring
- Threat detection engineering
- SOC dashboard design
- Incident analysis

---

## 📌 Purpose
Built as a cybersecurity home lab for:
- CISSP knowledge reinforcement
- SOC analyst skill development
- Threat detection engineering practice
