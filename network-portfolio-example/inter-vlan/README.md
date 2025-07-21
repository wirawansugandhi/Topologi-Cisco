# 🧷 Topologi Inter-VLAN Routing (Router-on-a-Stick)

**Deskripsi Topologi:**
- 1 Router
- 1 Switch
- 2 PC (PC1 di VLAN 10, PC2 di VLAN 20)
- Menggunakan metode Router-on-a-Stick untuk menghubungkan antar VLAN

---

## 💻 Konfigurasi IP

| Perangkat      | IP Address       | VLAN | Subnet Mask      | Keterangan            |
|----------------|------------------|------|------------------|------------------------|
| PC1            | 192.168.10.10    | 10   | 255.255.255.0     | VLAN 10 (HR)           |
| PC2            | 192.168.20.10    | 20   | 255.255.255.0     | VLAN 20 (IT)           |
| Router G0/0.10 | 192.168.10.1     | 10   | 255.255.255.0     | Sub-interface VLAN 10  |
| Router G0/0.20 | 192.168.20.1     | 20   | 255.255.255.0     | Sub-interface VLAN 20  |

> Gateway di masing-masing PC diisi sesuai IP router subinterface-nya

---

## ⚙️ Konfigurasi Switch

```bash
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name HR
Switch(config)# vlan 20
Switch(config-vlan)# name IT

Switch(config)# interface fa0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

Switch(config)# interface fa0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20

Switch(config)# interface fa0/1
Switch(config-if)# switchport mode trunk
```

---

## ⚙️ Konfigurasi Router

```bash
Router> enable
Router# configure terminal
Router(config)# interface g0/0
Router(config-if)# no shutdown

Router(config)# interface g0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

Router(config)# interface g0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
```

---

## 🧪 Langkah Pengujian

1. Hubungkan PC1 ke fa0/2, PC2 ke fa0/3, dan router ke fa0/1  
2. Masukkan IP dan gateway di masing-masing PC  
3. Uji koneksi:

```bash
ping 192.168.20.10
```

✅ Ping harus berhasil karena ada routing antar VLAN

---

## 📌 Catatan
- Port trunk penting untuk mengangkut banyak VLAN ke router  
- Sub-interface router harus sesuai dengan ID VLAN
