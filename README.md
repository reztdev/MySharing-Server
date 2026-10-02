# 📂 File Sharing (WebDAV)

Aplikasi desktop Windows untuk **berbagi folder di jaringan lokal (LAN)** lewat protokol **WebDAV**. Tinggal pilih folder, klik **Start Sharing**, lalu komputer lain bisa mengakses folder tersebut sebagai **drive** (Z:, Y:, dst.) di File Explorer, tanpa perlu install aplikasi tambahan di sisi client.

> **Versi:** 1.0.0  
> **Developer:** Muhammad Rizki (reztdev) · ryzkireztdev@gmail.com  
> © 2026 All Rights Reserved

---

## ✨ Fitur Utama

| Fitur | Keterangan |
|---|---|
| **Multi folder, satu URL** | Bagikan banyak folder sekaligus. Client cukup menyambung ke satu alamat dan semua folder muncul sebagai daftar. |
| **Login password (opsional)** | Pakai username `user` dan password buatanmu sendiri. |
| **HTTPS (opsional)** | Koneksi terenkripsi dengan sertifikat self-signed yang dibuat otomatis (TLS 1.2+). |
| **IP Whitelist & Blacklist** | Batasi siapa yang boleh masuk, mendukung IP tunggal maupun rentang CIDR. |
| **Batas jumlah client** | Atur maksimal berapa client yang boleh aktif bersamaan. |
| **Monitor client real-time** | Lihat siapa yang terhubung, berapa data yang dikirim/diterima, dan kapan terakhir aktif. |
| **Log aktivitas** | Semua akses, penolakan, dan percobaan login gagal tercatat. |
| **Script client otomatis** | Buat file `.bat` agar client bisa konek dengan satu klik. |
| **Config tersimpan otomatis** | Folder, port, dan pengaturan lain diingat saat aplikasi dibuka lagi. |
| **Portable** | Cukup satu file `.exe`, tidak perlu instalasi. |

---

## 🚀 Cara Pakai Cepat

**Di komputer server (yang membagikan folder):**

1. Buka aplikasi.
2. Di tab **Folders**, klik **Add...** lalu pilih folder yang mau dibagikan (bisa lebih dari satu).
3. *(Opsional)* Atur port, password, HTTPS, dan filter IP di tab lain.
4. Klik **Start Sharing**. Indikator di kiri bawah berubah **hijau**.
5. Alamat untuk client muncul di kotak **Client URL**. Klik **Copy** untuk menyalinnya.

**Di komputer client:**

