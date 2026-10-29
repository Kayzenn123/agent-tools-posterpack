# BRIEF — PosterPack — 2026-10-29

## Masalah/usecase (1 kalimat + siapa yang kena)
Pelajar yang ikut lomba poster/infografis sering kalah bukan karena ide jelek, tapi karena susunan poster berantakan dan syarat administratif kelewat — PosterPack memberi mereka dapur poster: isi 6 blok, pilih template, poster langsung jadi + checklist syarat.

## Konsep visual (1 kalimat)
Seluruh situs = satu meja redaksi koran kuning berbentuk lembar kerja spreadsheet: baris-baris blok poster yang diisi, pratinjau poster hidup di samping, berbutir grain koran.

## Hasil seed (re-seed ATURAN 19 v5, sebelum kode)
Seed pertama (base 2026-10-29 → 2026-11-09) DITOLAK: font display/body/aksen
(Allura/Catamaran/Playwrite AU VIC) identik dengan BidKit 2026-10-28 — tabrakan
1 hari. Re-seed base 2026-11-10 → seed 2026-11-15, dipakai:
- domain seed: resep nenek | layout: spreadsheet sebagai UI | referensi: koran kuning
- constraint: grain/noise wajib
- gerak: A kamera "terbang" scroll (scale+translate+rotate) | B 2 marquee berlawanan
  + skew velocity | C variable font axis (diganti: kata jatuh-menumpuk — Heebo via
  Astro Fonts bukan variable, dicatat di CRITIQUE) | D drag inertia flick
  | E curtain wipe antar section
- ikon: monoline satu garis menyambung (Streamline)
- font: Blaka (display) / Heebo (body) / Bilbo (aksen)
- palet: mid / split-complementary, hue dasar 21 — bg oklch(0.74 0.12 21),
  surface oklch(0.60 0.11 21), teks oklch(0.16 0.03 21), muted oklch(0.32 0.04 21),
  aksen teal oklch(0.61 0.182 171), aksen-2 biru oklch(0.56 0.20 231),
  pop biru muda oklch(0.83 0.225 237)

## 3 arah desain (2 kalimat masing-masing) + arah terpilih
1. Kios template statis: galeri 6 template, klik → unduh PDF. Rapi tapi bukan
   tool — pengunjung tidak bisa mengisi posternya sendiri.
2. Dapur poster spreadsheet (TERPILIH): editor berbentuk lembar kerja + pratinjau
   hidup + checklist syarat. Paling jauh dari default hero+3 kartu, dan paling
   sesuai janji "tool": data masuk, poster keluar.
3. Kanvas drag-and-drop bebas: poster diseret-seret seperti Figma. Paling seru
   tapi paling rawan pecah di HP dan paling lama dibangun — dipangkas.

## Keputusan berani
Tanpa hero marketing sama sekali: pengunjung mendarat langsung di meja kerja
(editor + poster kosong yang menunggu diisi). Navigasi = tab lembar kerja
(ISI | CONTOH | SYARAT), bukan menu atas.

## Persona/suara
Bu Tini, tukang cetak koran kuning yang ngomong seperti resep: "bahan dulu,
baru masak poster". Semua copy — termasuk pesan error dan empty state — pakai
suara ini, huruf kalimat normal (bukan semua-kecil; constraint seed = grain,
bukan huruf kecil).

## Palet (harmoni, kenapa, psikologi warna, token final, kontras AA)
Harmoni split-complementary dari hue jingga-kertas 21: teal dan biru sebagai
penyeimbang dingin. Psikologi: kertas hangat = koran yang akrab, teal = stempel
"sah" yang menenangkan. Token final (CSS vars): --bg, --surface, --text,
--muted, --accent, --accent-2, --pop + varian tinta gelap --accent-ink
(oklch 0.36) dan --accent2-ink (oklch 0.34) untuk teks kecil agar lolos AA
(aksen mentah 0.61/0.56 hanya untuk fill besar dan teks raksasa).
Kontras terverifikasi dengan script oklch→sRGB (lihat CRITIQUE): teks/bg 8,03:1,
muted/bg 5,31:1, accent-ink/paper 8,19:1, teks/paper 17,36:1, teks/aksen
(plt-laut) 6,06:1. Putih di atas aksen mentah hanya 3,22:1 → dipakai hanya
untuk teks besar (ajakan display); tombol template aktif memakai teks gelap.

