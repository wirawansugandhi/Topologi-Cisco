
```markdown
# Konfigurasi Jaringan Dasar dengan Static Routing di Cisco Packet Tracer

Repositori ini berisi panduan lengkap untuk mengkonfigurasi jaringan sederhana menggunakan Cisco Packet Tracer. Topologi ini terdiri dari dua jaringan lokal (LAN) yang dihubungkan oleh dua router menggunakan metode **Static Routing**.

Tujuan utama dari konfigurasi ini adalah untuk memungkinkan semua perangkat, terutama PC dari jaringan yang berbeda, agar dapat saling berkomunikasi.

## Topologi Jaringan



### Tabel Pengalamatan (Addressing Table)

| Perangkat | Interface | IP Address      | Subnet Mask     | Default Gateway |
| :-------- | :-------- | :-------------- | :-------------- | :-------------- |
| Router1   | Fa0/0     | `192.168.10.1`  | `255.255.255.0` | -               |
|           | Fa0/1     | `192.168.50.1`  | `255.255.255.0` | -               |
| Router2   | Fa0/0     | `192.168.10.2`  | `255.255.255.0` | -               |
|           | Fa0/1     | `192.168.100.2` | `255.255.255.0` | -               |
| PC1       | Fa0       | `192.168.50.11` | `255.255.255.0` | `192.168.50.1`  |
| PC2       | Fa0       | `192.168.50.22` | `255.255.255.0` | `192.168.50.1`  |
| PC3       | Fa0       | `192.168.100.11`| `255.255.255.0` | `192.168.100.2` |
| PC4       | Fa0       | `192.168.100.22`| `255.255.255.0` | `192.168.100.2` |

## Prasyarat

*   Perangkat lunak **Cisco Packet Tracer** sudah terinstal.

## Langkah-langkah Konfigurasi

### 1. Konfigurasi IP pada PC

Konfigurasikan setiap PC dengan IP Address, Subnet Mask, dan Default Gateway sesuai dengan tabel pengalamatan di atas. Default Gateway adalah alamat IP dari interface router yang terhubung ke jaringan lokal PC tersebut.

### 2. Konfigurasi Interface Router

Atur alamat IP pada setiap interface router dan aktifkan interface tersebut.

#### Konfigurasi Router1

```cisco
Router> enable
Router# configure terminal
Router(config)#

! Konfigurasi interface ke arah LAN
Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 192.168.50.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

! Konfigurasi interface ke arah Router2
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit


```

#### Konfigurasi Router2

```cisco
Router> enable
Router# configure terminal
Router(config)#

! Konfigurasi interface ke arah LAN
Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 192.168.100.2 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

! Konfigurasi interface ke arah Router1
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.10.2 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit


```

### 3. Konfigurasi Static Routing

Tambahkan rute statis pada setiap router agar mereka mengetahui cara mencapai jaringan yang tidak terhubung langsung.

#### Pada Router1
Tambahkan rute untuk mencapai jaringan `192.168.100.0/24` melalui `192.168.10.2` (next-hop).

```cisco
Router(config)# ip route 192.168.100.0 255.255.255.0 192.168.10.2

```
- 192.168.100.0 : Jaringan tujuan yang ingin dicapai.
- 255.255.255.0 : Subnet mask dari jaringan tujuan.
- 192.168.10.2 : Alamat IP "next-hop", yaitu alamat interface router tetangga yang akan menerima paket ini untuk diteruskan ke jaringan tujuan.

#### Pada Router2
Tambahkan rute untuk mencapai jaringan `192.168.50.0/24` melalui `192.168.10.1` (next-hop).

```cisco
Router(config)# ip route 192.168.50.0 255.255.255.0 192.168.10.1

```
- 192.168.50.0 : Jaringan tujuan yang ingin dicapai.
- 255.255.255.0 : Subnet mask dari jaringan tujuan.
- 192.168.10.1 : Alamat IP "next-hop", yaitu alamat interface router tetangga yang akan menerima paket ini untuk diteruskan ke jaringan tujuan.

### 4. Verifikasi dan Pengujian

Lakukan pengujian konektivitas dari berbagai titik untuk memastikan seluruh konfigurasi berhasil.

1.  **Tes koneksi PC ke Gateway**
    *   Dari **PC1** (Desktop > Command Prompt):
    ```bash
    C:\> ping 192.168.50.1
    ```
    Pastikan Anda mendapatkan balasan (Reply from...). Lakukan hal yang sama untuk PC lainnya.
    
2.  **Tes koneksi antar Router**
    *   Dari **Router1** (CLI):
    ```cisco
    Router1# ping 192.168.10.2
    ```
    Pastikan Success rate tinggi (misalnya, 100 percent).
    
3.  **Tes koneksi antar jaringan (End-to-End)**
    *   Dari **PC1** (Desktop > Command Prompt):
    ```bash
    C:\> ping 192.168.100.22
    ```
    Jika berhasil, Anda akan melihat balasan (Reply from...). Ini menunjukkan bahwa static routing Anda berfungsi dengan baik dan seluruh jaringan terhubung.
    
> **Catatan:** `ping` pertama kali ke perangkat di jaringan yang berbeda mungkin akan gagal atau menampilkan "Request timed out" karena proses ARP (Address Resolution Protocol). Coba ulangi `ping` untuk kedua kalinya untuk mendapatkan hasil yang akurat.

---
```
