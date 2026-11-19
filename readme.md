# TypoKu : Asisten Penulisan Akademik AI

**TypoKu** adalah platform asisten penulisan berbasis kecerdasan buatan (*Artificial Intelligence/AI*) yang dirancang untuk membantu mahasiswa, dosen, dan akademisi Indonesia dalam menyempurnakan kualitas tulisan. TypoKu menyediakan fitur koreksi ejaan dan salah ketik (*typo*), parafrase kalimat, penerjemahan naskah akademik, serta ekstraksi teks dari dokumen PDF dan DOCX.

Akses fitur AI dikelola melalui sistem langganan berbayar dengan pembayaran **QRIS** (Midtrans Snap).

---

## Fitur Utama

### 1. Koreksi Typo dan Ejaan
Membantu mendeteksi dan memperbaiki kesalahan penulisan, ejaan, serta kata tidak baku berdasarkan kaidah bahasa Indonesia.

- Menerima unggahan file `.pdf` atau `.docx`, lalu mengekstrak teksnya secara otomatis.
- Mengembalikan isi teks yang sudah diperbaiki dengan sorotan kuning pada setiap kata yang diperbaiki.

### 2. Parafrase Cerdas
Membantu menyusun ulang kalimat agar lebih jelas, lugas, formal, atau akademik tanpa mengubah inti makna.

- Mendukung penyesuaian gaya penulisan (misal: gaya akademik).

### 3. Penerjemah Akademik
Membantu menerjemahkan teks akademik dari bahasa Indonesia ke bahasa Inggris maupun sebaliknya.

- Mendukung penerjemahan dua arah (Indonesia–Inggris).
- Mempertahankan konteks dan terminologi akademik.

### 4. Akun & Langganan
- Login dengan email + kata sandi, atau **Google Sign-In**.
- Lupa kata sandi melalui tautan reset yang dikirim ke email.
- Tiga pilihan paket: **Cepat** (1 hari), **Pro** (30 hari), dan **Pro Plus** (90 hari).
- Riwayat transaksi dan status langganan aktif dapat dilihat dari dropdown profil.

### 5. Pembayaran QRIS
- Pembayaran memakai **Midtrans Snap popup** dengan kanal **QRIS** saja.
- Pesanan dibuat di backend dengan harga yang diambil langsung dari database, ditambah biaya admin tetap.
- Batas pembayaran **15 menit** sejak pesanan dibuat.
- Halaman checkout memantau status pembayaran secara otomatis, termasuk saat popup sudah ditutup, lalu mengarahkan ke halaman **berhasil** atau **kedaluwarsa**.

---

## Teknologi yang Digunakan

### Frontend
- **React.js** : membangun antarmuka aplikasi.
- **Vite** : development server dan build tool.
- **React Router DOM** : mengatur navigasi antar halaman.
- **TanStack Query** : pengambilan data, cache, dan sinkronisasi status server.
- **React Icons** : menyediakan ikon antarmuka.
- **Midtrans Snap.js** : popup pembayaran QRIS.
- **Google Identity Services** : login Google.
- **CSS** : mengatur tampilan dan responsivitas aplikasi.

### Backend
- **Node.js & Express 5** : bahasa pemrograman dan framework REST API.
- **MySQL (mysql2)** : basis data dengan connection pool dan dukungan TLS.
- **Google GenAI SDK** : integrasi dengan model AI Gemini (`gemini-3.8-flash`) beserta fallback model.
- **Midtrans Client** : pembuatan transaksi Snap dan pemeriksaan status transaksi.
- **JWT & bcryptjs** : autentikasi Bearer token dan hashing kata sandi.
- **express-validator** : validasi body request.
- **pdf-parse & mammoth** : pustaka ekstraksi teks dari file PDF dan Word (DOCX).
- **pdfkit** : pembuatan berkas hasil unduhan.
- **Multer** : penanganan unggahan file (*file upload*).
- **Helmet, CORS & Morgan** : keamanan HTTP, akses lintas origin, dan logging request.
- **dotenv** : pengelolaan variabel lingkungan.
- **Nodemon** : auto-reload saat pengembangan.

---

## Struktur Direktori

