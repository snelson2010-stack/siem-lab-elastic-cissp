# 01 - Setup: Architecture (VMware Lab)

## VMware-Based SIEM Lab

```
        [Kali Linux VM]
              |
              | attacks (SSH, Nmap, Hydra)
              v
     [Ubuntu Target VM - 192.168.70.128]
              |
              | logs (auth.log, syslog)
              v
        [Filebeat Agent]
              |
              v
   [SIEM Server VM - 192.168.56.10]
   (Elasticsearch + Kibana)
```

---

## VMware Network Design

### Host-Only Network 1
- 192.168.56.0/24 → SIEM Server

### Host-Only Network 2
- 192.168.70.0/24 → Target + Kali

---

## Why VMware?

- Full isolation of attack environment
- Safe penetration testing practice
- Realistic SOC simulation
- Repeatable lab environment
