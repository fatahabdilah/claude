# Catatan Proyek — Make with Dona

Ringkasan lengkap semua percakapan, keputusan, naskah, riset, dan artifact dari sesi Claude Code di cloud (22–25 November 2026). File ini dibuat supaya pekerjaan bisa diteruskan di laptop atau di sesi lain tanpa kehilangan konteks.

> **Untuk Claude yang membaca file ini:** baca seluruh file sebelum mulai. Ikuti preferensi pemilik di bagian 1. Jangan mengarang fakta; tulis "tidak tahu" atau "tidak disebutkan" kalau memang tidak ada sumbernya.

---

## 1. Tentang pemilik & aturan kerja

- Kreator konten **Make with Dona** (`makewithdona`): animasi 3D pendek vertikal untuk TikTok/Reels.
- Bahasa: Indonesia santai ("kamu", "nggak").
- Target penonton: **Gen Z yang ngantor + ngampus** (kerja pagi, kuliah malam).
- Preferensi pemilik:
  - Jangan pakai informasi palsu. Kalau tidak tahu, bilang tidak tahu.
  - Sumber harus valid, dan sumbernya disertakan.
  - Pikir dulu sebelum menjawab.
  - Pakai data terbaru.
- Jangan menjanjikan "pasti viral" atau "pasti 1 juta views". Bilang jujur bahwa tidak ada formula yang menjamin.
- Jangan sebut nama asli kampus, kantor, atau orang dalam konten keluh kesah (risiko hukum seperti UU ITE). Pakai tokoh fiksi.

---

## 2. File di repo (branch `claude/dona-animation-example-mz4w9k`)

| File | Isi | Artifact (privat, milik pemilik) |
|---|---|---|
| `dona-skor-ai.html` | Video animasi contoh 30 detik "Skor Kelihatan Buatan AI" + tombol Rekam & Unduh | https://claude.ai/artifact/7DyGLJrh5ogNuq5QjBZnTf |
| `dona-karakter.html` | 8 karakter 3D dengan nama, peran, tampilan, sifat, kalimat khas, dan tombol ekspresi | https://claude.ai/artifact/SgogoHKoaBiK9B2NUk57ku |
| `dona-setup.html` | 4 setup pemeran (Kos, Rumah, Startup, Pasar) + ide video + blok prompt siap salin | https://claude.ai/artifact/3aSmzuEargVyVAJHeCVW5f |
| `CATATAN-PROYEK.md` | File ini | – |

Cara buka di laptop: `git clone https://github.com/fatahabdilah/claude.git`, lalu `git checkout claude/dona-animation-example-mz4w9k`, lalu klik dua kali file HTML-nya. Perlu internet karena three.js dimuat dari cdnjs.

### Mesin animasi (`dona-skor-ai.html`), dipakai ulang untuk video berikutnya
- **Dasar:** satu file HTML, tanpa build. three.js r128 dari `https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js`.
- **Kanvas dan render:** kanvas output 2D 1080×1920. WebGL dirender ke kanvas terpisah lalu digabung, dan caption, vignette, butiran, serta logo digambar di kanvas 2D supaya ikut terekam.
- **Gerak:** semua ditentukan oleh fungsi deterministik `frame(t)`. Acak memakai seed `mulberry32`.
- **Struktur data:**
  - Timeline tokoh: `PATH` (posisi), `POSE`, `EXPR`, `JUMPS`.
  - Kamera: `SHOTS` (dari/ke posisi, target, fov; opsi `whipOut`/`whipIn`/`follow`/`dolly`).
  - Naskah: `LINES` (caption kata per kata, kata penting ditandai `*`).
- **Audio:** WebAudio tanpa file, berisi pop, klik, whoosh, ding, alarm, tick, dan musik bass + akor.
- **Rekam:** `canvas.captureStream(30)` + `MediaStreamDestination` lalu `MediaRecorder`. Urutan mimeType: mp4 avc1 → mp4 → webm vp9 → webm.
- **Unduh:** di dalam viewer claude.ai, pakai capability `downloads` (`claude.use("downloads").save`). Di luar viewer, pakai link `<a download>`.
- **Narasi:** tombol 🔊 Narasi memakai `speechSynthesis` id-ID. Hanya pratinjau, tidak ikut terekam.
- **Sudah diuji:** di Chromium headless tanpa error konsol, dan rekaman menghasilkan file 1080×1920 dengan audio. **Belum diuji** di iPhone/Android asli.

