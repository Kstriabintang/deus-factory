# Changelog

All notable changes to Deus are documented here. Versions follow [Semantic Versioning](https://semver.org/).

## 0.1.1 — 2026-10-17

### Changed

- Descriptions now match what v0 actually does: the wizard produces a project brief (`deus/brief.json`), and the website build engine is still on the roadmap. Updated in the README, the plugin and marketplace manifests, and the banner.
- Skill frontmatter: added `name`; the description now states when to use the skill.
- The wizard spec is referenced by a relative path (`references/wizard-flow.md`).

### Fixed (found by scenario testing)

- Falls back to a numbered list when the AskUserQuestion tool is not available.
- Industry options are designed from the website name as well as the purpose.
- Fixed-option steps (Purpose, Personality) keep their labels and tailor their descriptions; other steps design their options.
- Brief values: option labels are stored lowercase, "Other" input verbatim.
- The color step states WCAG AA thresholds: ink on surface ≥ 4.5:1; accent on surface ≥ 3:1 for large text and UI shapes, and 4.5:1 for normal-size text in or on the accent.
- Questions show their progress as (N/4), which is short and language-neutral.
- Project confirmation lists its options, places the folder in the current directory, and checks for an existing brief.
- Language: a user who typed only the command gets the language of their earlier messages or project instructions; option labels are shown in that language but stored in English.
- Clarified the brief schema (pending steps vs nested fields, industry sector and audience, the version field) and how renaming works in the confirmation step.
- Removed a local machine path from the plugin README; added the one-step install command.

## 0.1.0 — 2026-08-21

- First release (milestone M0): plugin scaffold and the v0 wizard — purpose, industry, personality and color direction → `deus/brief.json`.
