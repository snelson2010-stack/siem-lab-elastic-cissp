# 01 - Setup: VMware Configuration

## Step-by-Step VMware Setup

### 1. Install VMware Workstation Player
- Download from VMware official site
- Install with admin privileges

---

### 2. Create Virtual Networks

#### Network Adapter Settings
Go to:
- Edit → Virtual Network Editor

Create:

### VMnet1 (SIEM Network)
- Host-only
- Subnet: 192.168.56.0

### VMnet2 (Attack Network)
- Host-only or NAT
- Subnet: 192.168.70.0

---

### 3. Assign VMs

#### SIEM Server
- VMnet1 (192.168.56.10)

#### Target Machine
- VMnet2 (192.168.70.128)

#### Kali Linux
- VMnet2 (same as target)

---

### 4. Network Test

```bash
ping 192.168.70.128
ping 192.168.56.10
```

---

## Common VMware Issues

### ❌ No internet in VM
- Check NAT vs Host-only mode

### ❌ No communication between VMs
- Ensure same VMnet assignment

### ❌ IP mismatch
- Verify static IP config in `/etc/netplan/`
