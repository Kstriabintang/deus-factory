# ROADMAP — Plugin Dewa 🔱

> Pabrik website all-in-one untuk Claude Code. Satu input: nama website. Sisanya: keputusan yang kamu **acc satu per satu**, lalu Dewa membangun website company-grade — dan merawatnya seumur hidup proyek.

**Disusun:** 18 Agustus 2026 · **Target:** 8 milestone (M0–M7) · **Status:** M0 selesai ✅ — berikutnya M1 (Wizard Engine)

---

## 1. Visi

Plugin Dewa mengubah Claude Code menjadi pabrik website profesional dengan satu alur:

```
/dewa:buat NamaWebsite
   → Wizard: ±12 keputusan, kamu acc satu per satu (dengan preview)
   → Brief: semua keputusanmu jadi satu sumber kebenaran (brief.json)
   → Design System: brief diterjemahkan jadi token (warna, font, spacing, motion)
   → Build: pasukan agent spesialis membangun paralel
   → Audit: QA menyerang hasilnya sebelum kamu lihat
   → Serah terima + preview
   → Lifecycle: revisi, tambah halaman, rebrand, konten, deploy — kapan pun
```

Bukan generator template. Setiap acc-mu membentuk design system unik untuk proyek itu.

---

## 2. Prinsip Dewa

Enam prinsip yang membedakan plugin ini dari generator biasa:

1. **Satu input, sisanya pilihan.** Kamu tidak pernah disuruh mengetik panjang. Semua keputusan ditawarkan sebagai opsi siap-acc, dengan preview bila memungkinkan. Selalu ada "Other" kalau mau isi sendiri.
2. **Design-system-first.** Setiap acc menjadi token, bukan nilai hardcode. Ganti tema = ganti token, seluruh situs ikut. Ini yang membuat `/dewa:rebrand` mungkin.
3. **Konten asli, bukan lorem ipsum.** Copywriting sungguhan sesuai tone brand: headline, CTA, isi section — siap pakai.
4. **Dalam di tiap domain.** Arsitektur informasi, desain, copywriting, motion, SEO, dan QA masing-masing ditangani agent spesialis — bukan satu AI mengerjakan semua sambil lalu.
5. **Website hidup setelah jadi.** Brief tersimpan permanen di proyek. Revisi per bagian, halaman baru, rebrand — semua konsisten karena membaca brief yang sama.
6. **Diaudit sebelum diserahkan.** dewa-qa menguji responsive, aksesibilitas, performa, link, dan SEO secara adversarial. Kamu menerima website yang sudah lulus uji.

---

## 3. Pengalaman Pengguna — Alur Wizard

Wizard bertanya **satu keputusan per langkah** (sesuai gayamu: acc satu per satu). Tiap langkah maksimal 4 opsi terkurasi + "Other" untuk isian bebas.

| #  | Langkah              | Bentuk                                        | Preview                        |
|----|----------------------|-----------------------------------------------|--------------------------------|
| 1  | Tujuan situs         | Business / Personal / Organisasi / Toko       | —                              |
| 2  | Bidang & audiens     | Opsi dinamis menyesuaikan langkah 1           | —                              |
| 3  | Kepribadian brand    | Mewah / Profesional / Hangat / Berani         | —                              |
| 4  | Stack kode           | HTML+CSS+JS murni / Astro / Next.js           | Cuplikan struktur proyek       |
| 5  | Palet warna          | 4 palet **dirancang khusus** dari langkah 1–3 | Swatch warna                   |
| 6  | Tipografi            | Pasangan font display + body                  | Contoh set huruf               |
| 7  | Gaya UI/UX           | Preset: corporate premium / minimal elegan / bold modern / classic editorial | Mockup ASCII |
| 8  | Motion               | Tenang / Hidup / Sinematik                    | Deskripsi pola animasi         |
| 9  | Struktur halaman     | Multi-select halaman yang dibutuhkan          | —                              |
| 10 | Section per halaman  | Acc susunan section tiap halaman              | Sketsa susunan                 |
| 11 | Bahasa & tone konten | ID / EN / Dwibahasa + gaya penulisan          | Contoh kalimat                 |
| 12 | Ringkasan brief      | **Acc final** sebelum build                   | Brief lengkap                  |

