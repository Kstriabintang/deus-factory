# Deus 🔱

The `deus` plugin for Claude Code. **One input: a website name.** Everything else is a curated decision you approve one by one. Today Deus turns those decisions into a project brief (`deus/brief.json`); building the website itself is the goal of the roadmap (M3 onward).

**Version:** 0.1.1 (M0 — Foundation) · **Full plan:** [`../ROADMAP.md`](../ROADMAP.md)

## Install

Inside Claude Code 2.1.275 or later:

```
/plugin install deus --marketplace Kstriabintang/deus-factory
```

On any version:

```
/plugin marketplace add Kstriabintang/deus-factory
/plugin install deus@deus-factory
```

## Milestone status

- [x] **M0 — Foundation**: valid scaffold, `build` skill v0 (partial wizard → brief.json)
- [ ] M1 — Full wizard engine (12 steps)
- [ ] M2 — Design-system engine (brief → tokens)
- [ ] M3 — Build engine (first complete website)
- [ ] M4 — Crew of 6 specialist agents
- [ ] M5 — Post-build toolkit (revise/rebrand/audit/deploy)
- [ ] M6 — Packaging & marketplace

## Usage (development mode)

```bash
cd path/to/deus-factory
claude --plugin-dir ./deus
```

Then, inside the session:

```
/deus:build YourWebsiteName
```

The v0 wizard walks you through: purpose → industry → personality → color direction → producing `deus/brief.json` in your website project's folder.

## Development loop

- Change a skill/agent/hook → type `/reload-plugins` in the session (no restart).
- Validate after any structural change:

```bash
claude plugin validate ./deus --strict
```

## Structure

```
deus/
├── .claude-plugin/plugin.json    # manifest (name: deus)
└── skills/
    └── build/
        ├── SKILL.md              # wizard v0 — approve-one-by-one flow
        └── references/
            └── wizard-flow.md    # 12-step constitution + brief.json schema
```

The `agents/`, `hooks/`, `scripts/`, and `templates/` folders are filled at their respective milestones — structure follows [ROADMAP.md](../ROADMAP.md).
