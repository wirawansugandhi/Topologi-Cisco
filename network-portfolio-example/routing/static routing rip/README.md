# Static Routing & RIP v1 – Cisco Packet Tracer

## 1. Deskripsi
Lab ini menunjukkan cara menghubungkan dua jaringan LAN yang berbeda menggunakan **Static Routing** dan **Routing Information Protocol versi 1 (RIPv1)** pada perangkat Cisco.  
Topologi terdiri dari dua router yang terhubung serial dan masing-masing menghubungkan satu LAN.

---

## 2. Topologi
```
[PC0] --- [Switch0] --- [Router0] ===serial=== [Router1] --- [Switch1] --- [PC1]
```
- **Router0**
  - FastEthernet0/0 : `192.168.10.1/24`
  - Serial0/0/0     : `192.168.20.1/24`
- **Router1**
  - FastEthernet0/0 : `192.168.30.1/24`
  - Serial0/0/0     : `192.168.20.2/24`
- **PC0**
  - IP Address : `192.168.10.10/24`
  - Gateway    : `192.168.10.1`
- **PC1**
  - IP Address : `192.168.30.10/24`
  - Gateway    : `192.168.30.1`

---

## 3. Konfigurasi IP Address
### Router0
```bash
enable
configure terminal
interface FastEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface Serial0/0/0
 ip address 192.168.20.1 255.255.255.0
 clock rate 64000
 no shutdown
exit
```

### Router1
```bash
enable
configure terminal
interface FastEthernet0/0
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit
interface Serial0/0/0
 ip address 192.168.20.2 255.255.255.0
 no shutdown
exit
```

---

## 4. Konfigurasi Static Routing
### Router0
```bash
ip route 192.168.30.0 255.255.255.0 192.168.20.2
```

### Router1
```bash
ip route 192.168.10.0 255.255.255.0 192.168.20.1
```

---

## 5. Konfigurasi RIP v1
*(Hapus static route terlebih dahulu sebelum mengaktifkan RIP v1)*

### Router0
```bash
router rip
 version 1
 network 192.168.10.0
 network 192.168.20.0
```

### Router1
```bash
router rip
 version 1
 network 192.168.30.0
 network 192.168.20.0
```

---

## 6. Pengujian
1. **PC0 → PC1**
   ```bash
   ping 192.168.30.10
   ```
2. **PC1 → PC0**
   ```bash
   ping 192.168.10.10
   ```
3. Pastikan semua *reply* sukses.

---

## 7. Catatan
- RIPv1 tidak mendukung **VLSM**.
- Untuk static routing, pastikan setiap router memiliki route ke jaringan yang tidak langsung terhubung.
- Clock rate diperlukan di salah satu ujung serial link.