Poin penting: opsi palet, font, dan gaya **bukan daftar tetap** — Dewa merancang kandidatnya berdasarkan jawaban langkah sebelumnya. Dua brief berbeda tidak pernah ditawari opsi yang sama persis.

Setelah acc final → build berjalan → laporan audit → serah terima dengan preview.

---

## 4. Arsitektur Plugin

Struktur sesuai spesifikasi resmi plugin Claude Code (terverifikasi, lihat §8):

```
plugin-dewa/
├── .claude-plugin/
│   └── plugin.json               # manifest: name "dewa", versi, deskripsi
├── skills/
│   ├── buat/
│   │   ├── SKILL.md              # wizard utama (dijaga < 500 baris)
│   │   └── references/
│   │       ├── alur-wizard.md    # urutan langkah, aturan opsi & preview
│   │       ├── preset-gaya.md    # spesifikasi dalam tiap preset UI/UX
│   │       ├── palet.md          # sistem perancangan palet + aturan kontras
│   │       ├── tipografi.md      # pustaka pasangan font per kepribadian
│   │       ├── motion.md         # kosakata animasi: durasi, easing, pola
│   │       └── konten.md         # formula copywriting per jenis section
│   ├── status/SKILL.md           # tampilkan brief & progres
│   ├── revisi/SKILL.md           # ubah satu bagian, alur acc
│   ├── halaman/SKILL.md          # tambah halaman konsisten
│   ├── rebrand/SKILL.md          # ganti tema via token
│   ├── konten/SKILL.md           # perbarui copywriting
│   ├── preview/SKILL.md          # jalankan lokal + screenshot
│   ├── audit/SKILL.md            # QA menyeluruh
│   └── deploy/SKILL.md           # publikasi
├── agents/
│   ├── dewa-arsitek.md
│   ├── dewa-desainer.md
│   ├── dewa-penulis.md
│   ├── dewa-motion.md
│   ├── dewa-seo.md
│   └── dewa-qa.md
├── hooks/
│   └── hooks.json                # opsional (M5): validator & pemuat brief
├── scripts/
│   ├── validasi.sh               # cek cepat HTML/link hasil build
│   └── screenshot.sh             # bantu /dewa:preview
└── templates/                    # kerangka awal per stack
```

**Keputusan arsitektur kunci:**

- **Wizard jalan di percakapan utama, build didelegasikan ke agent.** Tool pertanyaan interaktif (AskUserQuestion) tidak tersedia di dalam subagent — jadi skill `buat` TIDAK memakai `context: fork`. Setelah brief final di-acc, barulah agent-agent dipanggil paralel.
- **Brief sebagai sumber kebenaran.** Setiap proyek yang dibangun Dewa punya folder `dewa/` berisi `brief.json` (semua keputusan) dan `design-system.css` (token). Semua skill pasca-build membaca ini — itulah kenapa revisi tidak pernah merusak konsistensi.
- **Preferensi lintas proyek** disimpan di `${CLAUDE_PLUGIN_DATA}` (folder data permanen plugin) — Dewa v2 bisa berkata "biasanya kamu pilih Astro + gaya minimal, mau sama?"

### Perintah

| Perintah              | Fungsi                                                            |
|-----------------------|-------------------------------------------------------------------|
| `/dewa:buat <nama>`   | Wizard lengkap → bangun website dari nol                          |
| `/dewa:status`        | Tampilkan brief & progres proyek aktif                            |
| `/dewa:revisi <bagian>` | Ubah satu bagian (dengan alur acc) tanpa merusak yang lain      |
| `/dewa:halaman <nama>`| Tambah halaman baru yang konsisten dengan design system           |
| `/dewa:rebrand`       | Ganti warna/font/gaya lewat token — seluruh situs ikut            |
| `/dewa:konten`        | Perbarui copywriting per section                                  |
| `/dewa:preview`       | Jalankan lokal + screenshot tiap halaman                          |
| `/dewa:audit`         | QA menyeluruh: responsive, a11y, SEO, performa, link              |
| `/dewa:deploy`        | Publikasi ke Vercel / Netlify / GitHub Pages                      |

### Pasukan Agent

