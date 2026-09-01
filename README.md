# Portal Belajar Mandiri — Deutschklasse XII ULW (v2)

**Bahasa Jerman · Kelas XII ULW · SMKN 1 Sewon · Fase F · TP 2026/2027**
Penyusun: Yudha Nur Wicaksono

Portal web satu-link untuk siswa, dibuka lewat HP. Dibangun dengan React 18 + Tailwind CSS
(lewat CDN, tanpa proses build). Alur tiap materi: **video pengantar → rangkuman → latihan**.

## Cara membagikan
Buka galeri Artifacts (claude.ai/code/artifacts) → **Deutschklasse XII ULW** → menu Share.
Artifact privat sampai dibagikan. Isi bisa diperbarui kapan saja tanpa tautannya berubah.

## Struktur (v2)
| No | Materi | Video YouTube | Latihan |
|---|---|---|---|
| 01 | Kata Benda & Artikel Dasar | `4cRAhte5oe0` | 9 |
| 02 | Akkusativ für Anfänger & Kata Kerja | `4i5rbghwRto` | 12 |
| 03 | Keluarga & Kepemilikan (Possessivartikel) | `luhPMbPRs3o` | 16 |
| 04 | Aktivitas Bersama Teman | `1BVkzaBL_Hc` | 8 |
| 05 | Negasi: Membantah dengan nicht | `1jnW4aKkHtU` | 8 |
| 06 | Länder & Städte | — | 7 |
| ⚡ | Daily Challenge (5 soal campuran) | — | 5 |
| A–Z | Wortschatz / Kamus Cepat (90+ kata) | — | — |
| ★ | Akkusativ Survival Kit (artifact terpisah, tertaut) | — | 10 |

Total 65 latihan di dalam portal.

## Integrasi materi tercecer
- **Verben mit Akkusativ** (haben, brauchen, essen, trinken, sehen, kaufen, finden, kennen,
  nehmen) masuk sebagai sub-bab ketiga di Materi 02, tepat setelah The Golden Rule.
- **Negasi `nicht`** dipisah jadi Materi 05 dan diikat langsung ke Materi 04: dua baris teratas
  tabel posisi `nicht` memakai kalimat aktivitas jamak (`Wir spielen nicht Fußball`,
  `Ihr spielt nicht Gitarre`), dan Materi 04 ditutup dengan pengantar ke Materi 05.

## Catatan teknis penting
- **Progres siswa aman.** Kunci penyimpanan dan id materi tidak diubah, jadi nilai terbaik yang
  sudah tercatat di HP siswa tetap terbawa antarversi.
- Daftar nama siswa pada Lampiran 3 kedua modul **tidak** dimasukkan (data pribadi).
- Contoh `telefonieren + Akkusativ` diganti `besuchen / sehen / kennen` (bentuk baku
  `telefonieren mit + Dativ`, atau `anrufen` yang transitif).
- `die USA` ditandai jamak → `aus / in den USA`.


## Versi mandiri untuk GitHub Pages (`index.html`)
Selain versi Artifact, ada berkas `index.html` mandiri: dokumen HTML5 utuh, satu file, tanpa
proses build. React 18 (UMD) dan Tailwind dipanggil dari CDN, jadi tidak perlu Node.js/NPM/Vite.

## Evaluasi Akhir & Sertifikat (v5)
- Tiap video kini didahului **kalimat pengantar (hook)** 1–2 kalimat, disimpan di field `hook`
  pada objek video di dalam `TOPICS`.
- Rute baru `#/evaluasi`: gerbang nama siswa → 5 soal pilihan ganda (satu per materi utama) →
  skor → sertifikat. Data soal ada di array `EVAL`.
- Sertifikat memuat nama, skor, tanggal (format Indonesia), "SMKN 1 Sewon", dan nama guru.
  Digambar dengan gaya sebaris berwarna tetap supaya tampilannya sama di mode terang/gelap dan
  terekam utuh oleh html2canvas. Ambang tuntas `LULUS_MIN = 60`.
- Unduh JPG memakai `html2canvas@1.4.1` dari jsDelivr, skala 2–4× menyesuaikan lebar layar.
- Konstanta `BISA_UNDUH` membedakan kedua versi: `true` di `index.html` (GitHub Pages, unduhan
  berfungsi), `false` di versi Artifact (unduhan diblokir browser pratinjau, jadi tombolnya
  diganti petunjuk menyimpan lewat screenshot).
- Nama siswa dan nilai evaluasi terbaik disimpan di localStorage bersama progres materi;
  tidak dikirim ke mana pun.
- **Babel/JSX sengaja tidak dipakai.** React dipanggil sebagai UMD dan komponen ditulis dengan
  `React.createElement`, karena Babel standalone berukuran ~3 MB dan harus mengompilasi di HP
  siswa tiap kali halaman dibuka.

## Varian v6 — SPA 4 materi + evaluasi ber-AI (berkas terpisah)
Dibangun ulang dari nol atas permintaan eksplisit; **menggantikan isi situs GitHub Pages** kalau
diunggah. Versi 6 materi + 65 latihan tetap tersimpan di Artifact sebagai cadangan.

- Struktur: Materi 01 Kata Benda & Artikel Dasar · 02 Keluarga & Kepemilikan · 03 Akkusativ &
  Kata Kerja · 04 Aktivitas Bersama Teman & Negasi (2 video) · Evaluasi Akhir. Tidak ada
  Wortschatz, Daily Challenge, Länder, maupun Survival Kit.
- Navigasi memakai React state (`useState`), bukan hash routing. Menu tab di bilah atas.
- Ditulis dengan **JSX** dan dikompilasi di browser oleh `@babel/standalone@7`
  (`<script type="text/babel" data-presets="react">`).
- Evaluasi: 19 soal auto-grading (5 PG, 4 Benar/Salah, 4 Menjodohkan, 3 Rumpang, 2 Susun Kata,
  1 Nominativ→Akkusativ) + 1 esai yang dikoreksi lewat Google Apps Script.
- Nilai akhir = objektif × 0,8 + esai × 0,2 (konstanta `BOBOT_OBJEKTIF` / `BOBOT_ESAI`).
- `GAS_URL` adalah placeholder. **Tidak ada kunci API di berkas HTML** — semuanya di sisi GAS.
- Permintaan ke GAS memakai `Content-Type: text/plain;charset=utf-8` dan `redirect:"follow"`
  karena Apps Script tidak menjawab preflight CORS. Ini wajib, jangan diubah ke application/json.
- Tiga jalur cadangan sudah diuji: GAS belum diisi, jaringan gagal, dan esai terlalu pendek —
  semuanya jatuh ke nilai objektif saja dengan pesan yang jelas, bukan halaman rusak.
- JSX diverifikasi dengan kompiler TypeScript offline (Babel CDN tidak terjangkau dari sandbox).
