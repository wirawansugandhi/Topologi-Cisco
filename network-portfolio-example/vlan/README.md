# 🧷 Topologi VLAN - Switch Layer 2

**Deskripsi Topologi:**
- 1 Switch
- 2 PC (PC1 dan PC2)
- PC1 berada di VLAN 10, PC2 berada di VLAN 20
- Tujuan: Memisahkan broadcast domain antar pengguna

---

## 💻 Konfigurasi IP

| Perangkat | IP Address     | VLAN | Subnet Mask     |
|-----------|----------------|------|------------------|
| PC1       | 192.168.10.10  | 10   | 255.255.255.0    |
| PC2       | 192.168.20.10  | 20   | 255.255.255.0    |

> Gateway tidak perlu diisi karena belum ada router

---

# ⚙️ Konfigurasi VLAN pada Switch

```bash
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name HR
Switch(config)# vlan 20
Switch(config-vlan)# name IT

Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

Switch(config)# interface fa0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20

---
```bash

🧪 ## **Langkah Pengujian**
Hubungkan PC1 ke port fa0/1 dan PC2 ke fa0/2
Masukkan IP statis di masing-masing PC

Lakukan ping:
Dari PC1: ping 192.168.20.10
Ping dari PC1 ke PC2 akan gagal, karena berbeda VLAN tanpa router

Komunikasi baru akan terjadi jika kamu konfigurasi Inter-VLAN Routing
