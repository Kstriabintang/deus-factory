# ROADMAP — Deus 🔱

> An all-in-one website factory for Claude Code. One input: a website name. The rest: decisions you **approve one by one**, then Deus builds a company-grade website — and maintains it for the life of the project.

**Authored:** 18 August 2026 · **Target:** 8 milestones (M0–M7) · **Status:** M0 done ✅ · public repo live — next up M1 (Wizard Engine)

---

## 1. Vision

Deus turns Claude Code into a professional website factory with a single flow:

```
/deus:build WebsiteName
   → Wizard: ±12 decisions, you approve one by one (with preview)
   → Brief: every decision becomes one source of truth (brief.json)
   → Design System: the brief becomes tokens (color, type, spacing, motion)
   → Build: a crew of specialist agents builds in parallel
   → Audit: QA attacks the result before you ever see it
   → Handoff + preview
   → Lifecycle: revise, add pages, rebrand, content, deploy — anytime
```

Not a template generator. Every approval shapes a unique design system for that project.

---

## 2. The Deus Principles

Six principles that set this apart from an ordinary generator:

1. **One input, everything else a choice.** You're never asked to write long prose. Every decision is offered as ready-to-approve options, with a preview where possible. "Other" is always there if you want to write your own.
2. **Design-system-first.** Every approval becomes a token, not a hardcoded value. Change the theme = change the tokens, and the whole site follows. This is what makes `/deus:rebrand` possible.
3. **Real content, not lorem ipsum.** Genuine copywriting in the brand's tone: headlines, CTAs, section body — ready to ship.
4. **Deep in every domain.** Information architecture, design, copywriting, motion, SEO, and QA are each handled by a specialist agent — not one AI doing everything in passing.
5. **The website is alive after launch.** The brief is stored permanently in the project. Section revisions, new pages, rebrands — all consistent because they read the same brief.
6. **Audited before handoff.** deus-qa adversarially tests responsive layout, accessibility, performance, links, and SEO. You receive a website that already passed.

---

## 3. User Experience — The Wizard Flow

The wizard asks **one decision per step** (your approve-one-by-one style). Each step offers at most 4 curated options + "Other" for free input.

| #  | Step                    | Form                                          | Preview                        |
|----|-------------------------|-----------------------------------------------|--------------------------------|
| 1  | Purpose                 | Business / Personal / Organization / Store    | —                              |
| 2  | Industry & audience     | Dynamic options matching step 1               | —                              |
| 3  | Brand personality       | Luxurious / Professional / Warm / Bold        | —                              |
| 4  | Tech stack              | Plain HTML+CSS+JS / Astro / Next.js           | Project structure snippet      |
| 5  | Color palette           | 4 palettes **designed** from steps 1–3        | Color swatches                 |
| 6  | Typography              | Display + body font pairing                   | Type specimen                  |
| 7  | UI/UX style             | Preset: corporate premium / minimal elegant / bold modern / classic editorial | ASCII mockup |
| 8  | Motion                  | Calm / Lively / Cinematic                     | Animation pattern description  |
| 9  | Page structure          | Multi-select the pages you need               | —                              |
| 10 | Sections per page       | Approve the section order of each page        | Section-order sketch           |
| 11 | Content language & tone | EN / ID / Bilingual + writing style           | Sample sentence                |
| 12 | Brief summary           | **Final approval** before building            | Full brief                     |

Key point: the palette, font, and style options are **not fixed lists** — Deus designs the candidates from earlier answers. Two different briefs are never offered the exact same options.

After final approval → the build runs → the audit report → handoff with a preview.

---

## 4. Plugin Architecture

Structure follows the official Claude Code plugin spec (verified — see §8):

```
deus/
├── .claude-plugin/
│   └── plugin.json               # manifest: name "deus", version, description
├── skills/
│   ├── build/
│   │   ├── SKILL.md              # main wizard (kept < 500 lines)
│   │   └── references/
│   │       ├── wizard-flow.md    # step order, option & preview rules
│   │       ├── style-presets.md  # deep spec of each UI/UX preset
│   │       ├── palette.md        # palette design system + contrast rules
│   │       ├── typography.md     # per-personality font-pairing library
│   │       ├── motion.md         # animation vocabulary: duration, easing, patterns
│   │       └── content.md        # copywriting formulas per section type
│   ├── status/SKILL.md           # show brief & progress
│   ├── revise/SKILL.md           # change one part, approval flow
│   ├── page/SKILL.md             # add a consistent new page
│   ├── rebrand/SKILL.md          # swap the theme via tokens
│   ├── content/SKILL.md          # update copywriting
│   ├── preview/SKILL.md          # run locally + screenshot
│   ├── audit/SKILL.md            # full QA sweep
│   └── deploy/SKILL.md           # publish
├── agents/
│   ├── deus-architect.md
│   ├── deus-designer.md
│   ├── deus-writer.md
│   ├── deus-motion.md
│   ├── deus-seo.md
│   └── deus-qa.md
├── hooks/
│   └── hooks.json                # optional (M5): validator & brief loader
├── scripts/
│   ├── validate.sh               # quick HTML/link check of the build
│   └── screenshot.sh             # helper for /deus:preview
└── templates/                    # starter skeletons per stack
```

