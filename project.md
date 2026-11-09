# PROJECT: Web Safelink Bypass - Auto Extract Direct Download Link

## 0. BRANDING - NAMA & LOGO
- **Nama Web: HANBY-PASS**
- **Tagline: "Tempel. Lewatin. Download."**
- **Logo: Text-based doang, JANGAN pakai logo AI / logo gradient / logo robot**
    - Logo di header: `[ HANBY-PASS_ ]` dengan font `JetBrains Mono` / `Space Grotesk` tebal, warna putih
    - Huruf `HANBY-PASS` warna putih #FFFFFF, underscore `_` warna biru #2563EB dan berkedip (blinking cursor kaya terminal)
    - Di sebelah kanan logo, ada label kecil abu-abu: `v1.0 - SFL BYPASS TOOL`
    - Favicon: Kotak biru tua #0A1A33 dengan huruf `H` putih tebal di tengah, jangan pakai emoji
- **Cara manggil di web:** Jangan pakai nama "SFL Bypass Pro" / "Safelink Extractor AI". Selalu pakai "HANBY-PASS".
- **Footer:** `HANBY-PASS © 2026 - Dibikin buat lewatin iklan yang ngeselin.`

## 1. TUJUAN PROJECT
Buat web yang fungsinya cuma 1: User tempel link safelink (yang penuh iklan), web akan mengekstrak dan menampilkan link download aslinya secara langsung, tanpa user harus melewati iklan, timer, dan tombol continue berkali-kali.

Contoh Input: `https://sfl.gl/xxxxx` / `https://safelink.contoh/go?url=xxx`
Contoh Output: `https://www.mediafire.com/file/xxx/file.zip` / `https://drive.google.com/file/xxx`

## 2. TECH STACK YANG HARUS DIPAKAI
- Frontend: HTML + Tailwind CSS + Vanilla JavaScript (Single Page, tanpa framework berat)
- Backend: Node.js + Express.js
- Library Backend: axios, cheerio, qs
- Deployment: Bisa jalan di Vercel / Railway / VPS

## 3. STRUKTUR FOLDER

### 3.1. Struktur Folder
/HANBY-PASS
├── /public
│   ├── http://index.html          -> Halaman utama (struktur)
│   ├── http://style.css           -> Semua style custom (animasi, shimmer, dll)
│   ├── http://script.js           -> Logic frontend (fetch API, copy, history)
│   └── /assets
│       ├── http://favicon.ico     -> Logo H biru tua
│       └── http://logo.svg        -> Logo [ HANBY-PASS_ ] versi SVG
├── http://server.js               -> Backend utama (Express + bypass logic)
├── /utils
│   ├── http://bypass.js           -> Fungsi utama bypass (fetch, parse, loop)
│   ├── http://extractors.js       -> Kumpulan extractor per jenis safelink
│   └── http://helpers.js          -> Fungsi bantu (decode base64, validasi URL)
├── http://package.json            -> Daftar dependency
├── .env                    -> Config (PORT, dll)
└── http://README.md               -> Cara jalanin project

3.2. Struktur Halaman (index.html dari atas ke bawah)
1. <header> - TOP BAR
   - Kiri: Logo [ HANBY-PASS_ ] + blinking cursor
   - Kanan: Label [ STATUS: READY ] + v1.0

2. <div> - MARQUEE / RUNNING TEXT
   - Text jalan: BYPASS IKLAN • LANGSUNG DOWNLOAD • TANPA TIMER •

