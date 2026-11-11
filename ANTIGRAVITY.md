# Using this repo with Google Antigravity

This project includes configuration and skills so the Karpathy-inspired behavioral guidelines apply automatically when you work with **Google Antigravity** (AGY CLI and Antigravity IDE).

## In this repository

Antigravity automatically discovers customizations in this repository:
- **Project Rule:** `GEMINI.md` and `AGENTS.md` are committed at the repository root. Antigravity discovers and loads these instructions automatically for all prompts in this workspace.
- **Project Skill:** `.agents/skills/karpathy-guidelines/SKILL.md` is committed under `.agents/skills/`, making the skill discoverable and activated on-demand.

No extra installation steps are required when opening this repository in Antigravity.

---

## Use the same guidelines in another project

### Option A: Project Rule (Always Active)
Copy `GEMINI.md` (or `AGENTS.md`) into your target project's root:
```bash
curl -o GEMINI.md https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/GEMINI.md
```
Antigravity will load these guidelines into the system prompt for every turn.

### Option B: Project Skill (On-Demand Progressive Disclosure)
Copy the skill folder into your project's `.agents/skills/` directory:
```bash
mkdir -p .agents/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o .agents/skills/karpathy-guidelines/SKILL.md
```
Antigravity will register the skill in its available skills list and inject its full content only when writing, refactoring, or reviewing code.

---

## Global installation (Apply to all your projects)

To enable Karpathy guidelines globally across all Antigravity workspaces on your machine:

### Global Skill:
```bash
mkdir -p ~/.gemini/config/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o ~/.gemini/config/skills/karpathy-guidelines/SKILL.md
```

### Global Rule:
```bash
mkdir -p ~/.gemini/config/rules
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/GEMINI.md \
  -o ~/.gemini/config/rules/karpathy-guidelines.md
```

---

## For contributors

When updating the four principles, keep all entry points in sync:
- `CLAUDE.md` (Claude Code)
- `.cursor/rules/karpathy-guidelines.mdc` (Cursor)
- `CODEX.md` (OpenAI Codex)
- `GEMINI.md` & `AGENTS.md` (Google Antigravity & Universal Agents)
- `skills/karpathy-guidelines/SKILL.md` & `.agents/skills/karpathy-guidelines/SKILL.md`
