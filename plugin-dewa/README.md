# Plugin Dewa 🔱

Pabrik website all-in-one untuk Claude Code. **Satu input: nama website.** Sisanya keputusan yang kamu acc satu per satu — lalu Dewa membangun website company-grade dan merawatnya seumur hidup proyek.

**Versi:** 0.1.0 (M0 — Fondasi) · **Roadmap lengkap:** [`../ROADMAP.md`](../ROADMAP.md)

## Status milestone

- [x] **M0 — Fondasi**: scaffold valid, skill `buat` v0 (wizard parsial → brief.json)
- [ ] M1 — Wizard Engine penuh (12 langkah)
- [ ] M2 — Mesin Design System (brief → token)
- [ ] M3 — Build Engine (website utuh pertama)
- [ ] M4 — Pasukan 6 agent spesialis
- [ ] M5 — Toolkit pasca-build (revisi/rebrand/audit/deploy)
- [ ] M6 — Packaging & marketplace

## Cara pakai (mode pengembangan)

```bash
cd "/Users/macbookpro/Documents/Revan Files/GITHUB-ME/Claude Powerfull"
claude --plugin-dir ./plugin-dewa
```

Lalu di dalam sesi:

```
/dewa:buat NamaWebsiteKamu
```

Wizard v0 akan menuntunmu: tujuan → bidang → kepribadian → arah warna → menghasilkan `dewa/brief.json` di folder proyek website-mu.

## Loop pengembangan

- Ubah skill/agent/hook → ketik `/reload-plugins` di sesi (tanpa restart).
- Validasi setiap selesai mengubah struktur:

```bash
claude plugin validate ./plugin-dewa --strict
```

## Struktur

```
plugin-dewa/
├── .claude-plugin/plugin.json    # manifest (name: dewa)
├── skills/
│   └── buat/
│       ├── SKILL.md              # wizard v0 — alur acc satu per satu
│       └── references/
│           └── alur-wizard.md    # konstitusi 12 langkah + skema brief.json
├── agents/                       # M4: dewa-arsitek, dewa-desainer, dst.
├── hooks/                        # M5 (opsional)
├── scripts/                      # M3+: validasi & screenshot
└── templates/                    # M3: kerangka per stack
```

Folder `agents/`, `hooks/`, `scripts/`, `templates/` diisi pada milestone-nya masing-masing — struktur mengikuti [ROADMAP.md](../ROADMAP.md) §4.
