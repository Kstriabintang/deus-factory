# Konstitusi Wizard Dewa — Spesifikasi 12 Langkah

Dokumen ini adalah sumber kebenaran alur wizard `/dewa:buat`. Skill membaca ini sebelum memulai.
Status implementasi: **v0 (M0)** menjalankan langkah 1, 2, 3, dan 5-lite. Sisanya diaktifkan di **M1**.

## Filosofi

1. Pengguna memberi SATU input: nama website. Sisanya pilihan yang di-acc satu per satu.
2. Setiap acc menjadi field di `brief.json` — sumber kebenaran seumur hidup proyek.
3. Opsi bukan daftar tetap: dirancang dari jawaban langkah-langkah sebelumnya.
4. Maksimal 4 opsi per pertanyaan (batas tool); "Other" selalu tersedia otomatis dan dihormati penuh.
5. Preview dipakai di setiap langkah single-select yang punya wujud visual/tekstual.
6. Satu keputusan per giliran. Tidak pernah dua langkah digabung.

## 12 Langkah

| #  | Langkah              | Bentuk        | Aturan kurasi opsi                                              | Preview                          | Field brief          |
|----|----------------------|---------------|------------------------------------------------------------------|----------------------------------|----------------------|
| 1  | Tujuan situs         | single-select | Tetap: Business / Personal / Organisasi / Toko                   | —                                | `tujuan`             |
| 2  | Bidang & audiens     | single-select | 4 bidang paling relevan untuk tujuan terpilih; audiens disebut   | —                                | `bidang`             |
| 3  | Kepribadian brand    | single-select | Mewah / Profesional / Hangat / Berani, dideskripsikan dalam konteks bidang | —                      | `kepribadian`        |
| 4  | Stack kode           | single-select | HTML+CSS+JS murni / Astro / Next.js — jelaskan trade-off untuk kebutuhan user | Cuplikan struktur proyek | `stack`              |
| 5  | Palet warna          | single-select | 4 palet dirancang dari langkah 1–3; patuhi aturan kontras WCAG   | Swatch: peran warna + hex        | `warna`              |
| 6  | Tipografi            | single-select | Pasangan display+body dari pustaka per kepribadian (Google Fonts) | Contoh set huruf                | `tipografi`          |
| 7  | Gaya UI/UX           | single-select | Preset: corporate premium / minimal elegan / bold modern / classic editorial | Mockup ASCII layout hero+section | `gaya_ui`     |
| 8  | Motion               | single-select | Tenang / Hidup / Sinematik — jelaskan pola konkretnya            | Deskripsi pola animasi           | `motion`             |
| 9  | Struktur halaman     | multi-select  | Halaman kandidat sesuai tujuan+bidang (Beranda selalu ada)       | — (multi-select tanpa preview)   | `halaman[]`          |
| 10 | Section per halaman  | single-select per halaman | Susunan section direkomendasikan per halaman, acc per halaman | Sketsa urutan section       | `halaman[].sections` |
| 11 | Bahasa & tone konten | single-select | ID / EN / Dwibahasa; tone diturunkan dari kepribadian            | Contoh 1–2 kalimat copywriting   | `konten`             |
| 12 | Ringkasan brief      | single-select | Acc final / Ubah salah satu langkah                              | Brief lengkap                    | `status`             |

Aturan tambahan:

- **Langkah 12** menawarkan revisi: bila pengguna memilih mengubah satu langkah, ulangi langkah itu saja lalu kembali ke ringkasan.
- **Setelah acc final** (mulai M3): delegasikan build ke pasukan agent; wizard tetap di percakapan utama karena AskUserQuestion tidak tersedia di subagent.
- Bila pengguna memilih "Other", catat teks aslinya di brief dan rancang ulang opsi langkah berikutnya berdasarkan itu.

## Skema `dewa/brief.json`

Lokasi: `<folder-proyek>/dewa/brief.json`. Contoh terisi (v0 — field pending bernilai null):

```json
{
  "dewa": "0.1.0",
  "dibuat": "2026-08-18",
  "nama": "Kopi Senja",
  "folder": "kopi-senja",
  "tujuan": "business",
  "bidang": {
    "industri": "kedai kopi",
    "audiens": "pekerja urban 20-35 yang cari tempat kerja santai"
  },
  "kepribadian": "hangat",
  "stack": null,
  "warna": {
    "arah": "earthy senja",
    "peran": { "latar": "#F6EFE6", "tinta": "#2B2118", "aksen": "#C46A2B" },
    "palet_penuh": null
  },
  "tipografi": null,
  "gaya_ui": null,
  "motion": null,
  "halaman": null,
  "konten": { "bahasa": null, "tone": null },
  "status": {
    "wizard": "v0-parsial",
    "acc_final": false,
    "pending": ["stack", "tipografi", "gaya_ui", "motion", "halaman", "konten"]
  }
}
```

Aturan skema:

- Semua keputusan acc-an masuk brief **apa adanya** — termasuk isian "Other".
- Field yang belum ditanya = `null` dan tercatat di `status.pending` (bukan dihapus).
- `status.acc_final` hanya `true` setelah langkah 12 di-acc — syarat mutlak sebelum build (M3).
- Brief tidak pernah ditimpa buta: bila sudah ada, tanyakan dulu mau lanjut/mulai ulang.

## Rencana referensi pendamping (diisi di M1–M2)

- `preset-gaya.md` — anatomi 4 preset UI/UX: komponen, layout, karakter.
- `palet.md` — sistem perancangan palet + aturan kontras.
- `tipografi.md` — pustaka pasangan font per kepribadian.
- `motion.md` — kosakata animasi: durasi, easing, pola reveal.
- `konten.md` — formula copywriting per jenis section.
