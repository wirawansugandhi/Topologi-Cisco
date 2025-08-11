# DHCP Server in Cisco Network (Router-on-a-Stick)

## 📜 Deskripsi Proyek
Proyek ini mensimulasikan konfigurasi **DHCP Server** di router Cisco (Router-on-a-Stick) untuk memberikan alamat IP otomatis ke perangkat di tiga VLAN berbeda.  
Setiap VLAN merepresentasikan lantai berbeda pada gedung perusahaan.

## 🎯 Tujuan
- Mengonfigurasi subinterface pada router untuk setiap VLAN.
- Mengaktifkan **DHCP Server** di router untuk memberikan IP otomatis.
- Memastikan komunikasi antar VLAN berjalan dengan baik.


**Keterangan VLAN:**
- **VLAN 10 (Lantai 1):** `192.168.10.0/24` (Merah)
- **VLAN 20 (Lantai 2):** `192.168.20.0/24` (Biru)
- **VLAN 30 (Lantai 3):** `192.168.30.0/24` (Kuning)

## 📋 Tabel IP Address

| Perangkat   | Interface   | IP Address      | VLAN |
|-------------|-------------|-----------------|------|
| Router0     | Fa0/0.10    | 192.168.10.1    | 10   |
| Router0     | Fa0/0.20    | 192.168.20.1    | 20   |
| Router0     | Fa0/0.30    | 192.168.30.1    | 30   |
| Switch0     | VLAN 1      | 192.168.1.2     | Mgmt |
| PC0, PC1    | DHCP        | Otomatis        | 10   |
| PC2, PC3    | DHCP        | Otomatis        | 20   |
| PC4, PC5    | DHCP        | Otomatis        | 30   |

## ⚙️ Konfigurasi Router

```bash
# Masuk ke konfigurasi subinterface
conf t
interface fa0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit

interface fa0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit

interface fa0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit

# Konfigurasi DHCP
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10

ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8

ip dhcp pool VLAN30
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8
```

## ⚙️ Konfigurasi Switch

```bash
# Buat VLAN
conf t
vlan 10
 name LANTAI1
vlan 20
 name LANTAI2
vlan 30
 name LANTAI3

# Assign port ke VLAN
interface fa0/2
 switchport mode access
 switchport access vlan 10
exit

interface fa0/3
 switchport mode access
 switchport access vlan 10
exit

interface fa0/5
 switchport mode access
 switchport access vlan 20
exit

interface fa0/7
 switchport mode access
 switchport access vlan 20
exit

interface fa0/6
 switchport mode access
 switchport access vlan 30
exit

interface fa0/8
 switchport mode access
 switchport access vlan 30
exit

# Trunk ke router
interface fa0/1
 switchport mode trunk
```

## 🔍 Langkah Pengujian
1. Atur semua PC ke mode **DHCP**.
2. Jalankan `ipconfig` di masing-masing PC untuk memastikan IP otomatis sesuai VLAN.
3. Lakukan **ping antar VLAN** untuk memastikan routing berjalan.
4. Periksa lease DHCP dengan perintah `show ip dhcp binding` di router.

## 📂 Struktur Folder
```
/DHCP-Server-Cisco-Network
│── DHCP_Server.pkt    # File Packet Tracer
│── Topology.png       # Gambar topologi
│── README.md          # Dokumentasi proyek
```

## 📌 Catatan
- Pastikan port trunk dari switch ke router sudah dikonfigurasi dengan benar.
- DNS publik seperti **8.8.8.8** digunakan untuk resolusi nama jika simulasi mendukung.
