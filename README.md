# Karpathy-Inspired Coding Agent Guidelines

> **Fork & Extension Notice:**  
> This project is built on top of [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) (by [@jiayuan_jy](https://x.com/jiayuan_jy) / [Multica](https://github.com/multica-ai/multica)), derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls.  
> We expanded the original Claude Code guidelines into a universal, multi-assistant toolkit with first-class support for **Google Antigravity**, **OpenAI Codex**, **Cursor**, and **Claude Code**.

Actionable behavioral guidelines to reduce common AI coding mistakes, eliminate overcomplication, ensure surgical diffs, and enable test-driven execution loops.

English | [简体中文](./README.zh.md)

---

## Supported Coding Assistants

| Assistant | Rule / Instruction File | Skill Format | Integration Guide |
|---|---|---|---|
| **Google Antigravity** | [`GEMINI.md`](GEMINI.md) / [`AGENTS.md`](AGENTS.md) | `.agents/skills/karpathy-guidelines/` | [ANTIGRAVITY.md](ANTIGRAVITY.md) |
| **OpenAI Codex** | [`AGENTS.md`](AGENTS.md) | `.agents/skills/karpathy-guidelines/` | [CODEX.md](CODEX.md) |
| **Claude Code** | [`CLAUDE.md`](CLAUDE.md) | Plugin / Marketplace | [Claude Code Setup](#1-claude-code) |
| **Cursor** | [`.cursor/rules/`](.cursor/rules/karpathy-guidelines.mdc) | `~/.cursor/skills/` | [CURSOR.md](CURSOR.md) |

---

## The Problems

From Andrej Karpathy's post:

> "The models make wrong assumptions on your behalf and just run along with them without checking. They don't manage their confusion, don't seek clarifications, don't surface inconsistencies, don't present tradeoffs, don't push back when they should."

> "They really like to overcomplicate code and APIs, bloat abstractions, don't clean up dead code... implement a bloated construction over 1000 lines when 100 would do."

> "They still sometimes change/remove comments and code they don't sufficiently understand as side effects, even if orthogonal to the task."

## The Solution

Four principles in a single unified specification that directly address these issues:

| Principle | Addresses |
|-----------|-----------|
| **1. Think Before Coding** | Wrong assumptions, hidden confusion, missing tradeoffs |
| **2. Simplicity First** | Overcomplication, bloated abstractions |
| **3. Surgical Changes** | Orthogonal edits, touching code you shouldn't |
| **4. Goal-Driven Execution** | Leverage through tests-first, verifiable success criteria |

---

## The Four Principles in Detail

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

LLMs often pick an interpretation silently and run with it. This principle forces explicit reasoning:

- **State assumptions explicitly** — If uncertain, ask rather than guess.
- **Present multiple interpretations** — Don't pick silently when ambiguity exists.
- **Push back when warranted** — If a simpler approach exists, say so.
- **Stop when confused** — Name what's unclear and ask for clarification.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

Combat the tendency toward overengineering:

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If 200 lines could be 50, rewrite it.

**The test:** Would a senior engineer say this is overcomplicated? If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:

- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:

- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

**The test:** Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform imperative tasks into verifiable goals:

| Instead of... | Transform to... |
|--------------|-----------------|
| "Add validation" | "Write tests for invalid inputs, then make them pass" |
| "Fix the bug" | "Write a test that reproduces it, then make it pass" |
| "Refactor X" | "Ensure tests pass before and after" |

For multi-step tasks, state a brief plan:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let the LLM loop independently. Weak criteria ("make it work") require constant clarification.

---

## Install & Usage by Assistant

### 1. Claude Code

**Option A: Claude Code Plugin (Recommended)**
```bash
/plugin marketplace add longhakly/andrej-karpathy-skills
/plugin install andrej-karpathy-skills@karpathy-skills
```

**Option B: CLAUDE.md (Per-project)**
```bash
# New project:
curl -o CLAUDE.md https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/CLAUDE.md

# Existing project (append):
echo "" >> CLAUDE.md && curl https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/CLAUDE.md >> CLAUDE.md
```

---

### 2. OpenAI Codex

Codex automatically reads instructions from `AGENTS.md` at your workspace root.

**Per-Project Instructions:**
```bash
curl -o AGENTS.md https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/AGENTS.md
```

**Per-Project Skill (On-Demand):**
```bash
mkdir -p .agents/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o .agents/skills/karpathy-guidelines/SKILL.md
```

**Global (All Codex Sessions):**
```bash
mkdir -p ~/.codex && curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/AGENTS.md >> ~/.codex/instructions.md
```
See **[CODEX.md](CODEX.md)** for full details.

---

### 3. Google Antigravity

Antigravity natively discovers `GEMINI.md`, `AGENTS.md`, and skills under `.agents/skills/`.

**Option A: Project Rule (Always Active)**
```bash
curl -o GEMINI.md https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/GEMINI.md
```

**Option B: Project Skill (On-Demand Progressive Disclosure)**
```bash
mkdir -p .agents/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o .agents/skills/karpathy-guidelines/SKILL.md
```

**Option C: Global Antigravity Skill (All Projects on Machine)**
```bash
mkdir -p ~/.gemini/config/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o ~/.gemini/config/skills/karpathy-guidelines/SKILL.md
```
See **[ANTIGRAVITY.md](ANTIGRAVITY.md)** for full details.

---

### 4. Cursor

This repository includes a committed Cursor project rule ([`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc)). 

To use in another Cursor project:
```bash
mkdir -p .cursor/rules
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.cursor/rules/karpathy-guidelines.mdc \
  -o .cursor/rules/karpathy-guidelines.mdc
```
See **[CURSOR.md](CURSOR.md)** for details.

---

## Key Insight

From Andrej Karpathy:

> "LLMs are exceptionally good at looping until they meet specific goals... Don't tell it what to do, give it success criteria and watch it go."

The "Goal-Driven Execution" principle captures this: transform imperative instructions into declarative goals with verification loops.

## How to Know It's Working

These guidelines are working if you see:

- **Fewer unnecessary changes in diffs** — Only requested changes appear
- **Fewer rewrites due to overcomplication** — Code is simple the first time
- **Clarifying questions come before implementation** — Not after mistakes
- **Clean, minimal PRs** — No drive-by refactoring or "improvements"

## Customization

These guidelines are designed to be merged with project-specific instructions. Add them to your existing `AGENTS.md`, `GEMINI.md`, or `CLAUDE.md`.

For project-specific rules, add sections like:

```markdown
## Project-Specific Guidelines

- Use TypeScript strict mode
- All API endpoints must have tests
- Follow the existing error handling patterns in `src/utils/errors.ts`
```

## Tradeoff Note

These guidelines bias toward **caution over speed**. For trivial tasks (simple typo fixes, obvious one-liners), use judgment — not every change needs the full rigor.

The goal is reducing costly mistakes on non-trivial work, not slowing down simple tasks.

## Examples

See **[EXAMPLES.md](EXAMPLES.md)** for real-world before/after code walkthroughs illustrating each principle.

## License

MIT
