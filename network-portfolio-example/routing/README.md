# 🧷 Topologi Inter-VLAN Routing (Router-on-a-Stick)

**Deskripsi Topologi:**
- 1 Router
- 1 Switch
- 2 PC (PC1 di VLAN 10, PC2 di VLAN 20)
- Menggunakan metode Router-on-a-Stick untuk menghubungkan antar VLAN

---

## 💻 Konfigurasi IP

| Perangkat     | IP Address      | VLAN | Subnet Mask       | Keterangan           |
|---------------|------------------|------|--------------------|----------------------|
| PC1           | 192.168.10.10    | 10   | 255.255.255.0      | VLAN 10 (HR)         |
| PC2           | 192.168.20.10    | 20   | 255.255.255.0      | VLAN 20 (IT)         |
| Router G0/0.10| 192.168.10.1     | 10   | 255.255.255.0      | Sub-interface VLAN 10|
| Router G0/0.20| 192.168.20.1     | 20   | 255.255.255.0      | Sub-interface VLAN 20|

> Gateway di masing-masing PC diisi sesuai IP router subinterface-nya

---

## ⚙️ **Konfigurasi Switch**

enable
configure terminal
vlan 10
name HR
vlan 20
name IT

interface fa0/2
switchport mode access
switchport access vlan 10

interface fa0/3
switchport mode access
switchport access vlan 20

interface fa0/1
switchport mode trunk

⚙️ **Konfigurasi Router**

interface g0/0
no shutdown

interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0


🧪 **Langkah Pengujian**

1. Hubungkan PC1 ke fa0/1, PC2 ke fa0/2, dan router ke fa0/24
2. Masukkan IP dan gateway di masing-masing PC
3. Ping dari PC1 ke PC2:

C:\>ping 192.168.20.10

Pinging 192.168.20.10 with 32 bytes of data:
Reply from 192.168.20.10: bytes=32 time<1ms TTL=127
Reply from 192.168.20.10: bytes=32 time<1ms TTL=127
Reply from 192.168.20.10: bytes=32 time<1ms TTL=127
Reply from 192.168.20.10: bytes=32 time<1ms TTL=127


📌 Catatan
Port trunk penting untuk mengangkut banyak VLAN ke router
Sub-interface harus sesuai dengan ID VLAN
