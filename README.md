# posterpack — dapur poster edukasi

> bahan dulu, baru masak poster.

PosterPack membantu pelajar yang ikut lomba poster/infografis: isi 6 blok
(judul, pesan, 3 bahan, ajakan) + sumber + nama, pilih 1 dari 6 template dan
1 dari 3 palet, poster langsung jadi di pratinjau. Ada 6 contoh poster siap
pakai dan checklist 12 syarat lomba biar tidak ada yang kelewat.

## fitur

- **lembar kerja spreadsheet** — 8 baris isian, tiap baris ada penanda terisi.
- **pratinjau poster hidup** — ketik, poster ikut berubah (6 template: koran,
  resep, papan, kartu, selebaran, struk; 3 palet: kertas koran, laut, tinta malam).
- **6 contoh poster** — hemat air, pilah sampah, cuci tangan, hemat listrik,
  baca tiap hari, aman bersepeda. Klik kartu → semua baris terisi.
- **checklist syarat lomba** — 12 item + progres, tersimpan di perangkat.
  Daftar ini pengingat umum, bukan aturan resmi lomba mana pun — yang mengikat
  tetap pamflet resmi lombamu.
- **ekspor** — salin teks poster, unduh .txt, cetak draf, salin tautan.
- **deep-link** — `?judul=...&pesan=...&f1=...&tpl=struk&plt=laut` — isian ikut
  kebawa lewat tautan.
- **penyimpanan lokal** — semua data di localStorage (`posterpack-v1`,
  `posterpack-syarat-v1`), tidak dikirim ke server. Tanpa akun, tanpa pelacak.
- **gerak** — kamera scrub, 2 marquee berlawanan + skew velocity, kata
  jatuh-menumpuk, drag strip inertia, curtain wipe. Ada tombol gerak on/off,
  jeda marquee, dan mode ringan otomatis. Hormat prefers-reduced-motion.
- **easter egg** — ketik judul mengandung "resep nenek", Bu Tini berkomentar.

## tech

- astro v7 + tailwind css v4 (template astrowind)
- font: Blaka (display), Heebo (body), Bilbo (aksen) — via Astro Fonts API
  (fontsource, self-hosted, subset latin)
- ikon: SVG monoline digambar sendiri (tanpa emoji)
- tanpa framework js di klien — vanilla js, tanpa tracker

## jalanin lokal

```bash
npm install
npm run dev     # http://localhost:4321/agent-tools-posterpack/
npm run build   # output ke dist/
```

## skor

- live: https://kayzenn123.github.io/agent-tools-posterpack/ (http 200,
  pages status built — verifikasi di akhir run)
- qa.js lokal: exit 0 — 0 overflow, 0 console error, 0 tap target kecil,
  0 img tanpa alt, 0 input tanpa label, h1 = 1, lang id; fps 4x cpu throttle:
  56,4; reduced-motion: 0 animasi berjalan
- tes fungsi: 14/14 lulus (deep-link, ketik→pratinjau, easter egg, contoh→isian,
  template, checklist + localstorage, reload bertahan, kosongkan, error tanpa judul)
- lighthouse: BELUM TERVERIFIKASI — chrome-launcher gagal di VM (3x percobaan:
  meta-chromium + playwright chromium via launcher; preseden build 10-16..10-28).
  Playwright qa.js jalan normal. Skor lighthouse tidak dikarang.
- vercel: BELUM — percobaan sebelumnya 403 scope tim (catatan BidKit 10-28).
  Deploy via github pages (repo baru, project baru).

## lisensi

Font OFL (Google Fonts). Ikon dan grafis buatan sendiri. Data contoh bersifat
edukasi umum, bukan klaim statistik resmi.