| Agent           | Keahlian (dalam, bukan permukaan)                                        |
|-----------------|--------------------------------------------------------------------------|
| `dewa-arsitek`  | Struktur informasi: sitemap, susunan section, alur pengunjung → konversi  |
| `dewa-desainer` | Design system: token warna/tipografi/spacing, komponen, layout            |
| `dewa-penulis`  | Copywriting asli: headline, CTA, isi section — mengikuti tone brief       |
| `dewa-motion`   | Sistem animasi: durasi, easing, reveal, micro-interaction, reduced-motion |
| `dewa-seo`      | Meta, Open Graph, sitemap.xml, structured data, HTML semantik             |
| `dewa-qa`       | Penguji adversarial: mencari cacat responsive/a11y/performa/link/SEO      |

---

## 5. Fase Pengerjaan (M0 → M7)

### M0 — Fondasi ✅ *(selesai 18 Agustus 2026)*
Scaffold plugin yang valid dan bisa di-load.
- Buat struktur folder + `.claude-plugin/plugin.json` (name: `dewa`)
- Satu skill percobaan untuk membuktikan alur load
- Siapkan loop pengembangan: `claude --plugin-dir ./plugin-dewa` + `/reload-plugins`

**Definisi selesai:** `claude plugin validate ./plugin-dewa --strict` hijau; `/dewa:buat` muncul di menu slash command.

### M1 — Wizard Engine *(± 1–2 sesi)*
Jantung pengalaman "acc satu per satu".
- Skill `buat` + seluruh file `references/` (alur, preset, palet, tipografi, motion, konten)
- Aturan merancang opsi dinamis per langkah (bukan daftar statis) + preview
- Output: `dewa/brief.json` yang rapi dan lengkap
- Langkah ringkasan + acc final

**Definisi selesai:** wizard bisa dijalankan penuh 12 langkah (tanpa build), menghasilkan brief.json yang konsisten untuk 3 skenario uji: company profile, portfolio personal, toko.

### M2 — Mesin Design System *(± 1 sesi)*
Penerjemah brief → sistem visual.
- Generator `design-system.css`: token warna (light+dark), skala tipografi, spacing, radius, shadow
- 4 preset gaya UI/UX terdefinisi **dalam**: anatomi komponen, aturan layout, karakter tiap preset
- Kosakata motion: durasi, easing, pola reveal, aturan `prefers-reduced-motion`
- Aturan kontras & aksesibilitas warna bawaan

**Definisi selesai:** dari satu brief.json, design system tergenerasi utuh dan terdokumentasi di folder proyek.

### M3 — Build Engine *(± 2–3 sesi)*
Dari design system → website utuh. Mulai **satu stack dulu** (statis modern) supaya kualitas dalam, stack lain menyusul.
- Generator halaman + komponen sesuai preset gaya
- Copywriting asli seluruh section (via aturan `konten.md`)
- Aset: logo placeholder SVG, ikon, strategi gambar
- Responsive penuh (mobile-first), HTML semantik
- SEO teknis bawaan: meta, OG, sitemap.xml, structured data

**Definisi selesai:** `/dewa:buat` menghasilkan 1 website business lengkap dari nama → wizard → jadi, tanpa intervensi manual.

### M4 — Pasukan Agent *(± 1–2 sesi)*
Kedalaman per domain + kecepatan paralel.
- Tulis 6 definisi agent (`agents/*.md`) dengan spesialisasi tajam
- Orkestrasi: skill `buat` mendelegasikan build ke agent secara paralel
- `dewa-qa` berjalan wajib di akhir; hasil audit dilampirkan di serah terima

**Definisi selesai:** build M3 dikerjakan oleh pasukan agent; laporan audit menyertai setiap serah terima.

### M5 — Toolkit Pasca-Build *(± 2 sesi)*
Website yang hidup.
- Skill: `status`, `revisi`, `halaman`, `rebrand`, `konten`, `preview`, `audit`, `deploy`
- Semua membaca `dewa/brief.json` — konsistensi terjaga otomatis
- Hooks opsional: `SessionStart` memuat ringkasan brief bila folder proyek Dewa terdeteksi; validator ringan pasca-edit
- `deploy`: deteksi CLI yang tersedia (Vercel/Netlify/gh-pages), tuntun sampai live

**Definisi selesai:** siklus penuh terbukti — bangun → revisi 1 section → tambah 1 halaman → rebrand → audit → deploy, semua konsisten.