3. <main> - CONTAINER UTAMA (max-width 720px, tengah)
   
   3a. HERO SECTION
       - Label kecil: [ TOOL #01 - SFL BYPASS ]
       - H1: LEWATIN IKLAN. LANGSUNG DOWNLOAD.
       - P: Tempel link sfl lu, gw lewatin iklannya.

   3b. INPUT SECTION (CARD #01 - INPUT CARD)
       - Label: PASTE LINK SFL LU DISINI
       - Input: [ paste link sfl disini... ]
       - Button: [ LEWATIN -> ]
       - Text kecil bawah: Contoh: https://sfl.gl/abc123

   3c. LOADING SECTION (HIDDEN, MUNCUL PAS KLIK)
       - Skeleton Card #1: Bar judul
       - Skeleton Card #2: Bar link
       - Skeleton Card #3: Bar tombol
       - Semua pakai efek shimmer

   3d. RESULT SECTION (HIDDEN, MUNCUL PAS BERHASIL)
       - Card #02 - RESULT CARD
         - Header: HASIL BYPASS + Badge domain (mediafire.com)
         - Box Link: Link asli + Tombol COPY
         - Button: [ DOWNLOAD LANGSUNG ]
         - Button: [ BYPASS LAGI ]

   3e. ERROR SECTION (HIDDEN, MUNCUL PAS GAGAL)
       - Card Error: Gagal bypass, safelink ini memakai captcha...

   3f. HISTORY SECTION
       - Judul: RIWAYAT LU
       - Grid: 1 kolom di HP, 2 kolom di Desktop
       - Card History (x5): Domain + Link pendek + Tombol COPY + Waktu

4. <footer> - FOOTER
   - Kiri: HANBY-PASS © 2026
   - Kanan: No ads. No tracking. Paste - Extract - Download.

3.3. Struktur Backend (server.js)
http://server.js
├── Import: express, cors, axios, cheerio, http://bypass.js
├── Middleware: cors(), http://express.json()
├── Route: GET / -> kirim http://index.html
├── Route: GET /api/bypass?url=...
│   ├── 1. Validasi URL
│   ├── 2. Panggil http://bypass.js
│   ├── 3. Return JSON { success, direct, domain }
│   └── 4. Handle error
└── Listen PORT 3000

3.4. Struktur Fungsi Bypass (utils/bypass.js)
bypass(url)
├── fetchPage(url) -> ambil HTML
├── findLink(html) -> cari di <a>, <meta>, <script>
├── decodeLink(link) -> decode base64 / atob
├── if masih safelink -> loop lagi (max 5x)
└── return directLink


## 4. FITUR FRONTEND (index.html)
- Tampilan dark mode, mobile friendly
- Header: Logo `[ HANBY-PASS_ ]` di kiri, status `READY` warna abu-abu di kanan
- Hero: Judul besar rata kiri: `LEWATIN IKLAN. LANGSUNG DOWNLOAD.` (2 baris, uppercase, putih)
- Sub-judul abu-abu: `Tempel link sfl lu, gw lewatin iklannya.`
- Ada 1 input besar untuk tempel link safelink
- Ada tombol [EXTRACT / BYPASS] warna mencolok, teksnya: `LEWATIN ->`
- Ada area loading: "Sedang mengekstrak link..."
- Setelah berhasil:
    - Tampilkan link asli di box yang bisa di-copy
    - Tampilkan tombol [DOWNLOAD LANGSUNG] yang mengarah ke link asli
    - Tampilkan domain asli (misal: mediafire.com, drive.google.com)
- Setelah gagal:
    - Tampilkan pesan error: "Gagal bypass, safelink ini memakai captcha / proteksi baru"
- Ada history: Simpan 5 link terakhir yang berhasil di-bypass di LocalStorage, judul sectionnya: `RIWAYAT LU`
- Tanpa iklan, tanpa popup

## 5. LOGIKA BACKEND (server.js)
Buat 1 endpoint utama:
`GET /api/bypass?url=LINK_SAFELINK`

Langkah backend:
1. Validasi URL, harus http / https
2. Fetch URL safelink dengan header User-Agent Mozilla/5.0 agar tidak dianggap bot
3. Gunakan cheerio untuk parsing HTML
4. Cari link asli dengan strategi berurutan:
   a. Cari di tag `<a id="download">`, `<a class="btn-download">`, `<a id="btn-continue">`
   b. Cari di tag `<meta http-equiv="refresh">`
   c. Cari di dalam `<script>` yang berisi `window.location`, `base64`, `atob`, `decodeURIComponent`
   d. Cari form POST yang berisi token / input hidden, lalu lakukan POST kedua untuk dapat link
5. Jika link yang ditemukan masih berupa link safelink lagi (redirect berantai), lakukan looping fetch maksimal 5x sampai ketemu link final (mediafire, google drive, mega, zippyshare, dll)
6. Return JSON: `{ success: true, original: "link_awal", direct: "link_asli", domain: "mediafire.com" }`
7. Jika gagal setelah 5x, return `{ success: false, message: "Gagal..." }`
8. Tambahkan handling untuk CORS: `app.use(cors())`

## 6. KEAMANAN & ANTI-BLOCK
- Tambahkan delay 500ms - 1000ms antar request agar tidak di-ban IP
- Gunakan header Referer dari URL safelink itu sendiri
- Jangan simpan log link user di database, hanya di LocalStorage di sisi client
- Tambahkan rate-limit: Maksimal 20 request per menit per IP

## 7. WARNA - REQUEST USER (WAJIB IKUTIN INI, JANGAN GANTI)
HANYA PAKAI 3 WARNA INI. JANGAN TAMBAH WARNA LAIN SEPERTI UNGU, PINK, HIJAU NEON, ORANGE.

- **BIRU TUA (Warna Utama / Primary):**
    - Background utama: `#0A1A33` / `#0B1D3A` (biru tua pekat, bukan hitam)
    - Card: `#12264A` (biru tua sedikit lebih terang)
    - Border card: `#1E3A5F` (biru tua medium)
    - Tombol utama EXTRACT / DOWNLOAD: `#1D4ED8` / `#2563EB` (biru tua terang, SOLID, bukan gradient)
    - Tombol hover: `#1E40AF` (lebih gelap dikit)
- **PUTIH (Warna Teks / Accent Terang):**
    - Judul & teks utama: `#FFFFFF`
    - Teks tombol utama: `#FFFFFF`
    - Garis pemisah / label kecil: `#FFFFFF` dengan opacity 60%
- **ABU-ABU (Warna Sekunder / Support):**
    - Teks sekunder / deskripsi: `#94A3B8` (abu slate)
    - Placeholder input: `#64748B`
    - Background input: `#0F2447` (biru tua + abu gelap)
    - Border input: `#334155` (abu gelap)
    - Background skeleton loading: `#1E293B` (abu gelap)
    - Background history card: `#13294B` (campuran biru tua + abu)

ATURAN WARNA:
- DILARANG gradient (apapun, termasuk biru ke biru muda)
- DILARANG warna ungu, pink, hijau neon, kuning
- Semua warna harus SOLID / FLAT
- Rasio: 60% Biru Tua, 25% Putih (teks), 15% Abu (detail sekunder)

## 8. TAMPILAN - WAJIB TIDAK KAYA AI / TIDAK GENERIC
INI PALING PENTING. JANGAN BUAT TAMPILAN SEPERTI HASIL GENERATE AI PADA UMUMNYA.

### 8.1. YANG DILARANG KERAS (Ciri Khas Web AI):
- DILARANG gradient ungu - biru - pink di background / tombol / teks
- DILARANG glassmorphism / kaca-kaca blur berlebihan di semua card
- DILARANG semua card rounded-2xl / rounded-3xl + shadow-soft yang sama semua
- DILARANG pakai emoji berlebihan sebagai icon, apalagi 🚀 ✨ 🔗 di judul
- DILARANG font Inter + Poppins doang yang dipakai semua web AI
- DILARANG hero section besar dengan teks "Unlock the Power..." / "Supercharge Your..."
- DILARANG teks tengah semua, layout yang terlalu simetris dan bersih kaya template
- DILARANG badge "AI-Powered" / "100% Free" / "No Sign Up Required" yang nempel dimana-mana
- DILARANG pakai warna gradient untuk teks judul

### 8.2. STYLE YANG HARUS DIPAKAI - KAYA DIBIKIN MANUSIA / INDIE HACKER:
- Gaya: Brutalist + Utility, kaya webnya anak forum / tools downloader, tapi versi modern & rapi
- Warna: Ikutin palet di Section 7 (Biru Tua, Putih, Abu). Background #0A1A33, Card #12264A dengan border 1px solid #1E3A5F (border tegas, bukan shadow lembut)
- Font: Judul pakai font tebal yang karakter: `Space Grotesk` / `Archivo Black` / `JetBrains Mono` untuk angka / link. Body pakai `Space Grotesk`. Jangan pakai Inter.
- Layout: Rata kiri (text-left), bukan semua rata tengah. Judul besar, kasar, uppercase, tracking-tight. Ada garis pemisah, ada nomor, ada label kecil kaya `[ TOOL #01 ]`, `[ STATUS: READY ]` dengan warna abu-abu
- Detail manusiawi: Tambahkan di pojok: "Buat lewatin iklan yang ngeselin", footer kecil: "No ads. No tracking. Paste - Extract - Download." warna abu-abu
- Input: Kotak tajam, border 2px #334155, background #0F2447, placeholder kaya: `paste link sfl disini...` (huruf kecil, santai, bukan "Enter your URL here...")
- Tombol: Kotak, tidak terlalu rounded (rounded-md / rounded-lg aja), background biru solid #2563EB, teks putih uppercase tebal, ada border bawah yang lebih tebal kaya tombol arcade (box-shadow: 0px 4px 0px #0A1A33)
- Tambahkan elemen kasar: garis putus-putus abu-abu, dot pattern putih opacity 5% di background, atau marquee text jalan kecil di atas: `BYPASS IKLAN • LANGSUNG DOWNLOAD • TANPA TIMER •` warna abu-abu
- Icon: Pakai SVG line icon / Lucide yang tipis warna putih / abu, jangan emoji

Tujuannya: Pas orang buka, langsung kerasa "oh ini tools buatan orang, bukan template AI".

## 9. CONTOH FLOW USER
1. User buka web HANBY-PASS
2. Tempel: `https://sfl.gl/abc123`
3. Klik `LEWATIN ->`
4. Loading 2-3 detik
5. Muncul: `Direct Link: https://mediafire.com/...`
6. User klik DOWNLOAD LANGSUNG

## 10. UI / UX TAMBAHAN - WAJIB ADA

### 10.1. Button Animation - Pas Di Pencet
- Semua tombol utama (LEWATIN, DOWNLOAD LANGSUNG, COPY LINK) harus punya animasi saat ditekan:
    - Efek `active:scale-95` / `transform: scale(0.95)` saat ditekan
    - Efek ripple / gelombang kecil dari titik klik (warna putih opacity 20%)
    - Transisi `transition-all duration-150 ease-out`
    - Saat loading, tombol berubah jadi disabled + ada spinner berputar di dalam tombol + teks berubah jadi "Memproses..." (warna abu-abu)
    - Setelah berhasil, tombol berkedip putih sebentar (success pulse)
- Tombol LEWATIN: Warna solid biru #2563EB, teks putih, saat hover jadi #1D4ED8, saat ditekan mental ke dalam (pressed effect) kaya tombol arcade
- Jangan buat tombol yang kaku / langsung ganti warna tanpa transisi

### 10.2. Card Hover - Pas Di Pencet Ada Animasi
- Card hasil (card yang nampilin link asli) dan card history harus interaktif:
    - Saat di-hover (desktop): `transform: translateY(-4px)`, border berubah jadi putih / biru terang #2563EB, bukan shadow lembut
    - Saat di-pencet / di-tap (mobile): `transform: scale(0.97)` + efek bounce balik ke 1.0
    - Transisi: `transition: all 0.2s ease-in-out`
    - Card history yang diklik harus ada animasi copy: icon berubah jadi checklist + teks "Tersalin!" selama 1.5 detik
- Card harus terasa hidup, bukan kotak diam

### 10.3. Skeleton Shimmer - Loading
- Jangan pakai loading teks polos atau spinner doang
- Saat user klik LEWATIN dan menunggu backend, tampilkan Skeleton Card:
    - Buat 2-3 bar skeleton berbentuk kotak (bukan rounded-full), warna abu gelap #1E293B
    - Tambahkan efek Shimmer: gradient putih opacity kecil yang jalan dari kiri ke kanan terus-menerus (animation: shimmer 1.5s infinite) - shimmernya putih / abu terang, bukan warna lain
    - CSS contoh: `background: linear-gradient(90deg, #1E293B 25%, #334155 50%, #1E293B 75%); background-size: 200% 100%; animation: shimmer 1.2s infinite`
    - Skeleton untuk: Judul link, box link asli, tombol download
- Setelah data datang, skeleton hilang dengan fade-out, lalu card asli muncul dengan fade-in + slide-up

## 11. RESPONSIVE - SESUAIKAN UKURAN LAYAR
- Web HARUS 100% responsive, enak di semua ukuran:
    - **HP (320px - 480px):** Layout 1 kolom, input + tombol jadi full width dan bertumpuk ke bawah, font judul lebih kecil (text-2xl), card history jadi 1 kolom, padding lebih kecil (p-4), tombol besar gampang di-tap (min-height 48px), logo `[ HANBY-PASS_ ]` tetap kecil di atas
    - **Tablet (481px - 768px):** Layout tetap 1 kolom tapi max-width 600px di tengah, font sedang
    - **Laptop / Desktop (769px ke atas):** Layout di tengah max-width 720px, input + tombol sejajar (input flex-1 + tombol di kanan), card bisa 2 kolom untuk history
- Gunakan Tailwind responsive: `w-full sm:w-auto`, `flex-col sm:flex-row`, `text-xl sm:text-2xl md:text-3xl`, `p-4 sm:p-6 md:p-8`, `grid-cols-1 sm:grid-cols-2`
- Pastikan tidak ada elemen yang overflow / kepotong di HP
- Input tidak boleh ke-zoom saat di-tap di iPhone: set `font-size: 16px` minimal untuk input
- Tombol dan card harus tetap gampang di-tap dengan jempol, jangan terlalu kecil
- Test di: iPhone SE (375px), Samsung (360px), iPad (768px), Laptop (1366px)

## 12. TUGAS CLAUDE
- Buatkan file `server.js`, `public/index.html`, `public/style.css`, `public/script.js`, `package.json` yang lengkap dan bisa langsung dijalankan dengan `npm install && npm start`
- Buat kode yang modular, beri komentar di setiap fungsi bypass agar mudah ditambah jenis safelink baru
- Pastikan frontend dan backend sudah terhubung dengan benar
- Pastikan semua animasi (Button Animation, Card Hover, Skeleton Shimmer) dan Responsive sudah jalan di semua ukuran layar
- PENTING: Cek lagi hasil tampilannya, kalau masih keliatan kaya template AI (gradient ungu, glassmorphism, rounded gede semua), revisi sampai keliatan kaya tools indie / brutalist buatan manusia
- PENTING WARNA: Hanya pakai Biru Tua, Putih, Abu sesuai Section 7. Kalau ada warna lain, revisi.
- PENTING BRANDING: Pakai nama HANBY-PASS di semua tempat, logo `[ HANBY-PASS_ ]` dengan underscore biru berkedip.


## 13. FONT
1.0 FONT - WAJIB PAKAI INI (JANGAN PAKAI INTER / POPPINS)

1.0. Font Yang Dipakai (Hanya 3 Font Ini)
- **FONT #1 - JUDUL / LOGO / H1: `Archivo Black`**
    - Buat: Logo `[ HANBY-PASS_ ]`, H1 `LEWATIN IKLAN. LANGSUNG DOWNLOAD.`, Judul Section `RIWAYAT LU`
    - Karakter: Tebal, kasar, padat, kaya poster. Bukan font tipis kaya AI.
    - Weight: 400 (Archivo Black cuma ada 1 weight)
    - Style: UPPERCASE semua, tracking-tight (-0.02em), line-height 1.0

- **FONT #2 - BODY / TEKS / TOMBOL / INPUT: `Space Grotesk`**
    - Buat: Semua teks biasa, deskripsi, placeholder, tombol `LEWATIN ->`, tombol `DOWNLOAD`, label kecil
    - Karakter: Modern tapi ada karakter, gak ngebosenin kaya Inter
    - Weight yang dipakai:
        - Regular 400 -> untuk deskripsi / teks abu-abu
        - Medium 500 -> untuk input / link
        - Bold 700 -> untuk tombol / label
    - Style: Kalau tombol, UPPERCASE + Bold 700 + letter-spacing 0.05em

- **FONT #3 - LINK / KODE / ANGKA / STATUS: `JetBrains Mono`**
    - Buat: Link hasil `https://mediafire.com/...`, label `[ STATUS: READY ]`, `[ TOOL #01 ]`, angka, waktu history
    - Karakter: Monospace kaya terminal / hacker, bikin kerasa tools beneran
    - Weight: 400 untuk link, 700 untuk label status
    - Style: Jangan di-uppercase kalau link, biarin lowercase kaya link asli

2.0 Cara Import Font (Wajib di index.html)
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo+Black&family=JetBrains+Mono:wght@400;700&family=Space+Grotesk:wght@400;500;700&display=swap" rel="stylesheet">
3.0 Setting Font di CSS (style.css)
:root {
  --font-judul: 'Archivo Black', sans-serif;
  --font-body: 'Space Grotesk', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
}

body { font-family: var(--font-body); }
h1, .logo, .judul-section { font-family: var(--font-judul); }
.link-hasil, .label-status, .waktu-history { font-family: var(--font-mono); }
4.0 Ukuran Font (Responsive)
- *Logo `[ HANBY-PASS_ ]`:* 20px di HP, 24px di Desktop
- *H1 `LEWATIN IKLAN...`:* 32px di HP (text-3xl), 48px di Tablet, 56px di Desktop (text-5xl)
- *Deskripsi:* 14px di HP, 16px di Desktop
- *Input:* 16px WAJIB (biar gak ke-zoom di iPhone)
- *Tombol `LEWATIN ->`:* 16px Bold, UPPERCASE
- *Link Hasil:* 13px di HP, 14px di Desktop (Mono, biar muat)
- *Label Kecil `[ TOOL #01 ]`:* 11px Mono, letter-spacing 0.15em, warna abu-abu #94A3B8
- *History Card:* Judul 14px Bold, Link 12px Mono

5.0 Yang DILARANG Soal Font
- DILARANG pakai Inter, Poppins, Roboto, Arial
- DILARANG semua teks pakai 1 font doang
- DILARANG H1 pakai font tipis / Light (300)
- DILARANG pakai font gradient / font warna-warni
- DILARANG line-height terlalu renggang kaya template AI (1.8 - 2.0). Pakai 1.2 untuk judul, 1.5 untuk body.

 Jadi sekarang fontnya:
**Judul = Archivo Black (tebel kasar)**
**Body = Space Grotesk**
**Link / Kode = JetBrains Mono (kaya terminal)**
