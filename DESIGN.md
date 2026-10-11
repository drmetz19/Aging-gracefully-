# DESIGN.md — Pola Web-Book "Gracefully Aging Series"

Acuan desain untuk semua versi web ebook dr. Metz. Polanya diambil dari **Body Type Mapping** (body-type-mapping.vercel.app).
Ebook berikutnya (Buku 1 Aging Gracefully, Deep Sleep, dst.) **wajib ikut aturan ini**. Yang boleh berubah cuma isi, gambar, dan aksen warna seri.

---

## 1. Prinsip

1. **Isi buku tidak dikarang ulang.** Teks dipindah apa adanya dari PDF. Yang dirapikan hanya struktur (judul, list, paragraf) dan typo ekstraksi.
2. **Satu kolom baca, nyaman di HP.** Lebar teks maks `40rem` (±65 karakter per baris). HP adalah layar utama, desktop menyesuaikan.
3. **Krem + cokelat, serif untuk judul.** Ini identitas seri dan sama dengan PDF Canva aslinya.
4. **Jualan menyatu dengan isi.** CTA ditaruh di titik yang nyambung dengan isi bab, bukan banner acak.
5. **Ringan.** Halaman awal maksimal ±250 KB. Gambar dipisah dan di-lazy-load.
6. **Selalu ada disclaimer kesehatan** di awal dan di footer.

---

## 2. Warna (design tokens)

Semua warna dipakai lewat token. Jangan menulis hex langsung di komponen.

| Token | Terang | Gelap | Fungsi |
|---|---|---|---|
| `--paper` | `#F6EFE4` | `#1E1813` | background halaman |
| `--paper-2` | `#EFE4D3` | `#28201A` | latar sekunder, thead, pilihan aktif |
| `--card` | `#FBF7F0` | `#251E18` | kartu (CTA, tool, galeri) |
| `--ink` | `#2E241B` | `#EDE3D5` | teks utama |
| `--ink-soft` | `#6A5949` | `#B8A894` | caption, keterangan |
| `--bark` | `#6B4A2E` | `#D2A877` | aksen utama: tombol, link, label |
| `--bark-deep` | `#4A3220` | `#E6C59C` | judul, kotak penawaran utama |
| `--sand` | `#C9A77C` | `#8F7150` | nomor bab besar, garis kutipan |
| `--line` | `#E0D2BD` | `#3A2F25` | border tipis |
| `--on-bark` | `#FBF5EC` | `#1E1813` | teks di atas `--bark` |

**Warna body type.** Dipakai hanya untuk penanda tipe, tidak untuk dekorasi umum.

| Tipe | Token | Hex |
|---|---|---|
| Red Oval | `--red` | `#C9554E` |
| Green Square | `--green` | `#5E9A4A` |
| Yellow Triangle / Rectangle | `--yellow` | `#E2B53D` |
| White Circle | `--white` | `#8C8F94` (chip diberi border) |

Mode gelap otomatis ikut `prefers-color-scheme`. Kontras teks minimal WCAG AA.

---

## 3. Tipografi

| Peran | Font | Pemakaian |
|---|---|---|
| Display | **DM Serif Display** (regular + italic) | judul hero, judul bab, H2, judul CTA/offer, kutipan |
| Body / UI | **Poppins** (400, 500, 600, italic) | paragraf, list, tombol, label |

Fallback: `Georgia, serif` untuk display dan `system-ui, sans-serif` untuk body. Font diambil dari Google Fonts.

**Skala** (rem, body 17px):

| Elemen | Ukuran | Gaya |
|---|---|---|
| Hero H1 | `clamp(2.2, 7vw, 3.2)` | display, kata kedua *italic* di baris baru |
| Nomor bab | `5rem` | display, warna `--sand` |
| Judul bab (H1) | `clamp(1.9, 6vw, 2.6)` | display, `--bark-deep` |
| H2 seksi | `clamp(1.45, 4.6vw, 1.8)` | display, `--bark-deep` |
| H3 sub-judul | `1.08` | Poppins 600; nomor "01." pakai display warna `--bark` |
| H4 label | `0.78` | Poppins 600, UPPERCASE, letter-spacing `.12em`, `--bark` |
| Body | `1.0625` | line-height `1.75` |
| Kutipan | `1.3` | display *italic*, garis kiri 3px `--sand` |
| Caption | `0.82` | italic, `--ink-soft` |

Aturan: judul pakai `text-wrap: balance`. Teks tidak boleh justify. Maksimal 2 keluarga font.

---

## 4. Layout

```
HP (<1100px)                      Desktop (≥1100px)
┌──────────────────────┐          ┌────────┬──────────────────────┐
│ ☰ Daftar Isi │ Bab x │ CekTipe│  │ Daftar │   kolom baca 40rem   │
├──────────────────────┤          │  Isi   │   (center)           │
│  kolom baca          │          │ (fixed │                      │
│  padding 16px        │          │ 19rem) │                      │
└──────────────────────┘          └────────┴──────────────────────┘
```

