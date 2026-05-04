# 📌 IP Assignment Plan — SIEM Lab

This document defines all static IP assignments used in the lab.

## 🖥️ Virtual Machines

| VM Name        | Function        | Network | IP Address       | Notes |
|----------------|----------------|---------|------------------|------|
| SIEM Server    | Elastic Stack   | VMnet1  | 192.168.56.10    | Kibana + Elasticsearch |
| Target Server  | Log Generator   | VMnet2  | 192.168.70.128   | Filebeat installed |
| Kali Linux     | Attacker       | VMnet2  | 192.168.70.50    | Attack simulation |

## 🔁 Network Segmentation

- VMnet1 → Security monitoring zone
- VMnet2 → Attack & target simulation zone