**Key architecture decisions:**

- **The wizard runs in the main conversation; the build is delegated to agents.** The interactive question tool (AskUserQuestion) is not available inside subagents — so the `build` skill does NOT use `context: fork`. Only after the brief is finalized are the agents invoked in parallel.
- **The brief is the source of truth.** Every project Deus builds gets a `deus/` folder holding `brief.json` (all decisions) and `design-system.css` (tokens). Every post-build skill reads these — that's why revisions never break consistency.
- **Cross-project preferences** are stored in `${CLAUDE_PLUGIN_DATA}` (the plugin's permanent data folder) — Deus v2 could say "you usually pick Astro + a minimal style, same again?"

### Commands

| Command                 | Function                                                            |
|-------------------------|---------------------------------------------------------------------|
| `/deus:build <name>`    | Full wizard → build a website from scratch                          |
| `/deus:status`          | Show the active project's brief & progress                          |
| `/deus:revise <part>`   | Change one part (with an approval flow) without breaking the rest    |
| `/deus:page <name>`     | Add a new page consistent with the design system                    |
| `/deus:rebrand`         | Swap colors/fonts/style via tokens — the whole site follows          |
| `/deus:content`         | Update copywriting per section                                      |
| `/deus:preview`         | Run locally + screenshot every page                                 |
| `/deus:audit`           | Full QA: responsive, a11y, SEO, performance, links                  |
| `/deus:deploy`          | Publish to Vercel / Netlify / GitHub Pages                          |

### The Agent Crew

| Agent            | Expertise (deep, not surface)                                            |
|------------------|--------------------------------------------------------------------------|
| `deus-architect` | Information structure: sitemap, section order, visitor flow → conversion  |
| `deus-designer`  | Design system: color/type/spacing tokens, components, layout              |
| `deus-writer`    | Real copywriting: headlines, CTAs, section body — following the brief's tone |
| `deus-motion`    | Animation system: duration, easing, reveal, micro-interaction, reduced-motion |
| `deus-seo`       | Meta, Open Graph, sitemap.xml, structured data, semantic HTML             |
| `deus-qa`        | Adversarial tester: hunts responsive/a11y/performance/link/SEO defects    |

---

## 5. Milestones (M0 → M7)

### M0 — Foundation ✅ *(done 18 August 2026)*
A valid, loadable plugin scaffold.
- Folder structure + `.claude-plugin/plugin.json` (name: `deus`)
- One trial skill to prove the load path
- Dev loop: `claude --plugin-dir ./deus` + `/reload-plugins`

**Done when:** `claude plugin validate ./deus --strict` is green; `/deus:build` shows in the slash-command menu.

### M1 — Wizard Engine *(± 1–2 sessions)*
The heart of the approve-one-by-one experience.
- The `build` skill + all `references/` files (flow, presets, palette, typography, motion, content)
- Rules for designing dynamic options per step (not static lists) + previews
- Output: a clean, complete `deus/brief.json`
- Summary step + final approval

**Done when:** the full 12-step wizard runs (without building), producing a consistent brief.json for 3 test scenarios: company profile, personal portfolio, store.

### M2 — Design-System Engine *(± 1 session)*
The translator from brief → visual system.
- `design-system.css` generator: color tokens (light+dark), type scale, spacing, radius, shadow
- 4 UI/UX presets defined **deeply**: component anatomy, layout rules, character of each preset
- Motion vocabulary: durations, easing, reveal patterns, `prefers-reduced-motion` rules
- Built-in color contrast & accessibility rules

**Done when:** from one brief.json, a full design system generates and is documented in the project folder.

### M3 — Build Engine *(± 2–3 sessions)*
From design system → a full website. Start with **one stack** (modern static) so quality is deep; other stacks follow.
- Page + component generator per style preset
- Real copywriting for every section (via `content.md` rules)
- Assets: placeholder logo SVG, icons, image strategy
- Fully responsive (mobile-first), semantic HTML
- Technical SEO built in: meta, OG, sitemap.xml, structured data

**Done when:** `/deus:build` produces one complete business website from name → wizard → done, with no manual intervention.

### M4 — The Agent Crew *(± 1–2 sessions)*
Per-domain depth + parallel speed.
- Write 6 agent definitions (`agents/*.md`) with sharp specializations
- Orchestration: the `build` skill delegates the build to agents in parallel
- `deus-qa` runs mandatorily at the end; the audit result is attached to the handoff

**Done when:** the M3 build is executed by the agent crew; an audit report accompanies every handoff.

### M5 — Post-Build Toolkit *(± 2 sessions)*
A living website.
- Skills: `status`, `revise`, `page`, `rebrand`, `content`, `preview`, `audit`, `deploy`
- All read `deus/brief.json` — consistency is preserved automatically
- Optional hooks: `SessionStart` loads the brief summary when a Deus project folder is detected; a light post-edit validator
- `deploy`: detect the available CLI (Vercel/Netlify/gh-pages), guide it live

**Done when:** the full cycle is proven — build → revise 1 section → add 1 page → rebrand → audit → deploy, all consistent.

### M6 — Packaging & Distribution *(± 1 session)*
From a local folder → a plugin anyone can install.
- README + usage docs
- Semver versioning + changelog
- GitHub repo + `marketplace.json`
- Clean-install test: `/plugin marketplace add Kstriabintang/deus-factory` → `/plugin install deus@deus-factory`

> **Early progress (21 August 2026):** the public repo + marketplace are already live at [github.com/Kstriabintang/deus-factory](https://github.com/Kstriabintang/deus-factory) — install: `/plugin marketplace add Kstriabintang/deus-factory` → `/plugin install deus@deus-factory`. Remaining M6: clean-install verification, first real-site screenshot. (Changelog added in 0.1.1 — see [CHANGELOG.md](CHANGELOG.md).)

**Done when:** a marketplace install on a clean machine/session succeeds; validate --strict is green.

### M7 — v2 Evolution *(ongoing)*
- Cross-project preference memory (`${CLAUDE_PLUGIN_DATA}`)
- Additional stacks: full Next.js, Astro content collections
- Modules: blog, store (e-commerce), CMS integration, forms + a light backend
- Industry templates (restaurant, agency, clinic, etc.) as wizard accelerators
- Full bilingual (i18n) + analytics integration

---

## 6. Overall Estimate

± **8–11 work sessions** through M6 (a complete, distributed plugin). The first working website is born at the end of **M3**.

---

## 7. Risks & Mitigations

| Risk                                                    | Mitigation                                                              |
|----------------------------------------------------------|------------------------------------------------------------------------|
| Max 4 options per question (tool limit)                   | Curate the best 4 per brief context; "Other" is always available        |
| Preview only for single-select questions                  | Multi-select steps (page structure) use rich descriptions               |
| Question tool unavailable in subagents                    | Wizard in the main conversation; only the build is delegated to agents   |
| SKILL.md bloat (> 500 lines)                              | Split into `references/`; SKILL.md holds only the core flow             |
| Scope too large at once                                   | One stack deep first (M3), other stacks after it's stable               |
| Inconsistent quality across commands                      | Every command must read the brief; QA mandatory before handoff          |
| `claude plugin eval` still early access                   | Manual testing via M1/M3/M5 scenarios; adopt official eval when ready    |

---

## 8. Verified Technical Facts (official Claude Code docs)

- The manifest is required at `.claude-plugin/plugin.json`; only `name` is required (kebab-case, becomes the namespace prefix).
- Skill: `skills/<name>/SKILL.md` → invoked as `/deus:<name>`. Agent: `agents/<name>.md` → `@deus:<name>`.
- Skill frontmatter supports: `description`, `argument-hint`, `allowed-tools`, `context: fork` (avoided for the wizard), `model`, `effort`, etc.
- Available placeholders: `$ARGUMENTS`, `${CLAUDE_PLUGIN_ROOT}` (plugin folder), `${CLAUDE_PLUGIN_DATA}` (permanent data), `${CLAUDE_PROJECT_DIR}`.
- Local dev: `claude --plugin-dir ./deus`; skill/agent/hook changes just need `/reload-plugins` (MCP needs a restart).
- Validation: `claude plugin validate ./deus --strict`.
- Distribution: `marketplace.json` in the repo → `/plugin marketplace add` → `/plugin install deus@<marketplace>`.
- AskUserQuestion: 1–4 questions per call, 2–4 options per question, `multiSelect` supported, preview for single-select; **not available in subagents**.
- Available hooks: `SessionStart`, `UserPromptSubmit`, `PreToolUse`, `PostToolUse`, `Stop`, `SubagentStop`, `FileChanged`, etc. via `hooks/hooks.json`.

Source: code.claude.com/docs — plugins, plugins-reference, skills, sub-agents, hooks, plugin-marketplaces.

---

## 9. Next Step

**M0 done** (18 Aug 2026): `deus/` stands, `claude plugin validate --strict` green, `deus:build` proven registered via a `--plugin-dir` smoke test. Public repo + marketplace live (21 Aug 2026).

**Execute M1 — Wizard Engine:** enable all 12 steps + write every `references/` file (style presets, palette, typography, motion, content) to full depth. Just say **"go M1"**. 🔱
