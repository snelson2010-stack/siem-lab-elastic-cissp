
---

# ⚠️ Design Principles

## Network Segmentation
- VMnet1 isolates SIEM infrastructure
- VMnet2 simulates real-world attack surface

## Security Monitoring
- All logs are centralized in Elasticsearch
- Kibana used for visualization and alerting

## Least Privilege Design
- Target server only generates logs
- SIEM server only receives and analyzes data
- Attacker is isolated in separate subnet

---

# 🧠 CISSP Domain Mapping

## Security Operations
- Centralized logging and monitoring
- Incident detection and response simulation

## Security Architecture & Engineering
- Network segmentation design
- Secure data flow between zones

## Communication & Network Security
- Isolated attack network
- Controlled traffic flow between VMnets

## Security Assessment & Testing
- Simulated brute force and network scanning attacks

---

# 📌 Notes
- Do NOT mix VMnet1 and VMnet2 IP ranges.
- Ensure Filebeat/Elastic Agent is installed ONLY on the target server.
- Kibana must always be accessed via 192.168.56.10.
