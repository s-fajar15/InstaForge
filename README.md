# 🔥 InstaForge

> **Buat akun Instagram otomatis.**

InstaForge adalah *tools* otomasi berbasis Python untuk membuat akun Instagram
secara massal, melengkapi profil (foto & bio), mengaktifkan 2FA, dan
menyimpan datanya secara otomatis.

---

## ✨ Fitur

| Fitur | Keterangan |
|-------|-----------|
| 📧 **Registrasi Email** | Alur pendaftaran lengkap via email + kode konfirmasi |
| 👤 **Auto Data** | Nama (Faker id_ID), tanggal lahir (≥18 thn), password acak |
| 🎨 **Auto Profil** | Unggah foto profil & set bio acak dari 100+ pilihan |
| 👥 **Auto Follow** | Follow akun target otomatis setelah akun jadi |
| 🔐 **Auto 2FA** | Aktivasi Two-Factor Authentication + recovery codes |
| 💾 **Auto Simpan** | Semua data (username, password, cookie, 2FA) tersimpan ke JSON |
| ⚙️ **Batch Mode** | Buat banyak akun sekaligus dengan delay yang bisa diatur |
| 🖥️ **CLI Berwarna** | Tampilan menu & progress yang rapi dan informatif |

---

## 📁 Struktur Proyek

```
project/
├── main.py                  # Entry point
├── requirements.txt
├── config/
│   └── settings.py          # State global & path penyimpanan
├── core/
│   ├── constants.py         # Device ID, URL, user agent
│   ├── payload.py           # Builder payload Bloks CAA
│   ├── session.py           # Builder header HTTP
│   ├── tampilan.py          # Helper tampilan CLI (warna, kartu)
│   └── utils.py             # Generator nama & tanggal lahir
├── account/
│   ├── creator.py           # Orkestrator pembuatan akun
│   └── following.py         # Auto follow
├── profile/
│   ├── edit.py              # Antarmuka edit profil
│   └── upload.py            # Upload foto & bio
├── security/
│   └── two_factor.py        # Aktivasi 2FA
├── menu/
│   ├── main.py              # Menu utama
│   └── start.py             # Alur batch
└── data/
    ├── bio.py               # Daftar bio acak
    └── NewAccount.json      # Hasil (auto-generate)
```

---

## 🚀 Instalasi

### 1. Clone / Salin Proyek

Letakkan seluruh folder proyek di direktori mana saja, lalu masuk ke folder:

```bash
cd project/
```

### 2. Buat Virtual Environment (opsional tapi disarankan)

```bash
python -m venv venv

# Linux / macOS
source venv/bin/activate

# Windows
venv\Scripts\activate
```

### 3. Install Dependensi

```bash
pip install -r requirements.txt
```

Isi `requirements.txt`:
```
requests
faker
beautifulsoup4
```

### 4. Siapkan Folder Foto

Buat folder untuk foto profil (khusus Android/Termux):

```bash
mkdir -p /storage/emulated/0/CreatorIG/Profile
```

Isi dengan beberapa file `.jpg` / `.jpeg` / `.png`. Foto akan dipilih acak.

> **Linux/Windows:** ubah path di `config/settings.py` bagian `folder_foto`
> ke path lokal, misal `./assets/profile/`.

---

## 🎮 Cara Pakai

Jalankan:

```bash
python main.py
```

### Menu Utama

```
     [1]  Buat akun baru
     [2]  Penyimpanan akun
     [3]  Edit Profil
     [4]  Stats tool
     [5]  Keluar
```

### 1. Buat Akun Baru

Kamu akan ditanya:
- Jumlah akun yang ingin dibuat
- Delay antar akun (detik)
- Email untuk setiap akun (input manual)
- Kode verifikasi dari email