## Font (display/body/aksen, alasan, id Fontsource, variable atau bukan)
Blaka (display, dekoratif tebal — judul poster seperti headline koran),
Heebo (body, sangat terbaca, 400/500/700), Bilbo (aksen tulisan tangan untuk
catatan Bu Tini). Id Fontsource: blaka, heebo, bilbo. Bukan variable (Astro
Fonts mengambil weight statis) → animasi axis diganti kata jatuh-menumpuk.

## Ikon (gaya, sumber, daftar 12+ ikon, 3+ custom)
Gaya monoline satu garis menyambung, SVG inline digambar sendiri (stroke 1.5,
tanpa fill). 15 ikon: poster, sendok-resep (custom), stempel SAH (custom),
gulungan poster (custom), salin, unduh, cetak, tautan, acak, hapus, checklist,
lampu, air, sampah, tangan, sepeda, buku, listrik (dipakai di kartu contoh).
Dilarang: emoji sebagai ikon, rocket/lightbulb/shield/sparkle.

## Gerak (kategori A-F, narasi, MODE RINGAN)
Narasi: "masuk ke dapur cetak" — scroll = berjalan masuk ke meja redaksi.
A kamera: hero poster scale 1→1,06 + rotate scrub. B 2 marquee berlawanan +
skew velocity + tombol jeda. C kata jatuh-menumpuk di headline. D drag strip
contoh dengan inertia + tombol prev/next. E curtain wipe (clip-path) antar
section saat masuk viewport. F dipangkas. MODE RINGAN (hardwareConcurrency
≤4 / Save-Data / FPS <40 selama 2 dtk): matikan marquee, kamera, dan drag
inertia (strip jadi scroll native).

## Storyboard hero moment
| Scroll | Yang terlihat | Kamera | Marquee | Teks | Interaksi |
| --- | --- | --- | --- | --- | --- |
| 0% | meja kerja: editor kiri, poster kosong kanan | scale 1 | 2 band jalan | kata jatuh satu-satu | isi judul → poster ikut berubah |
| 25% | strip contoh poster | scale 1,02 | band atas miring saat scroll cepat | — | seret strip / klik contoh |
| 50% | template + palet | scale 1,04 | — | — | klik template → poster berubah bentuk |
| 75% | checklist syarat | scale 1,06 | band bawah berlawanan | — | centang → progres naik |
| 100% | ekspor + footer | kembali 1 | — | stempel SAH muncul | salin / unduh / cetak |

5 detik pertama: pengunjung melihat meja kerja + poster yang berubah saat
mengetik — "keputusan berani" (tanpa hero) langsung terasa.

## Konten (15+ item nyata, easter egg)
6 contoh poster edukasi (hemat air, pilah sampah, cuci tangan, hemat listrik,
baca tiap hari, aman bersepeda) + 12 item checklist syarat lomba (dilabeli
"periksa pamflet resmimu — tiap lomba beda", bukan fakta lomba tertentu) +
6 template + 3 palet. Easter egg: ketik judul "resep nenek" → catatan Bu Tini
muncul: "nah, ini resep andalan saya."

## Teknik visual orisinal (2+)
1. Grain koran via SVG feTurbulence overlay (constraint seed).
2. Tipografi sebagai ilustrasi: judul poster raksasa + stempel SAH berotasi.
3. Pola garis koran berulang (repeating-linear-gradient) sebagai divider.

## Suara
Tidak — tanpa audio.
