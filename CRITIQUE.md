# CRITIQUE — PosterPack — 2026-10-29

## Slop test (v3) + 3 hal paling template + yang dirombak
- Slop test: LULUS. Logo ditutup pun halaman ini tidak ketuker dengan web AI
  umum: kertas koran jingga + display Blaka bergaya blackletter + lembar kerja
  spreadsheet + stempel "sah" berotasi adalah kombinasi yang tidak generik.
- 3 hal paling template: (1) tombol aksi berderet generik — dirombak: tombol
  memakai suara Bu Tini di status, bukan label pasaran; (2) section "cara
  pakai" 3 langkah — dipertahankan karena memang urutan pakai, tapi ditulis
  dengan suara persona, bukan copy SaaS; (3) kartu contoh — dirombak: kartu
  berfungsi sebagai pengisi lembar kerja (klik → semua baris terisi), bukan
  pajangan.
- Keputusan berani (tanpa hero marketing, langsung meja kerja) kelihatan dalam
  5 detik pertama: screenshot 390px menunjukkan editor + judul raksasa tanpa
  hero. Signature moment (ketik → poster ikut berubah) terverifikasi tes fungsi.

## Gerbang gerak (v4): jumlah lapisan, zoom, pan, 2 marquee, FPS 4x
- Lapisan: A kamera scrub (scale 1→1,06 + rotate) ✓ | B 2 marquee berlawanan +
  skew velocity + tombol jeda ✓ | C kata jatuh-menumpuk ✓ (variable-axis
  diganti — Heebo via Astro Fonts bukan variable, dicatat di BRIEF) | D drag
  strip inertia + tombol prev/next ✓ | E curtain wipe (clip-path) ✓ | F dipangkas.
- FPS 4x CPU throttle (qa.js, chromium): 56,4 (run pertama 52,2; run dengan
  grain mix-blend-mode 43,2 → blend dihapus, naik ke 56,4).
- Pan horizontal pinned (desktop) dipangkas: strip contoh sudah memberi pan
  horizontal via drag; pin section menambah risiko kunci scroll di HP.

## Gerbang v5: tes bandingkan, blur, 3 detik, keyboard-only, reduced-motion, mode ringan
- Tes bandingkan: berdampingan dengan BidKit (10-28) — beda total: palet,
  font, layout (spreadsheet vs kios), persona. Tes blur: blok kertas + judul
  raksasa + stempel tetap beridentitas. Tes 3 detik: "masak poster di sini"
  + lembar kerja langsung menjelaskan fungsi.
- Keyboard-only: semua fitur bisa dipakai tanpa mouse (tab ke input, template,
  aksi, strip prev/next, checklist). Reduced-motion: 0 animasi berjalan di
  qa.js (matches: true). Mode ringan: hardwareConcurrency ≤4 / Save-Data →
  marquee dijeda + kamera dimatikan (class .lite).
- Budget: JS inline di halaman (tanpa library gerak eksternal); CSS halaman
  + Tailwind template; font self-host 3 family (Blaka/Heebo/Bilbo, subset
  latin). LCP/CLS/INP tidak terukur tanpa Lighthouse (lihat bawah).

## Budget performa + Lighthouse (mobile/desktop)
- Lighthouse: BELUM TERVERIFIKASI. 3 percobaan gagal di tahap chrome-launcher
  (meta-chromium + playwright chromium via launcher: "waiting for dynamic
  debugging port") — preseden yang sama di build 10-16..10-28. Playwright
  chromium (protokol langsung, bukan launcher) jalan normal untuk qa.js.
  Skor Lighthouse TIDAK dikarang; DoD Lighthouse di README dicatat jujur.
- qa.js lokal exit 0: 0 overflow di 4 viewport, 0 console error, 0 tap target
  kecil, 0 img tanpa alt, 0 input tanpa label, h1 = 1, lang = id.

## Keamanan + lisensi
- Data pengguna tidak pernah dirender via innerHTML (textContent semua).
  localStorage key berversi (posterpack-v1) + try/catch + cek skema; param URL
  divalidasi panjang dan diabaikan bila rusak. Tidak ada API key, tracker,
  atau analytics. Font OFL (Google Fonts), ikon SVG digambar sendiri.
- Fakta: tidak ada klaim harga/jadwal/statistik lomba — checklist syarat
  dilabeli pengingat umum, bukan aturan resmi lomba mana pun (R7).

## Kontras (dihitung dari oklch → sRGB, script /tmp/oklch.py)
- teks/bg 8,03:1 ✓ | muted/bg 5,31:1 ✓ | teks/paper 17,36:1 ✓ |
  accent-ink/paper 8,19:1 ✓ | teks/aksen (plt-laut) 6,06:1 ✓ |
  teks tinta-bg 16,35:1 ✓.
- Temuan & perbaikan: putih di atas aksen mentah hanya 3,22:1 → tombol
  template/palet aktif diubah ke teks gelap; putih di aksen dipertahankan
  hanya untuk teks besar (ajakan display ≥3:1 lolos). accent-ink di atas bg
  3,79:1 dan accent2-ink di atas bg 4,36:1 tidak dipakai untuk teks kecil
  (hanya border/outline/fill).

## Skor 1-10: karakter, keunikan, kerapian
- Karakter 9 | Keunikan 9 | Kerapian 8 (checkbox 44px terlihat besar; marquee
  Blaka rapat — sudah diberi line-height 1,4 + padding).

## Keputusan konflik aturan + daftar dipangkas
- Base path: preseden build sebelumnya (base '/') membuat aset /_astro
  404 di project Pages (terverifikasi: root-asset 404, subpath-asset 200).
  Build ini memakai base '/agent-tools-posterpack/' + trailingSlash true
  (wajib, kalau tidak homepage jadi stub redirect loop di Pages).
- Variabel-font axis diganti kata jatuh (font bukan variable).
- Lapisan F (shader/fisika) dan pan pinned dipangkas (alasan di atas).
- Vercel: tidak dicoba ulang — koneksi sebelumnya 403 scope tim (catatan
  BidKit 10-28); deploy via GitHub Pages repo baru (project baru, R2).

## Bug yang ditemukan tes fungsi dan diperbaiki (putaran rombak)
1. Wipe clip-path + IntersectionObserver pada elemen yang sama = section
   tidak pernah intersecting (clip-path ikut dihitung IO) → seluruh halaman
   di bawah header tidak terlihat. Perbaikan: sentinel 1px di luar section
   + class .js (tanpa JS, konten tetap tampil).
2. Data contoh via `<script type="application/json">{JSON.stringify(...)}</script>`
   tidak dievaluasi Astro (isi literal) → klik contoh gagal diam. Perbaikan:
   data lewat atribut data-* pada tombol kartu.
3. Marquee Blaka terpotong atas → line-height 1,4 + padding 12px.
4. pkill -f "astro preview" mematikan shell sendiri (self-match) → server
   di-restart via PID, bukan pola.

## Putaran rombak
- Ke-1: overflow marquee (min-width 0 + overflow-x clip), checkbox 44px,
  lang id, wipe sentinel, data-atribut contoh, grain tanpa blend.
- Ke-2: kontras tombol aktif, polish marquee. Tidak ada putaran ke-3.