Proses berjalan otomatis:
```
━━━ Akun 1 dari 3 ━━━

❯ Email » user@mail.com
❯ Masukan kode verifikasi email » 123456

✔  Password diset    ·  #PWD_BROWSER:0:...:Budi123#
✔  Tanggal lahir     ·  14-03-1998
✔  Nama diset        ·  Budi Santoso
✔  Username valid    ·  @budi_santoso42

╭──────────────────────────────────────────────╮
│  ✔  AKUN BERHASIL DIBUAT                     │
╰──────────────────────────────────────────────╯
  │ Username   : budi_santoso42
  │ Password   : #PWD_BROWSER:0:...:Budi123#
  │ Email      : user@mail.com
  │ Birthday   : 14-03-1998
  │ Foto       : Berhasil Upload Foto Profile
  │ Bio        : Berhasil Menambahkan Bio
  │ Follow     : Berhasil Follow, account: s.fajar_15
  │ 2FA        : ABC123  (Two Factor active)
──────────────────────────────────────────────
```

### 2. Penyimpanan

Menampilkan semua akun tersimpan dari `data/NewAccount.json`.

### 3. Edit Profil

- Tambah bio acak
- Unggah foto profil

Pilih akun dari daftar, lalu jalankan aksi.

### 4. Stats

Menampilkan jumlah akun berhasil & gagal di sesi berjalan.

---

## 💾 Format Penyimpanan

Data disimpan di `data/NewAccount.json` dalam format sederhana:

```
username|password
email|nama
tanggal_lahir
kode_2fa
cookie
```

Contoh:
```
budi_santoso42|Budi123#
user@mail.com|Budi Santoso
14-03-1998
ABC123
csrftoken=...;datr=...;sessionid=...
```

> ⚠️ File ini berisi **credential sensitif**. Jangan pernah di-commit ke Git publik.

---

## ⚙️ Konfigurasi

Edit `config/settings.py` untuk mengubah:

| Variabel | Fungsi | Default |
|----------|--------|---------|
| `folder_data` | Nama folder data | `"Data"` |
| `berkas_akun` | Path file akun | `"Data/NewAccount.json"` |
| `folder_foto` | Folder foto profil | `/storage/emulated/0/CreatorIG/Profile` |

Edit `account/following.py` untuk mengganti **akun target auto-follow**:
```python
self.users = ["s.fajar_15", "xxf1925"]
```

Edit `data/bio.py` untuk menambah/mengubah daftar bio.

---

## 🛠️ Troubleshooting

| Masalah | Solusi |
|---------|--------|
| `ModuleNotFoundError: faker` | `pip install -r requirements.txt` |
| `actorID tidak ditemukan` | Cookie tidak valid / akun kena checkpoint |
| `noncoercible_variable_value` | `device_id` di payload 2FA tidak valid — pastikan cookie punya `ig_did` |
| Foto tidak terunggah | Cek folder foto ada & berisi file gambar |
| Akun langsung diblokir | Gunakan email dari domain yang jarang dipakai (lihat catatan di bawah) |
| `DATA/NewAccount.json` tidak ada | Folder `Data/` harus ada — buat manual kalau perlu |

---

## 📌 Catatan Penting

- **Gunakan email yang tidak mudah diblokir.** Hindari Mailinator, 10MinuteMail,
  atau domain utama Temp-Mail. Pilih layanan dengan rotasi domain baik.
- **Gunakan delay 60+ detik** antar akun untuk mengurangi deteksi.
- **Selesaikan verifikasi di sesi yang sama.** Jangan tutup tab sebelum selesai.
- **Siapkan nomor HP cadangan.** Instagram mungkin meminta verifikasi HP
  meskipun email sudah diverifikasi.
- **Jangan gunakan tools ini untuk spam, penipuan, atau aktivitas ilegal.**

---

## ⚠️ Disclaimer

Tools ini dibuat **hanya untuk tujuan edukasi & penelitian**. Penggunaan untuk
membuat akun palsu, spam, atau pelanggaran Ketentuan Layanan Instagram
sepenuhnya menjadi tanggung jawab pengguna.

Penulis tidak bertanggung jawab atas:

- Akun yang diblokir atau dihapus oleh Instagram
- Penyalahgunaan data yang tersimpan
- Kerugian apa pun yang timbul dari penggunaan tools ini

**Gunakan dengan bijak dan bertanggung jawab.**

---

## 📜 Lisensi

MIT License — bebas digunakan untuk keperluan pribadi.

---

## 👤 Kredit

- **Author:** s.fajar
- **Versi:** 2.0
- **Tahun:** 2026

---

<p align="center">
  <b>🔥 InstaForge</b><br>
  <i>Buat akun Instagram otomatis.</i>
</p>