```text
typoku/
├── frontend/
│   ├── public/             # Aset statis dan favicon
│   ├── src/
│   │   ├── assets/         # Gambar dan logo
│   │   ├── components/     # Komponen UI yang dapat digunakan ulang
│   │   ├── context/        # AuthContext (sesi pengguna)
│   │   ├── hooks/          # Hook data & pembayaran (usePaymentPolling, dll.)
│   │   ├── layouts/        # Layout halaman aplikasi
│   │   ├── lib/            # Konfigurasi API & TanStack Query
│   │   ├── pages/          # Halaman aplikasi (Paket, Checkout, PembayaranLunas, ...)
│   │   ├── services/       # Konfigurasi dan layanan API
│   │   └── utils/          # Util Snap.js & pembantu
│   └── package.json
│
├── backend/
│   ├── confiq/             # Konfigurasi koneksi database
│   ├── controllers/        # Logika pemrosesan endpoint
│   ├── jobs/               # Cron expireSubscription & reconcilePayments
│   ├── middleware/         # Middleware authenticate, rateLimiter, checkSubscription
│   ├── routes/             # Definisi rute API
│   ├── utils/              # Util Midtrans & pembantu
│   ├── .env                # Variabel lingkungan (tidak di-commit)
│   ├── .gitignore
│   ├── package.json
│   ├── package-lock.json
│   ├── schema.sql          # Skema database MySQL
│   └── server.js           # Entry point Express
│
├── .gitignore              # Pengecualian file sensitif
└── readme.md
```

---

## Instalasi dan Menjalankan Proyek

### Prasyarat

Pastikan perangkat telah memiliki lingkungan berikut dengan versi yang didukung (disarankan menggunakan patch versi terbaru untuk alasan keamanan):

- **Node.js**: v20.x atau lebih baru (disarankan LTS terbaru)
- **npm**: v10.x atau lebih baru
- **Git**: v2.x atau lebih baru
- **MySQL**: v8.x atau lebih baru (lokal maupun layanan terkelola seperti Aiven)

### 1. Clone Repositori

```bash
git clone https://github.com/intannz/typoku.git
cd typoku
```

### 2. Menjalankan Frontend

```bash
cd frontend
npm install
npm run dev
```

Aplikasi Vite dapat diakses melalui: `http://localhost:5173`

Konfigurasi `frontend/.env`:

```env
VITE_API_BASE_URL=http://localhost:8000
VITE_GOOGLE_CLIENT_ID=xxxxx.apps.googleusercontent.com
```

`VITE_GOOGLE_CLIENT_ID` harus **sama** dengan `GOOGLE_CLIENT_ID` di backend.

Frontend **tidak** menyimpan konfigurasi Midtrans. `client_key` dan `midtrans_is_production` selalu dikirim backend melalui response API.

### 3. Menjalankan Backend

Buka terminal baru pada direktori `backend`:

```bash
cd backend
npm install
```

Siapkan database MySQL lalu jalankan skema:

```bash
mysql -u root -p typoku < schema.sql
```

Konfigurasi file `.env` di folder `backend`:

```env
PORT=8000
NODE_ENV=development
APP_URL=http://localhost:5173

# Database
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=
DB_NAME=typoku
DB_SSL=false

# Autentikasi
JWT_SECRET=ganti_dengan_string_acak_panjang
JWT_EXPIRES_IN=7d
GOOGLE_CLIENT_ID=xxxxx.apps.googleusercontent.com

# AI
GEMINI_API_KEY=GantiDenganApiKeyGeminiKamu

# Midtrans (sandbox untuk pengembangan lokal)
MIDTRANS_SERVER_KEY=SB-Mid-server-xxxxxxxxxxxxxxxx
MIDTRANS_CLIENT_KEY=SB-Mid-client-xxxxxxxxxxxxxxxx
MIDTRANS_IS_PRODUCTION=false
PAYMENT_METHODS=qris
ADMIN_FEE=700
```

Jalankan server Express:

```bash
npm run dev
```

Untuk mode produksi:
```bash
npm start
```

Backend dapat diakses di `http://localhost:8000`.