- Buka File Explorer → klik kanan **This PC** → **Map network drive** → isi alamat dari server, **atau**
- Jalankan file `.bat` hasil **Script...** dari server (cara paling gampang, lihat bagian [Script Client](#-script-client-sekali-klik)).

---

## 🖥️ Penjelasan Tiap Tab

### 1. Folders
Tempat mengatur folder yang dibagikan.

- **Add...** → tambah folder baru. Nama share dibuat otomatis dari nama folder (huruf kecil semua). Kalau ada nama kembar, otomatis diberi akhiran `_2`, `_3`, dan seterusnya.
- **Remove** → hapus folder yang dipilih dari daftar (folder aslinya di komputer **tidak ikut terhapus**).
- **Check All / Uncheck** → centang atau hilangkan centang semua folder sekaligus.
- **Kotak centang (☑ / ☐)** → klik untuk menentukan folder mana yang ikut dibagikan saat sharing dijalankan.
- **Baris berwarna merah** → folder tersebut sudah tidak ditemukan di komputer (terhapus, dipindah, atau drive-nya tidak terpasang).
- **Client URL** → daftar alamat yang bisa dipakai client.
  - Jika ada **lebih dari satu folder**, baris pertama adalah alamat **utama** yang menampilkan semua folder dalam satu drive. Baris berikutnya adalah alamat langsung ke tiap folder.
  - Jika hanya **satu folder**, alamat utama langsung mengarah ke folder tersebut.
  - **Copy** menyalin alamat yang dipilih (atau semua alamat kalau tidak ada yang dipilih).
  - **Script...** membuat file `.bat` untuk client.

### 2. Settings
- **Port** → nomor port server. Bawaan `8040`. Bisa diganti 1–65535.
- **Max Clients** → batas jumlah client aktif bersamaan (1–50, bawaan 3).
- **Enable HTTPS** → mengaktifkan enkripsi. Tombol **Export Cert...** akan aktif untuk menyimpan sertifikat yang harus dipasang di komputer client.
- **Enable Password** → mewajibkan login. Username selalu `user`.

> 🔒 Semua pengaturan di tab ini **terkunci saat sharing berjalan**. Stop dulu kalau mau mengubahnya.

### 3. Access (Filter IP)
Dua daftar terpisah yang masing-masing punya tombol **Enable**:

- **Whitelist** → kalau aktif, **hanya** IP di daftar ini yang boleh masuk.
- **Blacklist** → kalau aktif, IP di daftar ini **diblokir**.

Format yang diterima: IP tunggal (`192.168.1.20`) atau rentang jaringan (`192.168.1.0/24`).

Aturan penting:
- **Blacklist dicek lebih dulu** daripada whitelist.
- Tab ini **bisa diubah saat sharing berjalan**, dan perubahan langsung berlaku.
- Kalau IP diblokir ketika sedang mengunduh/mengunggah, koneksinya **diputus di tengah jalan**.
- Whitelist yang aktif tapi **kosong** akan menolak semua orang, jadi aplikasi akan memberi peringatan sebelum Start.

### 4. Clients
Monitor client yang sedang atau pernah terhubung.

| Kolom | Arti |
|---|---|
| **IP** | Alamat IP client |
| **Status** | `Active` atau `Idle` |
| **Reqs** | Jumlah request yang pernah dikirim client |
| **Sent** | Total data yang dikirim server ke client (download oleh client) |
| **Recv** | Total data yang diterima server dari client (upload oleh client) |
| **Last** | Berapa lama sejak aktivitas terakhir (`now`, `30s`, `5m`, `2h`) |

- Client dianggap **Active** jika sedang ada transfer berjalan, atau terakhir request kurang dari **60 detik** lalu.
- **Block IP** → langsung memasukkan IP yang dipilih ke blacklist dan mengaktifkannya.
- **Clear Idle** → membersihkan daftar client yang sudah tidak aktif.
- Label **Active: X / Y** di atas menunjukkan jumlah client aktif dibanding batas maksimal.

### 5. Log
Catatan aktivitas server dengan jam. Aplikasi hanya menyimpan sekitar **500 baris terakhir** supaya tidak berat. Jenis pesannya:

| Tag | Arti |
|---|---|
| `[INFO]` | Informasi umum (server mulai/berhenti, alamat, dll.) |
| `[ACCESS]` | Setiap request client: IP, jenis aksi, alamat file, dan kode hasilnya |
| `[AUTH]` | Login gagal, lengkap dengan username yang dicoba (password **tidak pernah** dicatat) |
| `[DENIED]` | Client ditolak (diblokir, tidak masuk whitelist, atau melebihi batas client) |
| `[WARN]` | Peringatan, misalnya folder hilang atau config tidak terbaca |
| `[ERROR]` | Error saat menjalankan atau menghentikan server |

Pesan penolakan yang berulang dari IP yang sama dibatasi (maksimal 1 kali per 10 detik) supaya log tidak banjir.

### 6. About
Informasi versi dan pembuat aplikasi.

---

## ⚙️ Cara Kerja

### Alur saat klik "Start Sharing"
Aplikasi memeriksa beberapa hal dulu sebelum server benar-benar menyala:

1. Minimal ada **1 folder tercentang** dan semua folder tersebut **masih ada**.
2. Port dan Max Clients berupa angka yang valid.
3. Jika password aktif, kolom password **tidak boleh kosong**.
4. Whitelist yang aktif **tidak boleh kosong**.
5. **Port belum dipakai** aplikasi lain.
6. Jika HTTPS aktif, sertifikat disiapkan (dibuat baru kalau belum ada atau sudah tidak cocok).

Kalau ada yang gagal, muncul pesan peringatan dan server tidak dijalankan. Kalau semua lolos, server menyala di semua kartu jaringan komputer dan alamat client ditampilkan.

### Alur setiap request dari client
Setiap permintaan dari client melewati tahapan berikut secara berurutan:

```
Client  →  [1] Cek IP  →  [2] Cek batas client  →  [3] Cek login  →  Folder dilayani
              ↓                  ↓                      ↓
           403 Ditolak       403 Ditolak           401 Minta login
```

1. **Cek IP** → diperiksa terhadap blacklist lalu whitelist. Kalau tidak lolos, ditolak.
2. **Cek batas client** → kalau client ini belum aktif dan jumlah client aktif lain sudah mencapai batas, ditolak dengan pesan "Max clients reached". Client yang sudah aktif tidak terpengaruh.
3. **Cek login** → kalau password aktif, client diminta username dan password.
4. **Layani file** → file dan folder dikirim, sementara aplikasi mencatat jumlah data dan aktivitasnya.

### Struktur folder yang dilihat client
- **Satu folder dibagikan** → alamat utama (`http://IP:PORT/`) langsung berisi isi folder tersebut.
- **Banyak folder dibagikan** → alamat utama menampilkan **daftar folder** (hanya-baca, tidak bisa tambah/hapus folder dari sini). Masuk ke tiap folder untuk melihat isinya.

### Soal login
- Username selalu **`user`**.
- Format username ala Windows seperti `PCNAME\user`, `.\user`, atau `user@host` **juga diterima**, karena Windows sering menambahkan nama komputer di depan username secara otomatis.
- Seluruh folder memakai **satu sesi login yang sama**, jadi client Windows tidak akan terus-terusan muncul popup login saat berpindah folder.
- Password **tidak pernah disimpan** ke file. Harus diisi ulang setiap kali mau Start Sharing.

### HTTPS & sertifikat
- Sertifikat dibuat otomatis dan disimpan di folder `certs` di samping file `.exe`.
- Sertifikat mencakup `localhost`, nama komputer, IP lokal, dan `127.0.0.1`.
- Berlaku sekitar **825 hari** dan dibuat ulang otomatis kalau IP komputer berubah atau masa berlakunya kurang dari 30 hari.
- Karena **self-signed**, client harus mempercayainya: klik **Export Cert...**, kirim file `.crt` ke client, lalu install di client.
- Kalau sertifikat dibuat ulang, **sertifikat lama di client tidak berlaku lagi** dan harus di-export dan di-install ulang. Aplikasi akan memberi tahu lewat log.

### Menghentikan sharing
Klik **Stop Sharing**. Server dimatikan di latar belakang, daftar client dikosongkan, dan semua pengaturan terbuka kembali. Tombol akan nonaktif sebentar sampai server benar-benar berhenti.

---

## 💾 Data yang Disimpan

Aplikasi menyimpan data di **folder yang sama dengan file `.exe`**:

| Item | Isi |
|---|---|
| `config.json` | Daftar folder (beserta status centang), port, max clients, status HTTPS, status password (aktif/tidak), dan daftar whitelist/blacklist |
| `certs/` | Sertifikat dan kunci HTTPS (hanya muncul kalau HTTPS pernah dipakai) |

- Config disimpan **otomatis** setiap ada perubahan, tidak perlu tombol Save.
- **Password tidak ikut disimpan.**
- Kalau `config.json` rusak atau tidak terbaca, aplikasi memakai pengaturan bawaan dan menampilkan peringatan di Log (tidak crash).
- Folder `certs` berisi **kunci privat**. Jangan dibagikan ke orang lain. Yang boleh dibagikan hanya file `.crt` hasil **Export Cert**.

---

## 📜 Script Client (Sekali Klik)

Tombol **Script...** di tab Folders membuat file `connect-file-sharing.bat` yang bisa dikirim ke komputer client. Setelah dijalankan, script akan:

1. Menyalakan layanan **WebClient** Windows (diperlukan untuk WebDAV) kalau belum jalan.
2. Mencari huruf drive yang masih kosong (dari Z mundur ke M).
3. Menyambungkan alamat server sebagai drive tersebut. Kalau password aktif, client akan diminta mengetik password.
4. Langsung membuka drive-nya di File Explorer.

Catatan:
- Server harus sudah **Start Sharing** dulu sebelum script bisa dibuat (karena alamatnya baru ada setelah server jalan).
- Koneksi dibuat **tidak permanen**: setelah client restart atau logoff, drive hilang dan script perlu dijalankan lagi.
- Kalau HTTPS aktif, **sertifikat harus sudah di-install** di client sebelum menjalankan script.
- Menyalakan layanan WebClient mungkin butuh hak **Administrator** di komputer client.

---

## 🔌 Cara Konek Manual dari Client

**Windows (Map Network Drive):**
1. Buka File Explorer → klik kanan **This PC** → **Map network drive**.
2. Pada kolom *Folder*, isi alamat dari server, contoh `http://192.168.1.10:8040/`.
3. Centang *Connect using different credentials* kalau server memakai password, lalu isi username `user` dan password-nya.

**Browser:** alamat server juga bisa dibuka langsung di browser untuk melihat daftar file (tampilan direktori sederhana).

**Perangkat lain (Mac, Linux, HP):** pakai aplikasi/klien yang mendukung WebDAV dengan alamat server yang sama.

---

## 🛡️ Catatan Keamanan

- Folder yang dibagikan bersifat **baca dan tulis**. Client yang berhasil masuk bisa **mengunggah, mengubah, dan menghapus** file di folder tersebut. Hati-hati memilih folder yang dibagikan.
- Tanpa password, **siapa saja di jaringan yang tahu alamatnya bisa mengakses**. Gunakan password dan/atau whitelist IP, terutama di jaringan umum atau kantor.
- Tanpa HTTPS, data dan login berjalan tanpa enkripsi. Cukup aman untuk LAN rumah yang dipercaya, tapi sebaiknya aktifkan HTTPS di jaringan yang kurang dipercaya.
- Aplikasi ini ditujukan untuk **jaringan lokal**. Jangan membuka port-nya langsung ke internet.
- Gunakan password yang kuat dan jangan dipakai ulang dari akun penting lain.

---

## 🧰 Troubleshooting

| Masalah | Kemungkinan Penyebab & Solusi |
|---|---|
| **"Port dipakai"** saat Start | Port sedang dipakai aplikasi lain. Ganti port di tab Settings. |
| **Client tidak bisa konek** | Pastikan server sudah Start, alamat dan port benar, kedua komputer satu jaringan, dan **Windows Firewall** mengizinkan aplikasi/port tersebut. |
| **Client ditolak ("Max clients reached")** | Batas client sudah penuh. Naikkan Max Clients, atau tunggu client lain tidak aktif (sekitar 60 detik). |
| **Client ditolak ("Access denied")** | IP client masuk blacklist atau tidak ada di whitelist. Cek tab Access dan Log. |
| **Login terus ditolak** | Username harus `user`. Cek password dan lihat tab Log untuk pesan `[AUTH]`. |
| **Peringatan sertifikat / HTTPS gagal di client** | Sertifikat belum di-install di client, atau sudah dibuat ulang. Export ulang dan install lagi. |
| **Folder berwarna merah** | Folder aslinya sudah tidak ada. Hapus dari daftar atau kembalikan foldernya. |
| **Alamat IP salah / berubah** | Aplikasi memakai IP lokal yang terdeteksi saat Start. Kalau berpindah jaringan (Wi-Fi/LAN), Stop lalu Start lagi supaya alamat diperbarui. |
| **Upload file besar gagal dari Windows** | Klien WebDAV bawaan Windows punya batas ukuran file bawaan (umumnya sekitar 50 MB) yang bisa diubah lewat pengaturan Windows di sisi client. |
| **Tombol Start tidak bisa diklik sebentar setelah Stop** | Normal, server sedang berhenti di latar belakang. |

---

## ❓ FAQ

**Apakah client perlu install aplikasi ini juga?**  
Tidak. Client cukup memakai fitur WebDAV bawaan Windows (atau klien WebDAV lain).

**Bisa dipakai lewat internet?**  
Tidak disarankan. Aplikasi dirancang untuk jaringan lokal. Kalau terpaksa, minimal wajib pakai HTTPS, password kuat, dan whitelist IP.

**Kenapa password harus diisi lagi tiap Start?**  
Demi keamanan, password sengaja tidak disimpan ke disk.

**Kenapa nama share saya jadi huruf kecil semua?**  
Server WebDAV memperlakukan nama share tanpa membedakan huruf besar/kecil, sehingga nama otomatis dibuat huruf kecil untuk menghindari bentrok.

**Pengaturan saya hilang / kembali ke bawaan?**  
Pastikan `config.json` masih ada di folder yang sama dengan `.exe`. Kalau `.exe` dipindah ke folder lain, aplikasi akan mulai dari pengaturan bawaan.

**Bisa ganti username?**  
Tidak, username tetap `user`.

---

## 🧾 Ringkasan Nilai Bawaan

| Pengaturan | Nilai Bawaan |
|---|---|
| Port | `8040` |
| Max Clients | `3` |
| HTTPS | Nonaktif |
| Password | Nonaktif |
| Username | `user` (tetap) |
| Whitelist / Blacklist | Nonaktif & kosong |
| Batas waktu client dianggap aktif | 60 detik |
| Masa berlaku sertifikat | ±825 hari |
| Log yang disimpan | ±500 baris terakhir |

---

*Dibuat oleh Muhammad Rizki (reztdev) · © 2026 All Rights Reserved*