### M6 — Packaging & Distribusi *(± 1 sesi)*
Dari folder lokal → plugin yang bisa di-install siapa pun.
- README + dokumentasi pemakaian
- Versioning semver + changelog
- Repo GitHub + `marketplace.json`
- Uji instal bersih: `/plugin marketplace add <akun-github>/plugin-dewa` → `/plugin install dewa@plugin-dewa`

**Definisi selesai:** instal dari marketplace di mesin/sesi bersih berhasil; validate --strict hijau.

### M7 — Evolusi v2 *(berkelanjutan)*
- Memori preferensi lintas proyek (`${CLAUDE_PLUGIN_DATA}`)
- Stack tambahan: Next.js penuh, Astro content collections
- Modul: blog, toko (e-commerce), integrasi CMS, formulir + backend ringan
- Template industri (restoran, agensi, klinik, dst.) sebagai akselerator wizard
- Dwibahasa penuh (i18n) + integrasi analytics

---

## 6. Perkiraan Total

± **8–11 sesi kerja** sampai M6 (plugin utuh, terdistribusi). Website pertama yang berfungsi sudah lahir di akhir **M3**.

---

## 7. Risiko & Mitigasi

| Risiko                                                   | Mitigasi                                                                 |
|----------------------------------------------------------|--------------------------------------------------------------------------|
| Maks 4 opsi per pertanyaan (batas tool)                   | Kurasi 4 opsi terbaik per konteks brief; "Other" selalu tersedia          |
| Preview hanya untuk pertanyaan single-select              | Langkah multi-select (struktur halaman) memakai deskripsi kaya            |
| Tool pertanyaan tidak tersedia di subagent                | Wizard di percakapan utama; hanya build yang didelegasikan ke agent       |
| SKILL.md membengkak (> 500 baris)                         | Pecah ke `references/`; SKILL.md hanya alur inti                          |
| Scope terlalu besar sekaligus                             | Satu stack dulu sampai dalam (M3), stack lain setelah stabil              |
| Kualitas tidak konsisten antar-perintah                   | Semua perintah wajib membaca brief; QA wajib sebelum serah terima         |
| `claude plugin eval` masih early access                   | Uji manual via skenario M1/M3/M5; adopsi eval resmi saat tersedia         |

---

## 8. Fakta Teknis Terverifikasi (dok resmi Claude Code)

- Manifest wajib di `.claude-plugin/plugin.json`; hanya `name` yang required (kebab-case, jadi prefix namespace).
- Skill: `skills/<nama>/SKILL.md` → terpanggil sebagai `/dewa:<nama>`. Agent: `agents/<nama>.md` → `@dewa:<nama>`.
- Frontmatter skill mendukung: `description`, `argument-hint`, `allowed-tools`, `context: fork` (dihindari untuk wizard), `model`, `effort`, dll.
- Placeholder tersedia: `$ARGUMENTS`, `${CLAUDE_PLUGIN_ROOT}` (folder plugin), `${CLAUDE_PLUGIN_DATA}` (data permanen), `${CLAUDE_PROJECT_DIR}`.
- Dev lokal: `claude --plugin-dir ./plugin-dewa`; perubahan skill/agent/hook cukup `/reload-plugins` (MCP perlu restart).
- Validasi: `claude plugin validate ./plugin-dewa --strict`.
- Distribusi: `marketplace.json` di repo → `/plugin marketplace add` → `/plugin install dewa@<marketplace>`.
- AskUserQuestion: 1–4 pertanyaan per panggilan, 2–4 opsi per pertanyaan, `multiSelect` didukung, preview untuk single-select; **tidak tersedia di subagent**.
- Hooks tersedia: `SessionStart`, `UserPromptSubmit`, `PreToolUse`, `PostToolUse`, `Stop`, `SubagentStop`, `FileChanged`, dll. via `hooks/hooks.json`.

Sumber: code.claude.com/docs — plugins, plugins-reference, skills, sub-agents, hooks, plugin-marketplaces.

---

## 9. Langkah Berikutnya

**M0 selesai** (18 Agu 2026): `plugin-dewa/` berdiri, `claude plugin validate --strict` hijau, `dewa:buat` terbukti terdaftar via smoke test `--plugin-dir`.

**Eksekusi M1 — Wizard Engine:** aktifkan 12 langkah penuh + tulis seluruh `references/` (preset gaya, palet, tipografi, motion, konten) sampai dalam. Bilang saja **"gas M1"**. 🔱
