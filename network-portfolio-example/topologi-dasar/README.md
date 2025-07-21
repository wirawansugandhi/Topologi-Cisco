# 🧷 Topologi Dasar Switch

**Deskripsi Topologi:**
- 1 Switch
- 2 PC (PC0 dan PC1)
- Terhubung dalam satu LAN menggunakan kabel straight

**Tujuan:**
- Menguji koneksi dasar antara dua PC melalui switch
- Mempelajari cara memberi IP statis dan melakukan ping

---

## 💻 Konfigurasi IP

| Perangkat | IP Address     | Subnet Mask       |
|-----------|----------------|-------------------|
| PC0       | 192.168.10.1   | 255.255.255.0     |
| PC1       | 192.168.10.2   | 255.255.255.0     |

> Gateway tidak perlu diisi karena tidak ada router

---

## 🧪 Langkah Pengujian

1. Hubungkan kedua PC ke switch pada port yang berbeda
2. Klik masing-masing PC → Desktop → IP Configuration
   - Masukkan IP dan Subnet sesuai tabel
3. Uji koneksi dari PC0 ke PC1:
   ```bash
   ping 192.168.10.2
   ```
4. Harus muncul reply seperti:
   ```
   Reply from 192.168.10.2: bytes=32 time<1ms TTL=128
   ```

---

## 🗂️ File Simulasi
- `topologi-switch.pkt` (bisa dibuat menggunakan Cisco Packet Tracer)

---

## 📌 Catatan
- Pastikan kabel yang digunakan adalah **copper straight-through**
- Port switch akan aktif otomatis (tanpa konfigurasi)