> **Catatan Midtrans:** pasangan server key dan client key harus dari environment yang sama. Kunci sandbox diawali `SB-Mid-...`, kunci production diawali `Mid-...`. Jangan pernah menaruh server key di frontend atau di repository. Lihat bagian [Konfigurasi Midtrans](#konfigurasi-midtrans) untuk perpindahan ke production.

### 4. Menjalankan dengan Docker (opsional)

Repositori menyertakan konfigurasi Docker untuk menjalankan frontend dan backend sekaligus:

```bash
docker compose up -d --build --force-recreate
```

| Layanan | Alamat |
|---|---|
| Frontend (vite preview) | `http://localhost:4173` |
| Backend (Express) | `http://localhost:8000` |

Karena frontend dibangun sebagai berkas statis, perubahan kode di `frontend/src` **wajib** disertai `docker compose build frontend`; `restart` saja tidak cukup. Nilai `VITE_API_BASE_URL` dan `VITE_GOOGLE_CLIENT_ID` dibakar ke bundle saat build melalui `build.args` di `docker-compose.yml`.

---

## Dokumentasi API

Seluruh endpoint backend mengembalikan respons JSON.

### 1. Cek Typo dan Ejaan
Menerima file dokumen (.pdf / .docx) atau teks mentah. Mengembalikan teks asli dengan sorotan merah coret garis pada kata salah/typo, teks hasil perbaikan dengan sorotan kuning pada kata yang dibetulkan, serta link unduhan dokumen.

- **Endpoint:** `POST /api/cek-typo`
- **Content-Type:** `multipart/form-data` atau `application/json`

| Parameter | Tipe | Keterangan |
|---|---|---|
| `file` | File | File dokumen (.pdf atau .docx) — opsional jika menggunakan `text` |
| `text` | String | Teks langsung yang ingin diperiksa — opsional jika menggunakan `file` |

**Contoh Respons Berhasil:**
```json
{
  "status": "success",
  "filename": "draft_artikel.docx",
  "total_errors": 2,
  "original_text": "saya beli <mark style=\"background-color: #fee2e2; color: #dc2626; text-decoration: line-through; padding: 2px 4px; border-radius: 4px; font-weight: 500;\">apotik</mark> dan <mark style=\"background-color: #fee2e2; color: #dc2626; text-decoration: line-through; padding: 2px 4px; border-radius: 4px; font-weight: 500;\">jadual</mark> di pasar",
  "result_text": "saya beli <mark style=\"background-color: #fef08a; color: #854d0e; padding: 2px 4px; border-radius: 4px; font-weight: 500;\">apotek</mark> dan <mark style=\"background-color: #fef08a; color: #854d0e; padding: 2px 4px; border-radius: 4px; font-weight: 500;\">jadwal</mark> di pasar",
  "clean_result_text": "saya beli apotek dan jadwal di pasar",
  "download_url": "/api/cek-typo/download/8ee017d5-4aec-4413-9d58-f0bb102f563c?format=pdf",
  "download_link": "http://localhost:8000/api/cek-typo/download/8ee017d5-4aec-4413-9d58-f0bb102f563c?format=pdf"
}
```

---

### 2. Parafrase Dokumen / Teks (Blok Kuning)
Memparafrase dokumen atau teks dengan sorotan kuning pada bagian yang diubah dan menyediakan tautan unduh.

- **Endpoint:** `POST /api/cek-typo/parafrase`
- **Content-Type:** `multipart/form-data` atau `application/json`

| Parameter | Tipe | Keterangan |
|---|---|---|
| `file` | File | File dokumen (.pdf atau .docx) — opsional |
| `text` | String | Teks naskah yang ingin diparafrase |
| `style` | String | Gaya bahasa (default: `akademik`) |

---

### 3. Unduh Hasil Pemeriksaan / Parafrase
Mengunduh hasil perbaikan teks dalam format PDF, TXT, atau HTML.

- **Endpoint:** `GET /api/cek-typo/download/:id?format=pdf` (atau `format=txt`)
- **Content-Type:** `application/pdf` / `text/plain`

---

### 4. Parafrase Cerdas
Menyusun ulang kalimat agar lebih sesuai dengan gaya bahasa akademik.

- **Endpoint:** `POST /api/parafrase`
- **Content-Type:** `application/json`

**Contoh Request Body:**
```json
{
  "text": "kiyoung suka makan salmon dari danau cermin",
  "style": "akademik"
}
```

**Contoh Respons Berhasil:**
```json
{
  "status": "success",
  "result_text": "Kiyoung memiliki preferensi konsumsi ikan salmon yang berasal dari kawasan Danau Cermin."
}
```

---

### 5. Penerjemah Akademik
Menerjemahkan teks akademik dua arah (Indonesia–Inggris).

- **Endpoint:** `POST /api/terjemah`
- **Content-Type:** `application/json`

**Contoh Request Body:**
```json
{
  "text": "mie adalah makanan favoritku",
  "target_lang": "en"
}
```

**Contoh Respons Berhasil:**
```json
{
  "status": "success",
  "source_language": "id",
  "target_language": "en",
  "result_text": "Noodles are my favorite food."
}
```

---

### 6. Paket & Langganan

- **GET `/api/packages`** — daftar paket aktif (publik).
- **GET `/api/subscriptions/my-access`** — status akses langganan pengguna.
- **GET `/api/subscriptions/my-history`** — riwayat langganan.

### 7. Pembayaran

- **POST `/api/orders`** — membuat pesanan dan Snap token. Body: `packageId`, `paymentMethod` (`qris`).
- **GET `/api/orders/:orderCode`** — detail pesanan beserta `can_reopen`, `snap_token`, dan `client_key` selama masih `pending`.
- **GET `/api/orders/my`** — riwayat transaksi pengguna.
- **GET `/api/payments/config`** — `{ admin_fee, payment_methods, midtrans_is_production }` (publik, tanpa rahasia).
- **GET `/api/payments/status/:orderCode`** — status terkini untuk polling frontend.
- **POST `/api/payments/notification`** — webhook Midtrans (dipanggil server Midtrans, bukan frontend).

Status pesanan: `pending`, `paid`, `expired`, `cancelled`, `failed`.

---

## Konfigurasi Midtrans

TypoKu hanya memakai kanal **QRIS**. Alurnya:

1. Frontend meminta pembuatan pesanan ke `POST /api/orders`.
2. Backend membuat transaksi Snap dengan `enabled_payments: ["other_qris"]` dan masa berlaku 15 menit.
3. Frontend memuat `snap.js` lalu menampilkan popup QRIS.
4. Setelah QR dipindai, Midtrans mengirim webhook yang mengubah status pesanan menjadi `paid` di database.
5. Frontend membaca status melalui `GET /api/payments/status/:orderCode` setiap 5 detik, lalu mengarahkan pengguna ke halaman berhasil.

Pemantauan status **tetap berjalan setelah popup ditutup**, karena QRIS biasanya dipindai dari ponsel. Jika pengguna menutup tab lalu kembali, halaman checkout memeriksa ulang status pesanan sehingga pesanan yang sudah lunas langsung diarahkan ke halaman berhasil.

### Pindah dari Sandbox ke Production

| Langkah | Keterangan |
|---|---|
| 1. Buat kunci production | Dashboard Midtrans → Settings → Access Keys → General Credentials |
| 2. Aktifkan kanal QRIS | Settings → Payment Methods → GoPay Dynamic QRIS → sampai berstatus **Active** |
| 3. Isi rekening pencairan | Settings → Withdrawal |
| 4. Daftarkan Notification URL | `https://<domain-backend>/api/payments/notification` |
| 5. Isi Finish Redirect URL | `https://<domain-frontend>/paket` |
| 6. Ubah environment backend | `MIDTRANS_SERVER_KEY`, `MIDTRANS_CLIENT_KEY`, `MIDTRANS_IS_PRODUCTION=true`, `PAYMENT_METHODS=qris` |

Frontend **tidak perlu diubah** dan tidak perlu di-rebuild: `client_key` serta `midtrans_is_production` datang dari API. Namun cache TanStack Query dipersist ke `localStorage`, sehingga pengguna lama sebaiknya logout lalu login (atau memakai jendela incognito) agar konfigurasi sandbox lama tidak terpakai.

Verifikasi di DevTools: `snap.js` harus dimuat dari `app.midtrans.com`, dan `data-client-key` pada elemen `#midtrans-snap-script` harus diawali `Mid-client-`.

---

### Batasan Penggunaan & Catatan Integrasi
1. **Batasan Kata (Word Limit):** Disarankan agar *frontend* membatasi input maksimal **1.000 – 2.000 kata** per pengiriman API demi menjaga waktu respons AI tetap di bawah 10 detik.
2. **Ukuran File Dokumen:** Maksimal **5 MB** untuk format `.pdf` dan `.docx`.
3. **Akses Fitur AI:** Seluruh endpoint AI memerlukan autentikasi Bearer JWT **dan** langganan aktif. Tanpa login menghasilkan `401`; tanpa langganan aktif menghasilkan `403`.
4. **Alur UI:**
   - User mengunggah file => Frontend memanggil `/api/cek-typo` (file dikirim langsung).
   - Backend mengekstrak teks, memperbaiki typo, dan mengembalikan `result_text` berisi HTML dengan sorotan kuning.
   - Untuk Parafrase / Terjemah, frontend mengirim teks JSON ke endpoint yang sesuai.
5. **Batas Waktu Pesanan:** Setiap pesanan berlaku **15 menit**. Pesanan yang kedaluwarsa tidak dapat dibayar dan pengguna harus membuat pesanan baru.

---

## Pengembang

Proyek ini dikembangkan sebagai bagian dari tugas Digital Entrepreneurship.

---

*TypoKu : Cek ejaan, pahami perbaikannya.*