- **Top bar sticky.** Isinya tombol Daftar Isi, nama bab yang sedang dibaca, tombol aksen "Cek Tipe", dan progress bar 3px warna `--bark`.
- **Daftar isi.** Di HP berupa laci dari kiri dengan scrim. Di desktop jadi sidebar tetap. Berisi bab, sub-seksi H2, dan pintasan ke Kuis, Tracker, dan Program.
- **Posisi baca diingat** (localStorage), jadi tombol hero berubah jadi "Lanjutkan Membaca".
- Gutter samping minimal 16px. Tidak ada scroll horizontal, kecuali tabel di dalam wadahnya sendiri.

---

## 5. Urutan halaman

1. **Hero:** cover buku, nama seri, judul, subjudul, 3 tombol (Mulai Membaca · Cek Body Type · Tracker 30 Hari)
2. **Bar "Mulai Dari Sini"** (cokelat)
3. **Disclaimer kesehatan** (kotak garis putus)
4. **Pengantar:** kata pengantar dan testimoni
5. **Bab 1 … n:** tiap bab dibuka dengan nomor besar, label "BAB n", dan judul
6. **Bonus**
7. **Daftar Pustaka**, lalu **Terima Kasih**
8. **Rak "Lanjutkan Perjalananmu":** semua produk
9. **Tentang Penulis**
10. **Footer:** disclaimer dan hak cipta ("untuk pembeli, mohon tidak disebarluaskan")

---

## 6. Komponen

| Komponen | Kelas | Aturan |
|---|---|---|
| Kepala bab | `.ch-head` | garis atas tipis, nomor `--sand`, kicker "BAB n", judul display |
| Kutipan | `blockquote` | display italic, garis kiri `--sand`. Untuk kalimat penekanan dari buku |
| Gambar isi | `figure.img` | radius 6px. `.spot` maks 9rem untuk ilustrasi kecil |
| Foto berpasangan | `.pair` | 2 kolom, rasio 3:4, crop dari atas (wajah + badan) |
| Foto tunggal | `.photo` | maks 18rem, di tengah, dengan caption |
| Foto kecil float | `.thumb` | 7.5rem, float kanan, untuk kartu tipe di bab bonus |
| Galeri profil | `.gal` | kartu dengan chip warna tipe, baris geser (scroll-snap) |
| Sekilas tipe | `.glance` | grid 2 kolom (HP) / 4 kolom (desktop). Foto 3:4, garis atas warna tipe |
| Tabel perbandingan | `.cmp` | wadah `overflow-x:auto`, kolom pertama sticky, tiap kolom tipe diberi strip warna |
| CTA biasa | `.cta` | kartu `--card`. Isi: label kecil, judul display, 1–2 kalimat, tombol pill |
| CTA mini | `.cta.mini` | versi pendek untuk sambungan antar bagian |
| Penawaran utama | `.offer` | latar `--bark-deep`, badge, list centang, harga besar, tombol penuh. **Hanya satu kali per buku** |
| Alat interaktif | `.tool` | kartu radius 18px, label "ALAT BANTU" |
| Kartu kombinasi | `.combo` | `<details>` buka/tutup, dua titik warna (dominan besar, sekunder kecil) |
| Rak produk | `.shelf` | grid kartu link, yang disorot (paket utama) berlatar `--bark` |
| Lightbox | `.lb` | gambar `button.zoom` bisa diketuk untuk diperbesar, tutup dengan Esc atau ketuk luar |
| Tombol | `.btn` / `.btn-lg` / `.btn-ghost` | pill radius 999px. Utama `--bark`, ghost pakai border `--line` |

Card styling (border + radius) **hanya** untuk komponen di atas. Paragraf isi tidak pernah dibungkus kartu.

---

## 7. Gambar

| Jenis | Ukuran export | Format | Kualitas |
|---|---|---|---|
| Ilustrasi dari PDF | maks 720px | webp (alpha dipertahankan) | 72 |
| Kartu / infografis | 1100–1536px | webp | 82 |
| Foto badan | 720px | webp | 76 |
| Foto wajah | 600px | webp | 76 |

- Semua `<img>` diberi `loading="lazy"` dan `alt` deskriptif.
- Ilustrasi PDF yang pecah atau rusak diganti foto atau infografis seri.
- Foto body type: latar krem polos, pakaian dalam hitam atau krem, pose depan. Konsisten antar tipe.
- Tidak ada before–after untuk klaim hasil.

---

## 8. Peta CTA (funnel)

