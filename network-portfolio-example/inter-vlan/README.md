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

## ⚙️ Konfigurasi Switch

```bash
Switch>enable
Switch#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#vlan 10
Switch(config-vlan)#name HR
Switch(config-vlan)#vlan 20
Switch(config-vlan)#name IT
Switch(config-vlan)#exit

Switch(config)#interface fa0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-vlan)#exit

Switch(config)#interface fa0/3
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-vlan)#exit

Switch(config)#interface fa0/1
Switch(config-if)#switchport mode trunk
Switch(config-vlan)#exit

---

⚙️ Konfigurasi Router

Router>enable
Router#configure terminal
Router(config)#interface g0/0
Router(config-if)#no shutdown
Router(config)#exit

Router(config)#interface g0/0.10
Router(config)-subif#encapsulation dot1Q 10
Router(config-subif)#ip address 192.168.10.1 255.255.255.0
Router(config)#exit

Router(config)#interface g0/0.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip address 192.168.20.1 255.255.255.0


🧪 **Langkah Pengujian**
Hubungkan PC1 ke fa0/1, PC2 ke fa0/2, dan router ke fa0/24

Masukkan IP dan gateway di masing-masing PC

Ping dari PC1 ke PC2:

ping 192.168.20.10
Harus sukses

📌 Catatan
Port trunk penting untuk mengangkut banyak VLAN ke router

Sub-interface harus sesuai dengan ID VLAN
