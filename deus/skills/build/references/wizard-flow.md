# Deus Wizard Constitution — 12-Step Specification

This document is the source of truth for the Deus wizard flow. The skill reads it before starting.
Implementation status: **v0 (M0)** runs steps 1, 2, 3, and 5-lite. The rest are enabled in **M1**.

## Philosophy

1. The user gives ONE input: a website name. Everything else is options they approve one by one.
2. Every approval becomes a field in `brief.json` — the source of truth for the life of the project.
3. Options are designed from the answers to earlier steps. Fixed-option steps (1 and 3) keep their labels but tailor the descriptions.
4. At most 4 options per question (tool limit); "Other" is always available and fully honored.
5. Preview is used on every single-select step that has a visual/textual form.
6. One decision per turn. Never combine two steps.

## The 12 Steps

| #  | Step                 | Form          | Option curation rule                                            | Preview                          | Brief field          |
|----|----------------------|---------------|-----------------------------------------------------------------|----------------------------------|----------------------|
| 1  | Purpose              | single-select | Fixed: Business / Personal / Organization / Store               | —                                | `purpose`            |
| 2  | Industry & audience  | single-select | 4 options from the purpose and the website name; name the audience | —                          | `industry`           |
| 3  | Brand personality    | single-select | Luxurious / Professional / Warm / Bold, described in context     | —                                | `personality`        |
| 4  | Tech stack           | single-select | Plain HTML+CSS+JS / Astro / Next.js — explain the trade-off      | Project structure snippet        | `stack`              |
| 5  | Color palette        | single-select | 4 palettes designed from steps 1–3; WCAG AA: ink/surface ≥ 4.5:1, accent/surface ≥ 3:1 (4.5:1 for normal-size text in or on the accent) | Swatch: color roles + hex        | `colors`             |
| 6  | Typography           | single-select | Display+body pairing from a per-personality library (Google Fonts) | Type specimen                  | `typography`         |
| 7  | UI/UX style          | single-select | Preset: corporate premium / minimal elegant / bold modern / classic editorial | ASCII hero+section mockup | `ui_style`   |
| 8  | Motion               | single-select | Calm / Lively / Cinematic — explain the concrete pattern         | Animation pattern description    | `motion`             |
| 9  | Page structure       | multi-select  | Candidate pages for the purpose+industry (Home always present)   | — (multi-select, no preview)     | `pages[]`            |
| 10 | Sections per page    | single-select per page | Recommended section order per page, approved per page  | Section-order sketch             | `pages[].sections`   |
| 11 | Content language & tone | single-select | EN / ID / Bilingual; tone derived from personality           | 1–2 sample copy sentences        | `content`            |
| 12 | Brief summary        | single-select | Final approval / Change one step                                | Full brief                       | `status`             |

Additional rules:

- **Step 12** offers revision: if the user chooses to change one step, repeat only that step, then return to the summary.
- **After final approval** (from M3 on): delegate the build to the specialist agent crew; the wizard stays in the main conversation because AskUserQuestion is not available inside subagents.
- If the user picks "Other", record the raw text in the brief and redesign the next step's options based on it.

## `deus/brief.json` Schema

Location: `<project-folder>/deus/brief.json`. Filled example (v0 — pending fields are null):

```json
{
  "deus": "0.1.1",
  "created": "2026-08-21",
  "name": "Aurora Coffee",
  "folder": "aurora-coffee",
  "purpose": "business",
  "industry": {
    "sector": "coffee shop",
    "audience": "urban workers 20-35 looking for a relaxed place to work"
  },
  "personality": "warm",
  "stack": null,
  "colors": {
    "direction": "earthy dusk",
    "roles": { "surface": "#F6EFE6", "ink": "#2B2118", "accent": "#C46A2B" },
    "full_palette": null
  },
  "typography": null,
  "ui_style": null,
  "motion": null,
  "pages": null,
  "content": { "language": null, "tone": null },
  "status": {
    "wizard": "v0-partial",
    "final_approved": false,
    "pending": ["stack", "typography", "ui_style", "motion", "pages", "content"]
  }
}
```

Schema rules:

- Approved option labels are stored lowercase, as in the example (`colors.direction` is the chosen palette's name); "Other" input is stored **verbatim**.
- Steps not yet asked are listed in `status.pending` (never deleted) and their fields hold `null` — inside the object when the field is an object (e.g. `content.language`). `colors.full_palette` stays `null` until M2 and is not a pending step.
- `industry.sector` is the chosen option's label; `industry.audience` is the audience named in its description. For an "Other" answer, store the text verbatim in `industry.sector` and the audience phrase it contains, if any, copied as written, in `industry.audience`.
- `deus` is the Deus version that wrote the brief (the value in this example).
- `status.final_approved` is only `true` after step 12 is approved — a hard requirement before building (M3).
- The brief is never blindly overwritten: if one exists, ask whether to continue or start over.

## Companion references (authored in M1–M2)

- `style-presets.md` — anatomy of the 4 UI/UX presets: components, layout, character.
- `palette.md` — palette design system + contrast rules.
- `typography.md` — per-personality font-pairing library.
- `motion.md` — animation vocabulary: durations, easing, reveal patterns.
- `content.md` — copywriting formulas per section type.