| Posisi | CTA | Tujuan |
|---|---|---|
| Atas (setelah hero) | 📚 Mulai Dari Sini | aginggracefully.site/mulaidarisini |
| Sebelum bab teori → bab inti | Buku 1 Aging Gracefully (dengan cover) | 10tahunlebihmuda.online |
| Setelah kuis | Beauty Tracker **gratis** | aginggracefully.online |
| Setelah bab tipe tubuh | Webinar 12x | /webinaraging12x |
| Tengah (sebelum bab program) | Paket Lengkap, versi ringan | /b/paket-… |
| Akhir bab program 30 hari | Challenge 14 Hari | /chalenge14days |
| Sebelum Bonus | **Penawaran utama:** Paket Rp499rb + konsultasi | /b/paket-… |
| Sebelum Daftar Pustaka | Challenge (mini) | /chalenge14days |
| Akhir buku | Rak semua produk + "rekomendasikan buku ini" | semua link |

Aturan: maksimal 1 CTA per batas bab, dan penawaran utama (`.offer`) hanya sekali. Buku yang sedang dibaca tidak dijual lagi di dalamnya, cukup dipasang sebagai link "rekomendasikan ke teman".

---

## 9. Tracking (Meta Pixel)

- Pixel: **Aging Gracefully `2472150893185263`**, dipasang di `<head>`.
- Hanya 4 event standar, semuanya dengan `content_category: 'ebook'`:

| Event | Kapan |
|---|---|
| `ViewContent` | halaman dibuka |
| `Lead` | selesai kuis **atau** klik Beauty Tracker (sekali per kunjungan) |
| `AddToCart` | klik CTA produk berbayar, dengan `value` + `currency: 'IDR'` |
| `Purchase` | dikirim dari halaman checkout Scalev (bukan dari ebook) |

- Semua link CTA otomatis diberi `utm_source=ebook_<slug-buku>&utm_medium=webbook&utm_campaign=<produk>&utm_content=<posisi>`.
- Audiens di Ads Manager (HKU 1 & MJO): `Ebook <KODE> · 1 ViewContent / 2 Lead / 3 AddToCart / 4 Purchase`.

---

## 10. Teknis & deploy

```
<repo-buku>/
├── index.html      # satu file: CSS + JS inline
├── img/            # semua gambar webp
├── vercel.json     # cleanUrls, cache 1 tahun untuk /img, X-Robots-Tag noindex
├── robots.txt      # Disallow: /  (konten berbayar)
└── DESIGN.md
```

- Satu repo GitHub dan satu project Vercel per buku (tim beautytracker, Framework "Other", tanpa build). Terpisah dari app tracker.
- Interaksi (posisi baca, jawaban kuis, centang tracker) disimpan di `localStorage` dengan prefix kode buku, misalnya `btm:`.
- Tanpa library JS. Vanilla saja.

---

## 11. Checklist sebelum publish

- [ ] Jumlah kata web = PDF (selisih < 1%), tidak ada teks hilang atau dobel
- [ ] Tidak ada scroll horizontal di lebar 390px
- [ ] Mode terang dan gelap terbaca
- [ ] Semua gambar muncul, ilustrasi rusak sudah diganti
- [ ] Kuis dan tracker jalan, progres tersimpan setelah reload
- [ ] Semua link CTA benar dan ber-UTM
- [ ] Events Manager → Test Events: ViewContent → Lead → AddToCart muncul
- [ ] Disclaimer kesehatan ada di atas dan di footer
- [ ] `noindex` aktif

---

## 12. Tambahan dari Buku 1 (Gracefully Aging: Beautycode Biohacking)

| Komponen | Kelas | Aturan |
|---|---|---|
| Tombol tema | `#themeBtn` | ikon bulan/matahari di top bar, simpan pilihan di `localStorage` (`<kode>:theme`), set `data-theme` di `<html>` sebelum render |
| Epigraf bab | `blockquote.epi` | di dalam `.ch-head`, `<cite>` untuk nama |
| Catatan Biohacker / Insight | `aside.note` | latar `--paper-2`, garis kiri `--bark`, label `.note-label` = teks label dari buku |
| Power of 3 Action | `aside.p3` | `ol` bernomor 01–03 font display `--bark` |
| Catatan Body Type | `aside.bt` | `li[data-t=red|green|yellow|white]`, chip warna tipe |
| Judul seksi body type | `h2[data-t]` | chip warna tipe di depan judul |
| Kuis skor | `.tool#kuis` | soal & interpretasi skor diambil apa adanya dari buku, hasil + meter + CTA Tracker gratis (Lead) |
| Tracker program | `.tool#tracker-12` | kartu per minggu, centang diambil otomatis dari `aside.p3` minggu itu |
| Rak buku penulis | `.gal.books` | sampul dari PDF, geser horizontal, tanpa link |

Bab bertingkat (Chapter 5 Bagian I–III) memakai `ch-num` angka Romawi dan kicker "Chapter 5 · Bagian I".
