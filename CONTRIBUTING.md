# Panduan Kontribusi (Contributing Guide)

Terima kasih atas ketertarikan Anda untuk berkontribusi pada **Clipping Studio**! Dokumen ini memuat panduan, standar, dan langkah-langkah yang diperlukan untuk berkontribusi pada proyek ini.

---

## 1. Selamat Datang

Selamat datang di repositori **Clipping Studio**! ✂️

Clipping Studio adalah aplikasi lokal untuk memotong video panjang menjadi klip-klip pendek vertikal/horizontal (TikTok, Reels, Shorts) secara otomatis menggunakan Kecerdasan Buatan seperti Gemini, Claude, atau ChatGPT. Klip dirender langsung di komputer lokal menggunakan FFmpeg dengan fitur pelacakan wajah (*face tracking*), penghapusan jeda diam (*silence removal*), subtitle otomatis tersinkronisasi (*whisper*), penambahan logo, hingga penerbitan terjadwal ke media sosial via Buffer.

Kami sangat menyambut berbagai bentuk kontribusi, baik berupa perbaikan bug, penambahan fitur baru, perbaikan dokumentasi, maupun pelaporan isu.

---

## 2. Prasyarat Sistem

Sebelum memulai penyiapan lokal, pastikan lingkungan komputer Anda telah memenuhi prasyarat berikut:

- **Python**: Versi `3.10` atau lebih baru (disarankan Python 3.12).
- **Git**: Versi terbaru untuk manajemen kode sumber.
- **Tool Manajemen Paket**:
  - [uv](https://github.com/astral-sh/uv) (sangat disarankan karena jauh lebih cepat dalam instalasi dependensi).
  - Atau paket bawaan Python standard: `python -m venv` dan `pip`.
- *(Catatan)*: FFmpeg tidak perlu diinstal manual secara global karena sudah terpasang otomatis melalui pustaka `imageio-ffmpeg`.

---

## 3. Langkah Instalasi

Ikuti langkah-langkah berikut untuk memasang dan menjalankan proyek di lingkungan pengembangan lokal Anda:

### 3.1. Clone Repository
Clone repository ke komputer Anda:
```bash
git clone https://github.com/yudeka-eureka/clipping-studio.git
```

### 3.2. Masuk ke Direktori Proyek
```bash
cd clipping-studio
```

### 3.3. Buat dan Aktifkan Virtual Environment
Pilih salah satu metode berikut:

**Opsi A: Menggunakan `uv` (Disarankan)**
```bash
uv venv .venv
source .venv/bin/activate
```

**Opsi B: Menggunakan `venv` standar Python**
```bash
python3 -m venv .venv
source .venv/bin/activate
```
*(Bagi pengguna Windows PowerShell, aktifkan via `.venv\Scripts\Activate.ps1`)*

### 3.4. Pasang Dependensi
Pasang semua library yang dibutuhkan:
```bash
# Jika menggunakan uv:
uv pip install -r requirements.txt

# Atau jika menggunakan pip standar:
pip install -r requirements.txt
```

### 3.5. Buat Berkas Konfigurasi
Salin template konfigurasi lingkungan (`.env.example`) menjadi `.env`:
```bash
cp .env.example .env
```
Buka berkas `.env` dan lengkapi API key yang diperlukan (seperti `GEMINI_API_KEY`, `ANTHROPIC_API_KEY`, atau `OPENAI_API_KEY`). Anda juga dapat mengaturnya nanti melalui antarmuka web.

---

## 4. Verifikasi Instalasi

Pastikan seluruh komponen dan dependensi telah terpasang dengan baik:

### 4.1. Menjalankan Perintah Diagnosis
Jalankan utilitas `clip doctor` untuk memverifikasi ketersediaan FFmpeg, MediaPipe, modul Whisper, API key, dan konfigurasi lainnya:
```bash
./clip doctor
```
Pastikan seluruh status penting menunjukkan tanda centang / siap digunakan.

### 4.2. Menjalankan Antarmuka Web
Jalankan aplikasi web lokal:
```bash
./run.sh
```
Aplikasi akan otomatis membuka peramban di alamat `http://localhost:8765`. Anda dapat mencoba memuat video pendek untuk menguji fungsi deteksi dan render.

---

## 5. Standar Alur Git & Penamaan Branch

Untuk menjaga riwayat repositori tetap rapi dan mudah ditelusuri, ikuti konvensi branch dan commit berikut:

### 5.1. Format Penamaan Branch
Buat branch baru dari branch `main` dengan format:
- `feat/<nama-fitur>`: Penambahan fitur atau kapabilitas baru (contoh: `feat/subtitle-style-presets`).
- `fix/<nama-bug>`: Perbaikan bug atau galat (contoh: `fix/audio-desync-on-trim`).
- `docs/<judul-dokumen>`: Perubahan atau pembaruan dokumentasi (contoh: `docs/contributing-guide`).

### 5.2. Konvensi Pesan Commit
Gunakan format [Conventional Commits](https://www.conventionalcommits.org/):
- `feat: ...` : Menambahkan fitur baru.
- `fix: ...` : Memperbaiki bug.
- `docs: ...` : Perubahan dokumentasi saja.
- `test: ...` : Menambahkan atau memperbarui pengujian.
- `refactor: ...` : Pengubahan struktur kode tanpa mengubah fungsionalitas.
- `chore: ...` : Pemeliharaan dependensi, skrip build, atau tooling.

Contoh pesan commit yang baik:
```bash
git commit -m "feat: tambahkan opsi export subtitle ass"
git commit -m "fix: tangani error saat kuota gemini habis"
```

---

## 6. Tata Cara Membuat Pull Request (PR)

Ketika kontribusi Anda sudah siap untuk digabungkan, ajukan Pull Request (PR) dengan panduan berikut:

### 6.1. Checklist Sebelum Membuat PR
Sebelum melakukan `git push` dan membuka PR, pastikan:
- [ ] Berkas rahasia/kredensial **`.env` tidak ter-commit**.
- [ ] Berkas video besar, job output, cache, atau data sementara di direktori **`data/` tidak ter-commit**.
- [ ] Tidak ada berkas sementara seperti `.DS_Store`, `*.tmp`, atau `__pycache__/` yang ikut terunggah.
- [ ] Kode telah diuji secara lokal dan berjalan dengan baik.
- [ ] Diagnosis `./clip doctor` berhasil tanpa kendala kritis.

### 6.2. Menulis Deskripsi PR
- Gunakan judul PR yang jelas dan mencerminkan perubahan (misal: `feat: integrasi opsi preset rasio video baru`).
- Jelaskan perubahan apa saja yang dilakukan, motivasi di balik perubahan tersebut, dan cara mengujinya.
- Sertakan referensi ke nomor issue yang diselesaikan, misalnya:
  ```markdown
  Fixes #1
  ```
  atau
  ```markdown
  Closes #12
  ```
- Sertakan tangkapan layar (screenshot) atau rekaman layar jika perubahan Anda melibatkan antarmuka pengguna web.

---

Terima kasih telah meluangkan waktu untuk membantu mengembangkan Clipping Studio! 🎉
