# Using this repo with OpenAI Codex

This project includes configuration and skills so the Karpathy-inspired behavioral guidelines apply automatically when you work with **OpenAI Codex** (Codex CLI and Codex extension).

## In this repository

Codex automatically discovers instructions in this repository:
- **Project Instructions:** `AGENTS.md` is committed at the repository root. OpenAI Codex natively loads `AGENTS.md` into the system prompt for all runs in this workspace.
- **Agent Skills:** `.agents/skills/karpathy-guidelines/SKILL.md` is committed under `.agents/skills/`, matching the Agent Skills specification used by Codex.

No extra installation steps are required when opening this repository with Codex.

---

## Use the same guidelines in another project

### Option A: Project Instructions (Recommended - Always Active)
Copy `AGENTS.md` into your target project's root:
```bash
curl -o AGENTS.md https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/AGENTS.md
```
Codex will automatically load these guidelines for every turn in that workspace.

### Option B: Project Skill (On-Demand Activation)
Copy the skill into your project's `.agents/skills/` directory:
```bash
mkdir -p .agents/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o .agents/skills/karpathy-guidelines/SKILL.md
```
Codex will list the skill and dynamically activate it during coding, debugging, or reviewing workflows.

---

## Global installation (Apply to all Codex sessions)

To apply these guidelines across all your Codex workspaces on this machine:

### Global Instructions:
Append to your global Codex instructions file (`~/.codex/instructions.md`):
```bash
mkdir -p ~/.codex
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/AGENTS.md >> ~/.codex/instructions.md
```

### Global Skill:
```bash
mkdir -p ~/.codex/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o ~/.codex/skills/karpathy-guidelines/SKILL.md
```

---

## For contributors

When updating the four principles, keep all entry points in sync:
- `AGENTS.md` (OpenAI Codex & universal agent standard)
- `GEMINI.md` & `ANTIGRAVITY.md` (Google Antigravity)
- `CLAUDE.md` (Claude Code)
- `.cursor/rules/karpathy-guidelines.mdc` & `CURSOR.md` (Cursor)
- `.agents/skills/karpathy-guidelines/SKILL.md` & `skills/karpathy-guidelines/SKILL.md` (Agent Skills)
