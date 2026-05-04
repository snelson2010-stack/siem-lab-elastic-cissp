# 🖥️ VMware Configuration – SIEM Lab

## 🎯 Objective
Create a segmented virtual network that simulates a real-world SOC environment while maintaining proper connectivity for log ingestion.

---

## 🧱 Virtual Network Design

### VMnet1 – SIEM Network
- Type: Host-Only
- Subnet: 192.168.56.0/24
- Purpose: SIEM infrastructure communication

### VMnet2 – Attack Network
- Type: Host-Only or NAT
- Subnet: 192.168.70.0/24
- Purpose: Attack simulation and log generation

---

## 🖥️ Virtual Machine Configuration

### 🔐 SIEM Server (Elastic Stack)
- Network Adapter 1:
  - VMnet1 (192.168.56.10)

---

### 🎯 Target Server (Log Source)
- Network Adapter 1:
  - VMnet2 (192.168.70.128)  ← receives attacks
- Network Adapter 2:
  - VMnet1 (192.168.56.30)   ← sends logs to SIEM

---

### ⚔️ Kali Linux (Attacker)
- Network Adapter 1:
  - VMnet2 (192.168.70.20)

---

## 🔄 Traffic Flow

Kali (Attack Network)
        ↓
Target Server (Dual-Homed)
        ↓
SIEM Server (SIEM Network)

---

## 🔍 Connectivity Tests

### Kali → Target
```bash
ping 192.168.70.128