### Gaya karakter (wajib konsisten)
- **Bentuk dasar:**
  - Kepala bola putih (MeshToonMaterial + emissive) dengan garis tepi #2a1612 (inverted hull).
  - Wajah digambar di CanvasTexture pada bidang lengkung.
  - Badan buah pir (LatheGeometry) dengan motif kain dari canvas.
  - Lengan dan kaki "mi" (TubeGeometry dari CatmullRomCurve3), tangan sarung putih kecil.
- **Ekspresi:** calm, talk, wide, pale (keringat), smug, grin.
- **Dona (tokoh utama brand):** rambut bob hitam mengkilap + poni tipis, jepit oranye #ff7a1a di kiri atas, cardigan rajut rust #9a4a24 di atas kaos krem #fff3e2, celana gelap, sepatu putih. Sifatnya percaya diri, agak sombong tapi lucu.
- **Aturan warna:** oranye #ff7a1a hanya untuk Dona dan momen menang.

---

## 3. Daftar karakter

### Set utama (`dona-karakter.html`)
| Nama | Peran | Tampilan | Kalimat khas |
|---|---|---|---|
| Dona | Tokoh utama, kreator | Lihat di atas | "Santai. Lihat aja." |
| Raka | Rekan kerja sok tahu | Topi pet merah #d8352a, kaos garis biru #2f6d9e/krem | "Halah, gampang itu mah." |
| Bu Sari | Klien pemilik UMKM | Sanggul cokelat, kemeja kuning #f2c14e polkadot, tas selempang | "Bisa diskon nggak, Mbak?" |
| Bimo | Barista kafe kantor | Rambut cepak, flanel kotak merah-navy, celemek | "Waduh." |
| Pak Bambang | Bos pemberi deadline | Kumis tebal, kacamata bulat, batik kawung cokelat/emas, badan agak bulat | "Pokoknya besok pagi sudah jadi, ya." |
| Tante Rita | Tante grup WA keluarga | Rambut keriting ungu tua, kacamata hitam di kepala, blus bunga magenta, pegang HP | "Sudah baca pesan yang Tante kirim belum?" |
| Nayla | Anak magang Gen Z | Hijab hijau sage, hoodie lavender, tote bag | "Kak, ini bisa pakai AI nggak sih?" |
| Ucup | Mahasiswa tingkat akhir | Rambut berjambul, kantung mata, jaket almamater biru tua, ransel | "Revisi lagi, revisi lagi." |

### 4 setup pemeran (`dona-setup.html`), ⭐ = tokoh utama
1. **Kos Melati:** ⭐Gilang (anak kos tanggal tua), Bu Endang (ibu kos), Sasa (anak kos sebelah).
2. **Rumah Pak Joko:** ⭐Kevin (anak SD), Mama Lina (ibu jualan online), Pak Joko (ayah santai).
3. **Kantor Startup Lantai 3:** ⭐Tari (tim keuangan), Ardi (founder), Ozan (programmer).
4. **Pasar Pagi:** ⭐Mak Ijah (penjual gorengan), Asep (tukang parkir), Putri (food vlogger).

Semua nama dan tokoh fiksi.

---

## 4. Riset (dengan sumber)

Catatan: TikTok tidak bisa ditonton langsung dari sesi cloud, jadi angka views berasal dari liputan media, bukan hitungan sendiri.

