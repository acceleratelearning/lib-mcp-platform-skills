# skills-repo

A starter template for building your own **Claude Code / agent skills** repository and serving it through [skillsovermcp.com](https://skillsovermcp.com) as an MCP server.

## What is this?

Skills are tiny markdown playbooks (`SKILL.md`) that teach an agent how to do a specific task — review a PR, ship a changelog, scaffold a route, run a discovery call, whatever you do every week.

Drop a folder with a `SKILL.md` inside this repo and it becomes available to any MCP-aware agent the moment you point it at:

```
https://skillsovermcp.com/mcp/<your-username>/<this-repo>
```

No deploys. No build step. The skillsovermcp service reads the repo on demand.

## Quick start

1. **Use this template** → click the green "Use this template" button on GitHub.
2. **Edit, add, or remove** the example skills under `skills/`.
3. **Paste your repo URL** at [skillsovermcp.com](https://skillsovermcp.com) to get your personal MCP URL.
4. **Add the URL** to Claude Code, Cursor, Codex, or any other MCP client.

## Layout

```
skills-repo/
├── README.md                  ← you are here
├── LICENSE
└── skills/
    ├── ship-changelog/
    │   └── SKILL.md          ← required: the skill body
    ├── review-pr/
    │   ├── SKILL.md
    │   └── references/       ← optional supporting files
    │       └── checklist.md
    └── scaffold-route/
        └── SKILL.md
```

The loader is layout-agnostic: SKILL.md files can live anywhere — top level, under `skills/`, deeply nested under category folders. Whatever you prefer.

## Anatomy of a SKILL.md

Each skill is a single markdown file with optional YAML frontmatter:

```markdown
---
name: ship-changelog
description: Turn a week of commits into a customer-ready release note.
---

# Ship Changelog

When the user asks for a changelog or release notes:

1. Run `git log --since='last sunday' --oneline` and group commits by area.
2. Write one bullet per user-visible change in plain English.
3. Lead with the change, not the implementation detail.
4. Skip refactors, internal renames, and dependency bumps.
5. Output as Markdown ready to paste into Linear / GitHub Releases.
```

The `name` and `description` are surfaced to the agent so it knows when to invoke the skill. The body is the actual playbook the agent follows.

## Tips

- Keep skills **small** (1-2 screens). One skill = one task.
- Write in **imperative voice** ("Run X. Then do Y.") — agents follow instructions better than they follow advice.
- Add **supporting files** (templates, examples, scripts) alongside the SKILL.md and reference them by relative path.
- Skills are **public** — anything in this repo is visible to anyone who points an MCP client at your URL.

## License

MIT — fork it, ship it, make it yours.
