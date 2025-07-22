
# Lab Konfigurasi Static Routing IPv6 - Cisco Packet Tracer

Repositori ini berisi daftar perintah lengkap untuk mengkonfigurasi topologi jaringan dengan **Static Routing IPv6** menggunakan Cisco Packet Tracer.

## Topologi Jaringan

<img width="965" height="711" alt="gambar topologi static routing ipv6" src="https://github.com/user-attachments/assets/908dacd5-0a31-4624-8870-4e94adfb7c51" />


## Skema Pengalamatan IPv6

| Perangkat | Interface | Alamat IPv6         | Default Gateway |
| :-------- | :-------- | :------------------ | :-------------- |
| Router0   | Fa0/0     | `9abc::1/126`       | -               |
|           | Se0/0/0   | `1234::1/126`       | -               |
| Router1   | Se0/0/0   | `1234::2/126`       | -               |
|           | Se0/0/1   | `5678::2/126`       | -               |
| Router2   | Se0/0/1   | `5678::1/126`       | -               |
|           | Fa0/0     | `def0::1/126`       | -               |
| PC0       | Fa0       | `9abc::2/126`       | `9abc::1`       |
| PC1       | Fa0       | `def0::2/126`       | `def0::1`       |

---
Analisis Topologi dan Jaringan
Pertama, kita identifikasi semua jaringan (network) yang ada di topologi:

- Jaringan A: 9abc::0/126 (LAN di antara Router0 dan PC0)
- Jaringan B: 1234::0/126 (WAN di antara Router0 dan Router1)
- Jaringan C: 5678::0/126 (WAN di antara Router1 dan Router2)
- Jaringan D: def0::0/126 (LAN di antara Router2 dan PC1)

## Daftar Perintah Konfigurasi Lengkap

Berikut adalah semua perintah yang dibutuhkan, diurutkan berdasarkan perangkat.

### 1. Konfigurasi PC

#### PC0
-   **IPv6 Address:** `9abc::2`
-   **Prefix Length:** `126`
-   **IPv6 Gateway:** `9abc::1`

#### PC1
-   **IPv6 Address:** `def0::2`
-   **Prefix Length:** `126`
-   **IPv6 Gateway:** `def0::1`

### 2. Konfigurasi Router
Pada setiap router (Router0, Router1, dan Router2), Anda harus mengaktifkan kemampuan routing untuk IPv6. Tanpa perintah ini, router tidak akan meneruskan paket IPv6.
#### Router0
```cisco
Router#enable
Router(config)#configure terminal

! Aktifkan IPv6 Routing, 
router(config)#ipv6 unicast-routing
! Konfigurasi Interface
router(config)#interface FastEthernet0/0
router(config-if)#ipv6 address 9abc::1/126
router(config-if)#no shutdown
router(config-if)#exit

router(config)#interface Serial0/0/0
router(config-if)#ipv6 address 1234::1/126
router(config-if)#no shutdown
router(config-if)#exit

! Konfigurasi Static Default Route
router(config)#ipv6 route ::/0 1234::2
```
- Tujuan: Router0 perlu tahu cara mencapai Jaringan C (5678::0/126) dan Jaringan D (def0::0/126).
- Jalan Keluar (Next-Hop): Untuk mencapai kedua jaringan tersebut, Router0 harus mengirim paket ke Router1 melalui alamat IP 1234::2.


#### Router1
```cisco
Router#enable
Router(config)#configure terminal

! Aktifkan IPv6 Routing
Router(config)#ipv6 unicast-routing

! Konfigurasi Interface
Router(config)#interface Serial0/0/0
Router(config-if)#ipv6 address 1234::2/126
Router(config-if)#no shutdown
Router(config-if)#exit

Router(config)#interface Serial0/0/1
Router(config-if)#ipv6 address 5678::2/126
Router(config-if)#no shutdown
Router(config-if)#exit

! Konfigurasi Static Route
! Rute menuju jaringan PC0
Router(config)#ipv6 route 9abc::0/126 1234::1
! Rute menuju jaringan PC1
Router(config)#ipv6 route def0::0/126 5678::1

```
- Tujuan: Router1 berada di tengah. Ia perlu tahu cara mencapai Jaringan A (9abc::0/126) dan Jaringan D (def0::0/126).
- Jalan Keluar (Next-Hop):
  - Untuk mencapai Jaringan A, paket harus dikirim ke Router0 melalui 1234::1.
  - Untuk mencapai Jaringan D, paket harus dikirim ke Router2 melalui 5678::1.

#### Router2
```cisco
Router#enable
Router(config)#configure terminal

! Aktifkan IPv6 Routing
Router(config)#ipv6 unicast-routing

! Konfigurasi Interface
Router(config)#interface Serial0/0/1
Router(config-if)#ipv6 address 5678::1/126
Router(config-if)#no shutdown
Router(config-if)#exit

Router(config)#interface FastEthernet0/0
Router(config-if)#ipv6 address def0::1/126
Router(config-if)#no shutdown
Router(config-if)#exit

! Konfigurasi Static Default Route
! Semua trafik dengan tujuan tidak dikenal akan dikirim ke Router1 (5678::2)
ipv6 route ::/0 5678::2

```
Pada Router2 :
- Tujuan: Router2 perlu tahu cara mencapai Jaringan A (9abc::0/126) dan Jaringan B (1234::0/126).
- Jalan Keluar (Next-Hop): Untuk mencapai kedua jaringan tersebut, Router2 harus mengirim paket ke Router1 melalui alamat IP 5678::2.

### 3. Verifikasi

Untuk memeriksa apakah konfigurasi berhasil, lakukan tes `ping` dari PC0 ke PC1.

Buka **Command Prompt** pada **PC0**:
```bash
ping def0::2
```

Jika berhasil, Anda akan melihat output seperti ini:
```
Pinging def0::2 with 32 bytes of data:
Reply from def0::2: bytes=32 time<1ms TTL=125
Reply from def0::2: bytes=32 time<1ms TTL=125
```
### Langkah 4: Verifikasi dan Pengujian

Setelah semua konfigurasi selesai, saatnya melakukan verifikasi.

1.  **Cek Tabel Routing**
    Pada setiap router, gunakan perintah `show ipv6 route` untuk melihat apakah rute statis (ditandai dengan huruf `S`) sudah masuk ke tabel routing.
    *   **Di Router0:** Anda akan melihat rute statis menuju `5678::0/126` dan `def0::0/126` (atau satu default route `::/0`).
    *   **Di Router1:** Anda akan melihat rute statis menuju `9abc::0/126` dan `def0::0/126`.
    *   **Di Router2:** Anda akan melihat rute statis menuju `9abc::0/126` dan `1234::0/126` (atau satu default route `::/0`).

2.  **Lakukan Ping**
    Cara terbaik untuk menguji konektivitas adalah dengan melakukan ping dari ujung ke ujung.
    *   Buka PC0, masuk ke **Command Prompt**.
    *   Ketik perintah ping ke alamat IPv6 PC1:
      ```
      ping def0::2
      ```
    *   Jika konfigurasi benar, Anda akan mendapatkan balasan (Reply from def0::2...). Ping pertama mungkin gagal (timeout) karena proses ARP/ND, ini normal. Coba lagi dan seharusnya berhasil.

Dengan mengikuti langkah-langkah di atas, topologi jaringan Anda akan berfungsi sepenuhnya, dan PC0 bisa berkomunikasi dengan PC1 menggunakan rute statis IPv6 yang telah Anda tentukan.