- **Pengguna TikTok Indonesia:** TikTok menyebut lebih dari 160 juta pengguna aktif bulanan di Indonesia ([Fortune IDN](https://www.fortuneidn.com/tech/tembus-160-juta-pengguna-tiktok-indonesia-nomor-1-di-asia-tenggara-00-6cmtl-r9xl4s/amp)). Rata-rata 44 jam 54 menit per bulan menurut We Are Social 2025 ([GoodStats](https://data.goodstats.id/statistic/orang-indonesia-habiskan-44-jam-per-bulan-main-tiktok-pada-2025-GGb0C)).
- **Hook di awal video:** panduan TikTok menyebut lebih dari 63% video dengan klik tertinggi menunjukkan pesan atau produk utama dalam 3 detik pertama (dikutip [OpusClip](https://www.opus.pro/blog/tiktok-hook-formulas)). Angka seperti "retensi 70–85% = 2,2x views" **tidak punya sumber primer**, jadi jangan dipakai.
- **Laporan TikTok What's Next 2025 versi Indonesia** ([Technode](https://technode.global/2025/02/27/tiktok-releases-whats-next-2025-trend-report/), [Fortune IDN](https://www.fortuneidn.com/tech/tren-konten-tiktok-2025-00-fykp7-2kvtxf)):
  - 80% pengguna suka campuran gambar dan video.
  - Pengguna 1,3x lebih menantikan AI generatif di iklan.
  - Kategori populer: komedi, edukasi, makanan.
- **Konten karier dan kantor:** disukai karena santai, lucu, dan memakai situasi kerja nyata ([Binus](https://binus.ac.id/binus-digital/2026/06/15/kenapa-konten-career-tiktok-semakin-digemari-gen-z/)). Drama kantor banyak dikomentari karena relate ([Pusat Sosial](https://pusatsosial.com/2026/05/14/fenomena-viral-di-tiktok-yang-sedang-ramai-dibahas-netizen-2026/)).
- **Fruit Love Island (series animasi AI):** dilaporkan hampir 300 juta views dari 20 episode ([DesignRush](https://news.designrush.com/tiktok-series-fruit-love-island-ai-format-risk), [Forbes](https://www.forbes.com/sites/danidiplacido/2026/03/24/tiktoks-viral-fruit-love-island-trend-sparks-online-backlash/), [RTÉ](https://www.rte.ie/entertainment/2026/0404/1566709-fruit-love-island-goes-viral-on-tiktok)). Kuncinya:
  - parodi format yang sudah dikenal,
  - episode harian dengan cliffhanger,
  - penonton ikut voting jalan cerita.

  Series ini juga dikritik sebagai konten asal jadi dan dipersoalkan hak ciptanya.
- **Drama mikro AI:** umumnya episode vertikal 60–90 detik, bersambung, dengan cliffhanger ([Movcl](https://movcl.com/blog/guides/why-do-ai-short-drama-leads-all-look-kind-of-familiar/)). Angka views di sumber seperti ini kebanyakan klaim promosi.
- **Survei penggunaan AI di Indonesia:** hasilnya sangat beda-beda, misalnya APJII 18,2% ([Metro TV](https://www.metrotvnews.com/read/kqYCdR56-survei-apjii-gen-z-paling-banyak-mengakses-ai)) dan Google e-Conomy SEA 2025 80% ([Google](https://blog.google/intl/id-id/company-news/outreach-initiatives/e-conomy-sea-2025-indonesia-pusat-kekuatan-ai-baru-asia-tenggara/)). Jangan dikutip sebagai satu angka.

**Kesimpulan:** pola yang dipakai adalah konflik yang relate, cerita bersambung dengan cliffhanger, penonton ikut berinteraksi, dan parodi format yang sudah dikenal. Naskah awal sempat dinilai pemilik "datar". Yang disukai adalah **format parodi yang lucu**.

---

## 5. Naskah yang sudah dibuat

### 5a. Video contoh: "Skor Kelihatan Buatan AI" (sudah jadi animasi)
Rangkuman cerita:
1. Semua website punya angka merah.
2. Raka meremehkan: "Punya Dona pasti seratus persen."
3. Dona: "Santai. Lihat aja."
4. Skor Raka 12%, Dona 94%.
5. Dona ganti foto asli, kalimat sendiri, dan satu lelucon, lalu skornya jadi 6% dan berubah oranye.
6. Analogi: "AI itu kayak nasi goreng hotel: enak, tapi rasanya sama di mana-mana. Bumbunya harus dari kamu."
7. Ending: Raka ngecek skor dirinya sendiri… dan ternyata…

Caption: "Kalau semua website punya skor 'kelihatan buatan AI', punyamu berapa? Bumbunya tetap harus dari kamu. #makewithdona #webdesign #AI"

### 5b. Naskah promosi untuk menjual "cara bikin animasi kayak gini" (±35 dtk)
Alur:
1. Hook: "Video ini nggak dibikin animator. Nggak pakai aplikasi edit sama sekali."
2. "Aku cuma nempel satu prompt…"
3. Teman meremehkan.
4. Bukti rekam layar.
5. "Rahasianya di cara nulis prompt-nya."
6. Tawaran [nama produk].
7. CTA: "Komen DONA."

Hook cadangan:
- "Ini dibikin dari satu chat. Serius."
- "Kalau kamu bisa ngetik, kamu bisa bikin animasi kayak gini."
- "Teman aku kira aku sewa animator. Padahal…"

Nama dan harga produk belum ditentukan pemilik.

### 5c. Satu suara: "Baterai Sosial"
```
Pagi ini, di atas kepala semua orang tiba-tiba muncul baterai.
Punyaku? Lima persen. Wajar sih. Pagi ngantor, malam ngampus.
Teman sekelasku langsung nyeletuk. Kerja sambil kuliah? Paling seminggu lagi kamu tidur di kelas.
Aku cuma jawab. Tenang. Aku punya cara.
Terus bosku chat. Nanti malam makan bareng tim, ya.
Baterai langsung turun. Dua persen.
Tapi aku pasang earphone. Nyalain mode pesawat. Dua puluh menit aja.
Dan baterainya naik. Delapan puluh persen.
Karena ternyata, baterai sosial itu kayak HP. Dicas pakai kopi cuma bikin panas. Yang ngisi itu jeda tanpa notifikasi.
Terus temanku iseng ngecek baterainya sendiri. Dan ternyata...
```

### 5d. Series misteri: "Pagi Ngantor, Malam Ngampus" (sticky note kuning)
- **Premis:** setiap malam ada sticky note kuning di laptopku, dan isinya selalu terjadi besoknya.
- **Penulisnya:** Pak Bambang (bos), yang ternyata juga kuliah malam di kelas yang sama. Dia tahu jadwal "aku" di kantor dan di kampus.
- **Episode:**
  1. Catatan dari Besok
  2. Dua Tempat: tulisannya ada di kantor dan kampus
  3. Orang Berbatik
  4. Besok Kamu Tahu: suara dari baris belakang mirip Pak Bambang
  5. Tulisan Kedua: penyamaran terbongkar, tapi ada catatan baru dengan tulisan beda, "Jangan percaya dia." Ini pancingan season 2.

  Naskah lengkap 5 episode ada di percakapan cloud. Kalau perlu, tulis ulang dari ringkasan ini.

### 5e. Cerita plenger (twist)
- **A. Teman Kursi Sebelah:** teman sekelas "Dimas" ternyata tidak pernah ada, dan catatannya tulisan tangan "aku" sendiri.
- **B. Bos Jam Dua Pagi:** bos selalu membalas chat dalam 5 detik, ternyata HP-nya hilang 3 bulan lalu, dan "yang naikin gajiku ternyata bukan dia".
- **C. Tulisan di Gelas Kopi:** mas penjual kopi ternyata direktur baru.

### 5f. Dari keluh kesah pemilik (dosen tidak masuk, gaji under paid)
- **Dosen Nggak Masuk:** izin kerja, hujan, kelas kosong, lalu kelas diganti tugas. Minggu depan "aku" tidak datang, dan ternyata dosennya datang dan mengadakan kuis.
- **Gaji Rekan Sebelah**, alasan yang dipilih pemilik adalah #1:
  - Rekan santai yang selalu bilang "nanti kita sinkronisasi ya" gajinya lebih besar.
  - HR menunjukkan laporan kinerja: tiga project "aku" semuanya tercatat atas nama dia.
  - Pelajaran: "Kerja keras yang nggak tercatat atas namamu, ya nggak dihitung."
  - Ending: "Dan balasan bos…" sebagai pancingan Part 2.
- **Alasan alternatif:** dia orang baru dengan gaji harga pasar, status "aku" di sistem masih anak magang, atau dia nego waktu masuk.

### 5g. Series sinetron: "Slip Gaji yang Tertukar"
- **Format:** parodi sinetron, zoom tiga kali.
- **Episode 1:** slip gaji Rizal keluar dari printer. Nomor karyawan, tanggal masuk, dan jabatannya sama dengan milik "aku". Ending: Rizal berkata, "Akhirnya kamu tahu juga."
- **Voting:** A) lapor HR, B) pura-pura nggak tahu.
- **Episode 2 (B):** Rizal menawarkan "bagi dua asal kamu diam". Rekaman HP "aku" tidak tersimpan karena baterai 1%.
- **Episode 3–7:** kepala HR ternyata tantenya Rizal → satpam punya CCTV → separuh kantor datanya tertukar → yang memasang sistem gaji adalah Rizal saat magang IT → "Masih ada satu nama lagi."

### 5h. Series lucu: "Azab Kantor & Kampus" (parodi judul sinetron azab, suara narator serius)
- Azab Rekan Kerja yang Gajinya Gede Tapi Kerjaannya Cuma Bilang "Sinkronisasi"
- Azab Dosen yang Nggak Masuk Padahal Mahasiswanya Sudah Hujan-hujanan ke Kampus
- Azab Bos yang Chat Jam Sebelas Malam, Seumur Hidup Cuma Dibalas "Siap"

### 5i. Format parodi lain
1. **Dokumenter Alam:** dipilih pemilik, lihat bagian 6.
2. **Berita Terkini Kampus**
3. **Iklan TV Belanja "Obat Anti Under Paid"**
4. **Resep "Cara Membuat Mahasiswa Kerja Sambil Kuliah"**, dengan punchline "Kapan lulus?"
5. **Review Jujur "Kerja Sambil Kuliah"**, bintang tiga

---

## 6. ✅ Naskah TERPILIH: "Kehidupan Liar Karyawan" (Dokumenter Alam)

```
Inilah karyawan Gen Z, di habitat aslinya. Meja paling pojok, dekat colokan.

Pagi hari, ia bergerak lambat. Ia belum mendapatkan kopi pertamanya.

Perhatikan. Sang bos mendekat.

Karyawan Gen Z tidak lari. Ia membuka tab Excel. Ini adalah bentuk penyamaran alami.

Bos pergi. Tab Excel ditutup kembali.

Pukul lima sore, terjadi fenomena langka. Karyawan Gen Z bergerak sangat cepat, menuju pintu keluar.

Tapi alam tidak selalu adil.

Pukul lima lewat satu menit, terdengar suara itu.

Bisa meeting sebentar?

Dan sang karyawan pun, kembali ke habitatnya.
```

### Pengaturan ElevenLabs (agar konsisten)
- **Model:** Eleven Multilingual v2 (rekomendasi untuk konsistensi). Dukungan bahasa Indonesia perlu dicek di daftar bahasa.
  - Eleven v3 lebih ekspresif dan mendukung tag `[whispers]` dan `[sighs]`, tapi tidak punya pengaturan Speed dan Similarity ([dokumentasi ElevenLabs](https://elevenlabs.io/docs/product-guides/playground/text-to-speech), [blog v3](https://elevenlabs.io/blog/eleven-v3)).
- **Suara:** cari *documentary narrator, calm, deep* di Voice Library. Atau pakai Voice Design dengan prompt: *"Indonesian male narrator, 40s, deep and warm voice, calm and slow, soft half-whisper like a wildlife documentary narrator, serious and full of wonder, clear Indonesian pronunciation."*
- **Pengaturan:** Stability 60, Similarity 75, Style 0–10, Speaker Boost nyala, Speed 0,9.
- **Supaya konsisten:**
  - Pakai suara, model, dan pengaturan yang sama.
  - Generate satu episode sekaligus, ambil 2–3 versi, lalu pilih yang terbaik.
  - Kalau pelafalan salah: "Gen Z" → "Jen Zi", "Excel" → "Eksel".
- **Kalimat "Bisa meeting sebentar?"** tetap dibaca narator. Di animasi, kalimat itu tampil sebagai teks besar dari kepala bos.

### Rancangan animasi (sesuaikan dengan durasi MP3 asli)
- **Pemeran:** Nayla (karyawan Gen Z) dan Pak Bambang (bos).
- **Gaya dokumenter:** kamera mengintip dari balik daun, label spesies *Homo officius genzensis*, musik marimba dan string yang tenang.

| ± Detik | Narasi | Visual |
|---|---|---|
| 0–4 | Inilah karyawan Gen Z… | Telefoto dari balik daun, meja pojok, kabel charger. Label spesies dan "Habitat: dekat colokan" |
| 4–8 | Pagi hari, ia bergerak lambat… | Slow motion, mata setengah tertutup, gelas kopi kosong, "zzz" |
| 8–11 | Perhatikan. Sang bos mendekat. | Kamera rendah dari bawah meja, Pak Bambang masuk, musik mencekam |
| 11–15 | Ia membuka tab Excel… | Insert layar: video kucing berganti ke Excel, label "Kamuflase" |
| 15–18 | Bos pergi… | Layar kembali ke video kucing |
| 18–22 | Pukul lima sore… | Jam 17.00, Nayla berlari cepat dengan garis kecepatan |
| 22–24 | Tapi alam tidak selalu adil. | Freeze frame, musik berhenti |
| 24–27 | Pukul lima lewat satu menit… | Jam 17.01, kepala bos menyembul. Teks besar: "Bisa meeting sebentar?" |
| 27–32 | Dan sang karyawan pun… | Kamera mundur ke balik daun, biola sedih, logo makewithdona |

**Status:** menunggu MP3 dari ElevenLabs, lalu animasi dibuat dengan mesin `dona-skor-ai.html` plus tombol "Pilih audio narasi".

**Rencana series (satu spesies per episode):** Anak Magang, Bos Jam Sebelas Malam, Mahasiswa Kelas Malam, HRD saat Gajian.

---

## 7. Campaign konten.com (BELUM SELESAI)

- **Status:** brief **belum dibaca**. Yang ada hanya screenshot halaman `konten.com/clipper-campaigns`. konten.com tidak bisa diakses dari sesi cloud, dan Claude tidak bisa login ke akun pemilik.
- **Data dari screenshot (25 Nov 2026):**

| Campaign | Bayaran | Kategori | Angka di kartu (kemungkinan peserta) | Budget tersisa |
|---|---|---|---|---|
| 3726 MDPL (Falcon Pictures), Film Clipping | Rp2.000 / 1K views | Entertainment | 32.996 | 64% |
| **People Pleaser: Si Paling Gak Enakan** (PT. Lyto Sukses Bersama) | **Rp3.000 / 1K views** | Lifestyle | 5.908 | 81% |
| Suli Short Bitcoin (Suli) | Rp10.000 / 1K views | Education | 17.280 | 72% |
| No na x Shopee | – | – | – | – |
| UGC Aplikasi Konten.com | – | UGC | – | – |
| Bevan Collab | – | – | – | – |

- **Rekomendasi:** **People Pleaser**.
  - Temanya cocok dengan dunia kantor dan kampus.
  - Pesaingnya lebih sedikit dan budget-nya masih banyak.
  - Hitungan kasar: 1 juta views × Rp3.000/1K = Rp3.000.000, belum memperhitungkan batas maksimal atau aturan views valid.
- **Info film:**
  - **People Pleaser:** tayang mulai 19 November 2026 menurut artikel prarilis. Mawar de Jongh sebagai Karina, perempuan yang selalu mendahulukan orang lain. Sutradara Bobby Prasetyo ([Popmama](https://www.popmama.com/life/relationship/punya-sifat-nggak-enakan-mawar-de-jongh-relate-dengan-film-terbarunya-00-lkyx7-8r530c), [Mamikos](https://mamikos.com/info/?p=344989)).
  - **3726 MDPL:** sutradara Fajar Bustomi, tentang sekelompok pendaki ([Kapanlagi](https://www.kapanlagi.com/film/3726-mdpl-id14827.html)). Rumah produksi dan tanggal tayang belum terkonfirmasi.
- **Hindari Suli Short Bitcoin:** konten keuangan atau kripto berisiko.
- **Konsep untuk People Pleaser:** "LAPORAN KHUSUS: Wabah Gak Enakan".
  - Dona menjadi pembawa berita di studio 3D sinematik, dan TV di belakangnya memutar klip resmi film.
  - Ada momen kamera masuk ke TV sehingga klip tampil penuh di layar.
  - Elemen lain: lower-third, ticker, zoom dramatis.
  - Twist: Dona sendiri ternyata "gak enakan".
  - Klip dimuat dari file lokal pengguna (input file → VideoTexture), jadi tidak perlu diunggah ke mana pun.

```
Selamat malam. Laporan khusus hari ini. Wabah gak enakan kembali melanda kantor-kantor di Indonesia.

Gejalanya mudah dikenali. Penderita selalu menjawab, bisa kok. Padahal kerjaannya sendiri belum selesai.
[KLIP 1: Karina mengiyakan permintaan — dari aset resmi]

Seorang karyawan dilaporkan dititipi tiga kerjaan, dua fotokopi, dan satu pesanan kopi. Dalam satu jam.

Saat diwawancarai, korban hanya berkata. Nggak apa-apa kok. Santai.
[KLIP 2: reaksi lucu/dramatis — dari aset resmi]

Para ahli menyebut, kata tidak, sudah lama hilang dari kamus penderita.

Kasus terparah, kini bahkan diangkat ke layar lebar.

People Pleaser. Si Paling Gak Enakan.
[KLIP JUDUL]

Pertanyaannya. Apakah Anda juga terinfeksi?

Tunggu sebentar. Barusan produser saya minta saya lembur.

Dan saya jawab. Bisa kok.
```
(Tanda [KLIP] jangan ditempel ke ElevenLabs.)

- **Yang harus dicek di brief:**
  1. Animasi atau tokoh AI boleh?
  2. Minimal porsi klip film?
  3. Wajib menyebut apa?
  4. Boleh narasi sendiri?
  5. Cara bayar: views valid, batas per video, minimal views.
  6. Platform yang diizinkan.
  7. Larangan.
  8. Cara mengunduh aset klip.

  Kalau sesuatu tidak ada di brief, tulis "tidak disebutkan". Jangan menebak.
- **Cadangan kalau animasi tidak boleh di People Pleaser:** 3726 MDPL dengan format dokumenter alam ("Inilah pendaki Gen Z, di habitat aslinya…").
- **Aturan akun:** **hanya membaca** di akun konten.com pemilik. Jangan daftar, ikut campaign, upload, atau kirim apa pun tanpa izin eksplisit.

---

## 8. Langkah berikutnya

1. Baca brief People Pleaser (dan 3726 MDPL) lewat Chrome pemilik, mode baca saja. Rangkum aturannya dalam tabel.
2. Kalau animasi boleh, buat `dona-berita-people-pleaser.html`:
   - mesin dari `dona-skor-ai.html`,
   - tombol "Pilih klip video" dan "Pilih audio narasi",
   - tombol Rekam & Unduh.
3. Buat animasi "Kehidupan Liar Karyawan" setelah MP3 ElevenLabs tersedia (`dona-dokumenter-karyawan.html`).
4. Uji di browser tanpa error konsol, lalu commit ke branch `claude/dona-animation-example-mz4w9k`.

## 9. Melanjutkan di laptop
```bash
git clone https://github.com/fatahabdilah/claude.git
cd claude
git checkout claude/dona-animation-example-mz4w9k
claude --teleport     # tarik riwayat sesi web ke terminal (jika versi Claude Code mendukung)
# atau mulai sesi baru dan minta: "Baca CATATAN-PROYEK.md lalu lanjutkan langkah berikutnya"
```
Untuk membaca brief di Chrome, jalankan Claude Code dengan integrasi Chrome (misalnya `claude --chrome`) kalau tersedia di versimu.
