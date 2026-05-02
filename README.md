# skills-repo

A starter template for building your own [Agent Skills](https://agentskills.io/) library and serving it through [skillsovermcp.com](https://skillsovermcp.com) as an MCP server.

> Conforms to the [Agent Skills specification](https://agentskills.io/specification) and [SEP-2640](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2640) (Skills Extension, Extensions Track) — the [Skills Over MCP Working Group](https://modelcontextprotocol.io/community/skills-over-mcp/charter)'s direction for serving skills over MCP via the Resources primitive.

## What is this?

A **skill** is a directory with a `SKILL.md` file: a markdown playbook that teaches an agent how to do one task. Drop the right folder into this repo and the agent knows how to review a PR, triage a bug, query your database, humanize your writing, or whatever you need.

Drop a folder with a `SKILL.md` into this repo and it becomes available to any MCP-aware agent the moment you point it at:

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
├── README.md
├── LICENSE
└── skills/
    ├── review-pr/
    │   ├── SKILL.md          ← required: frontmatter + body
    │   └── references/       ← optional: deep-dive content
    │       └── checklist.md
    ├── triage-bug/
    │   └── SKILL.md
    ├── write-like-a-human/
    │   └── SKILL.md
    ├── ask-database/
    │   └── SKILL.md
    ├── suggesting-skills/
    │   └── SKILL.md
    └── grill-me/
        └── SKILL.md
```

The MCP loader is layout-agnostic: SKILL.md files can live anywhere — top-level, under `skills/`, deeply nested under category folders. Whatever you prefer.

## Anatomy of a SKILL.md

Per the [Agent Skills specification](https://agentskills.io/specification#skillmd-format):

```markdown
---
name: triage-bug
description: Take a raw bug report and turn it into a clean, prioritized ticket with a title, repro steps, and severity. Use when the user says "triage this", pastes a support escalation, or asks "what's this bug".
license: MIT
---

# Triage Bug

Convert a noisy bug report into a ticket someone can act on.

1. Restate the symptom in one sentence as a user would say it.
2. Pull out numbered repro steps.
3. Note expected vs actual.
4. Assign a severity (S1–S4) and an area.
5. Output the ticket in Markdown.
```

Required frontmatter:

| Field | Required | Constraint |
|-------|----------|------------|
| `name` | Yes | 1-64 chars · lowercase letters, digits, hyphens · no leading/trailing/double hyphens · **must match the parent directory name** |
| `description` | Yes | 1-1024 chars · describe both **what** the skill does and **when** to use it (keywords help routing) |

Optional frontmatter (see [the spec](https://agentskills.io/specification)): `license`, `compatibility`, `metadata`, `allowed-tools`.

## Progressive disclosure

Skills are loaded in three tiers — keep this in mind when authoring:

1. **Metadata (~100 tokens)**: `name` and `description` load at startup for every skill. This is what the agent uses to decide whether to activate the skill.
2. **Instructions (≤ ~5000 tokens)**: the full `SKILL.md` body loads when the skill is activated.
3. **Resources (on demand)**: files in `scripts/`, `references/`, or `assets/` load only when the body explicitly references them.

Aim for a `SKILL.md` body under 500 lines. Move detailed reference material to `references/`.

## How this template is served over MCP

When you paste your repo URL into [skillsovermcp.com](https://skillsovermcp.com), the service:

1. Walks the GitHub tree on demand to find every `SKILL.md`.
2. Exposes each skill across **four MCP primitives** so it's discoverable by both today's and future clients:
   - **Tool** `skill_<name>` — body of the SKILL.md (works on every MCP client today).
   - **Prompt** `<name>` — slash-command UX for Claude Code / Cursor.
   - **Resource** `skill://<owner>/<repo>/<name>/<file>` — [SEP-2640](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2640) compliant URI.
   - **Resource** `skill://index.json` — well-known [Agent Skills discovery index](https://agentskills.io/well-known-uri).
   - The server declares the `io.modelcontextprotocol/skills` capability so spec-aware hosts can discover the layout.

When clients adopt SEP-2640 natively, the per-skill tools become redundant — but until then they keep the skills usable everywhere.

## Tips

- Keep skills **small and single-purpose**. One skill = one task.
- Write in **imperative voice** ("Run X. Then do Y."). Agents follow instructions better than they follow advice.
- Front-load the **`description`** with keywords the user will say. That's how the agent decides to activate the skill.
- Add **supporting files** (templates, examples, scripts) alongside the SKILL.md and reference them by relative path.
- Skills are **public** — anything in this repo is visible to anyone who points an MCP client at your URL.

## Validation

You can validate any skill against the spec with [`skills-ref`](https://github.com/agentskills/agentskills/tree/main/skills-ref):

```bash
skills-ref validate ./skills/triage-bug
```

## Resources

- [Agent Skills specification](https://agentskills.io/specification) — the canonical SKILL.md format.
- [Skills Over MCP charter](https://modelcontextprotocol.io/community/skills-over-mcp/charter) — the MCP Working Group governing this direction.
- [SEP-2640: Skills Extension](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/2640) — Resources-based protocol binding.
- [experimental-ext-skills](https://github.com/modelcontextprotocol/experimental-ext-skills) — WG incubation repo, decision log, findings.

## License

MIT — fork it, ship it, make it yours.
