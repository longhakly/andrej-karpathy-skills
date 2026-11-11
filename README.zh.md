# 受 Karpathy 启发的编码智能体行为准则

> **Fork 与扩展说明：**  
> 本项目是基于 [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)（由 [@jiayuan_jy](https://x.com/jiayuan_jy) / [Multica](https://github.com/multica-ai/multica) 创建）构建的 Fork 增强版，源自 [Andrej Karpathy 总结的](https://x.com/karpathy/status/2015883857489522876) LLM 编程常见陷阱与观察。  
> 我们将最初仅针对 Claude Code 的规则拓展为支持多智能体的统一规范，原生支持 **Google Antigravity**、**OpenAI Codex**、**Cursor** 以及 **Claude Code**。

切实可行的行为准则，用于减少常见的 AI 编程错误、杜绝过度复杂化、保证精准代码改动（Surgical Diffs），并通过测试驱动目标独立闭环。

[English](./README.md) | 简体中文

---

## 支持的编码智能体

| 智能体 | 规则/指令文件 | 技能格式 | 集成指引 |
|---|---|---|---|
| **Google Antigravity** | [`GEMINI.md`](GEMINI.md) / [`AGENTS.md`](AGENTS.md) | `.agents/skills/karpathy-guidelines/` | [ANTIGRAVITY.md](ANTIGRAVITY.md) |
| **OpenAI Codex** | [`AGENTS.md`](AGENTS.md) | `.agents/skills/karpathy-guidelines/` | [CODEX.md](CODEX.md) |
| **Claude Code** | [`CLAUDE.md`](CLAUDE.md) | 插件 / 市场 | [Claude Code 安装说明](#1-claude-code) |
| **Cursor** | [`.cursor/rules/`](.cursor/rules/karpathy-guidelines.mdc) | `~/.cursor/skills/` | [CURSOR.md](CURSOR.md) |

---

## 问题所在

来自 Andrej Karpathy 的推文：

> "模型会代你做错误假设，然后不假思索地执行。它们不管理自身的困惑，不寻求澄清，不呈现矛盾，不展示权衡，在应该提出异议时也不反驳。"

> "它们真的很喜欢把代码和 API 搞复杂，堆砌抽象概念，不清理死代码……明明 100 行能搞定的事情，非要实现成 1000 行的臃肿架构。"

> "它们有时仍会改动或删除自己理解不足的代码和注释，即使这些内容与任务本身无关。"

## 解决方案

四个原则，集中在统一规范中，直接解决这些问题：

| 原则 | 解决什么问题 |
|-----------|-----------|
| **1. 编码前思考** | 错误假设、隐藏困惑、缺少权衡 |
| **2. 简洁优先** | 过度复杂、臃肿抽象 |
| **3. 精准修改** | 无关编辑、触碰不应碰的代码 |
| **4. 目标驱动执行** | 通过测试优先、可验证的成功标准 |

---

## 四个原则详解

### 1. 编码前思考

**不要假设。不要隐藏困惑。呈现权衡。**

LLM 经常默默选择一种解释然后执行。这个原则强制明确推理：

- **明确说明假设** — 如果不确定，询问而不是猜测。
- **呈现多种解释** — 当存在歧义时，不要默默选择。
- **适时提出异议** — 如果存在更简单的方法，说出来。
- **困惑时停下来** — 指出不清楚的地方并要求澄清。

### 2. 简洁优先

**用最少的代码解决问题。不要过度推测。**

对抗过度工程的倾向：

- 不要添加要求之外的功能。
- 不要为一次性代码创建抽象。
- 不要添加未要求的"灵活性"或"可配置性"。
- 不要为不可能发生的场景做错误处理。
- 如果 200 行代码可以写成 50 行，重写它。

**检验标准：** 资深工程师会觉得这过于复杂吗？如果是，简化。

### 3. 精准修改

**只碰必须碰的。只清理自己造成的混乱。**

编辑现有代码时：

- 不要"改进"相邻的代码、注释或格式。
- 不要重构没坏的东西。
- 匹配现有风格，即使你更倾向于不同的写法。
- 如果注意到无关的死代码，提一下 —— 不要删除它。

当你的改动产生孤儿代码时：

- 删除因你的改动而变得无用的导入/变量/函数。
- 不要删除预先存在的死代码，除非被要求。

**检验标准：** 每一行修改都应该能直接追溯到用户的请求。

### 4. 目标驱动执行

**定义成功标准。循环验证直到达成。**

将指令式任务转化为可验证的目标：

| 不要这样做... | 转化为... |
|--------------|-----------------|
| "添加验证" | "为无效输入编写测试，然后让它们通过" |
| "修复 bug" | "编写重现 bug 的测试，然后让它通过" |
| "重构 X" | "确保重构前后测试都能通过" |

对于多步骤任务，说明一个简短的计划：

```
1. [步骤] → 验证: [检查]
2. [步骤] → 验证: [检查]
3. [步骤] → 验证: [检查]
```

强有力的成功标准让 LLM 能够独立循环执行。弱标准（"让它工作"）需要不断澄清。

---

## 安装与配置使用

### 1. Claude Code

**选项 A：Claude Code 插件（推荐）**
```bash
/plugin marketplace add longhakly/andrej-karpathy-skills
/plugin install andrej-karpathy-skills@karpathy-skills
```

**选项 B：CLAUDE.md（按项目）**
```bash
# 新项目：
curl -o CLAUDE.md https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/CLAUDE.md

# 已有项目（追加）：
echo "" >> CLAUDE.md && curl https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/CLAUDE.md >> CLAUDE.md
```

---

### 2. OpenAI Codex

OpenAI Codex 会自动读取工作区根目录下的 `AGENTS.md`。

**按项目指令：**
```bash
curl -o AGENTS.md https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/AGENTS.md
```

**按项目技能（按需激活）：**
```bash
mkdir -p .agents/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o .agents/skills/karpathy-guidelines/SKILL.md
```

**全局配置（所有 Codex 会话）：**
```bash
mkdir -p ~/.codex && curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/AGENTS.md >> ~/.codex/instructions.md
```
更多详情请参阅 **[CODEX.md](CODEX.md)**。

---

### 3. Google Antigravity

Antigravity 会自动发现工作区根目录下的 `GEMINI.md`、`AGENTS.md` 以及 `.agents/skills/` 中的技能。

**选项 A：项目规则（始终生效）**
```bash
curl -o GEMINI.md https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/GEMINI.md
```

**选项 B：项目技能（渐进式按需激活）**
```bash
mkdir -p .agents/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o .agents/skills/karpathy-guidelines/SKILL.md
```

**选项 C：全局技能（当前机器的所有 Antigravity 项目）**
```bash
mkdir -p ~/.gemini/config/skills/karpathy-guidelines
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.agents/skills/karpathy-guidelines/SKILL.md \
  -o ~/.gemini/config/skills/karpathy-guidelines/SKILL.md
```
更多详情请参阅 **[ANTIGRAVITY.md](ANTIGRAVITY.md)**。

---

### 4. Cursor

本仓库包含一个已提交的 Cursor 项目规则 ([`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc))。

在其他 Cursor 项目中使用：
```bash
mkdir -p .cursor/rules
curl -sL https://raw.githubusercontent.com/longhakly/andrej-karpathy-skills/main/.cursor/rules/karpathy-guidelines.mdc \
  -o .cursor/rules/karpathy-guidelines.mdc
```
详情请参见 **[CURSOR.md](CURSOR.md)**。

---

## 核心洞察

来自 Andrej Karpathy：

> "LLM 非常擅长循环执行直到达成特定目标……不要告诉它该做什么，给它成功标准，然后看着它完成。"

"目标驱动执行"原则正是捕捉了这一点：将指令式指令转化为带有验证循环的声明式目标。

## 如何判断它在起作用

如果你看到以下情况，说明这些指南正在发挥作用：

- **diff 中不必要的改动更少** —— 只有请求的改动出现
- **因过度复杂而导致的重写更少** —— 代码第一次就写得简洁
- **澄清问题在实现之前提出** —— 而不是在犯错之后
- **干净、精简的 PR** —— 没有顺带的重构或"改进"

## 定制

这些指南设计用于与项目特定指令合并。将它们添加到现有的 `AGENTS.md`、`GEMINI.md` 或 `CLAUDE.md` 中。

## 示例

参阅 **[EXAMPLES.md](EXAMPLES.md)** 查看涵盖每个原则的实际代码示例。

## 许可

MIT
