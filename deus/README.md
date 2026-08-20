# Deus 🔱

The `deus` plugin for Claude Code — an all-in-one website factory. **One input: a website name.** Everything else is a curated decision you approve one by one — then Deus builds a company-grade website and maintains it for the life of the project.

**Version:** 0.1.0 (M0 — Foundation) · **Full plan:** [`../ROADMAP.md`](../ROADMAP.md)

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
cd "/Users/macbookpro/Documents/Revan Files/GITHUB-ME/Claude Powerfull"
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
