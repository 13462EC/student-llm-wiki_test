<div align="center">

# 📚 Student LLM Wiki

**基于 Karpathy LLM Wiki 模式的学生知识库插件**
**A student-focused knowledge base plugin powered by Karpathy's LLM Wiki pattern**

[English](#english) · [中文](#中文)

</div>

---

## English

### What is this?

A Claude Code / Cowork **plugin** that turns an Obsidian vault into a self-maintaining course knowledge base. You drop course slides into `raw/`; the AI compiles them into an interlinked wiki in `wiki/`. You never write wiki pages yourself — you curate sources and ask questions. Based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

### Student-specific features

- **Confidence decay** — Concepts auto-downgrade after 30 days without review. Weak areas surface on the dashboard.
- **Feynman review mode** — AI quizzes you with "why" and "what if" questions to test real understanding.
- **Exam prep generation** — Auto-generates practice questions from your weakest concepts.
- **Cross-course connections** — Actively links concepts across different courses.
- **Contradiction tracking** — Conflicting descriptions get flagged with `[!contradiction]` callouts.
- **Bilingual** — All pages in Chinese-English.
- **Domain rules** — Security: attack-defense pairs. ML: bias-variance. NLP: pretrain/finetune distinction.

### Plugin architecture

This is a real Claude Code plugin with modular skills — not a monolithic prompt. Each skill loads only when relevant, which keeps token usage low.

```
student-llm-wiki/
├── .claude-plugin/
│   ├── plugin.json          ← Plugin manifest
│   └── marketplace.json     ← Distribution config
├── skills/                  ← Modular skills (load on demand)
│   ├── wiki-core/           ←   Architecture + token rules (loads first)
│   ├── wiki-ingest/         ←   INGEST: digest a source
│   ├── wiki-lint/           ←   LINT: health check + confidence decay
│   ├── wiki-review/         ←   REVIEW: Feynman quiz
│   └── exam-prep/           ←   EXAM-PREP: generate questions
├── commands/                ← Slash commands
│   ├── ingest.md            ←   /ingest
│   ├── lint.md              ←   /lint
│   ├── review.md            ←   /review
│   └── exam-prep.md         ←   /exam-prep
├── raw/                     ← Layer 1: your course slides (read-only)
│   ├── COMP4337/  COMP6713/  COMP9417/  INFS5730/  misc/
│   └── .manifest.json       ←   hash dedup tracker
├── wiki/                    ← Layer 2: AI-maintained pages
│   ├── hot.md               ←   context cache (read first)
│   ├── index.md  overview.md  log.md
│   ├── courses/  concepts/  sources/  exam-prep/
├── SCHEMA.md                ← Human-readable design reference
└── Home.md                  ← Obsidian dashboard
```

### Why split into skills?

A single SCHEMA file was loaded fully into context every operation (~3000 tokens). Splitting into skills means `/ingest` loads only ingest rules, `/lint` loads only lint rules. Combined with the `hot.md` cache (≤500 words read at session start) and manifest dedup (skip unchanged files), this cuts token usage dramatically.

### Token efficiency

- **Hot cache** (`wiki/hot.md`) — AI reads ≤500 words at start instead of full rules.
- **Manifest dedup** (`raw/.manifest.json`) — files hashed before ingest; same hash = skip.
- **Token budget rules** (in `wiki-core`) — max 3–5 pages read per ingest, surgical edits, batch meta updates.

### Quick start

#### Option A: Install as a plugin (Claude Code / Cowork)

```bash
# Add this repo as a marketplace
claude plugin marketplace add YOUR_USERNAME/student-llm-wiki
# Install the plugin
claude plugin install student-llm-wiki@student-llm-wiki
```

Then open your vault folder, drop slides into `raw/`, and use the slash commands.

#### Option B: Use directly without installing

1. Download/clone this repo. Open the folder as an Obsidian vault. Install the [Dataview](https://github.com/blacksmithgu/obsidian-dataview) plugin.
2. Open the folder in Claude Code, or point a Cowork project at it.
3. The AI reads `skills/wiki-core/SKILL.md` automatically when you start working.
4. Drop course PDFs into `raw/{COURSE_CODE}/` and run a command.

### Commands

| Command | What it does |
|---------|-------------|
| `/ingest raw/COMP6713/L3.pdf` | Digest a source. Dedup + concept pages + cross-course links. |
| `/lint` | Health check: orphans, broken links, contradictions, confidence decay. |
| `/review COMP9417` | Feynman quiz mode. Adjusts confidence, generates practice questions. |
| `/exam-prep COMP4337` | Generate practice questions from weak concepts. |

You can also just say "ingest this file" or "quiz me on COMP9417" in natural language — the skills trigger automatically.

### Adding your own courses

1. Create a folder under `raw/` (e.g. `raw/MATH1131/`).
2. Create `wiki/courses/MATH1131-overview.md`.
3. If the course has special needs, add a domain rule in `skills/wiki-core/SKILL.md`.

### Credits

- [Andrej Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — the original pattern
- [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) — hot cache, manifest dedup, skill decomposition
- [Obsidian](https://obsidian.md) — the knowledge platform

### License

MIT

---

## 中文

### 这是什么？

一个 Claude Code / Cowork **插件**，把 Obsidian 知识库变成一个自维护的课程知识系统。你把课件丢进 `raw/`，AI 自动编译成互相链接的 wiki 到 `wiki/`。你不需要写任何 wiki 页面——只负责选材料和提问。基于 [Karpathy 的 LLM Wiki 模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)。

### 学生专属功能

- **Confidence 衰减** — 概念超过30天未复习自动降级，薄弱环节自动浮现
- **费曼复习模式** — AI 用"为什么""如果...会怎样"提问，测试真正理解
- **自动出题** — 根据最薄弱的概念生成练习题
- **跨课程连接** — 主动链接不同课程的概念
- **矛盾标注** — 冲突的描述用 `[!contradiction]` 标记
- **中英双语** — 所有页面双语
- **域规则** — 安全:攻防配对 | ML:bias-variance | NLP:预训练/微调区分

### 插件架构

这是一个真正的 Claude Code 插件，采用模块化 skill——不是单体提示词。每个 skill 只在相关时加载，大幅降低 token 消耗。

```
student-llm-wiki/
├── .claude-plugin/
│   ├── plugin.json          ← 插件清单
│   └── marketplace.json     ← 分发配置
├── skills/                  ← 模块化技能（按需加载）
│   ├── wiki-core/           ←   架构 + token规则（最先加载）
│   ├── wiki-ingest/         ←   消化课件
│   ├── wiki-lint/           ←   健康检查 + confidence衰减
│   ├── wiki-review/         ←   费曼复习
│   └── exam-prep/           ←   出题
├── commands/                ← 斜杠命令
│   ├── ingest.md  lint.md  review.md  exam-prep.md
├── raw/                     ← 第一层：课件（只读）
│   ├── COMP4337/  COMP6713/  COMP9417/  INFS5730/  misc/
│   └── .manifest.json       ←   hash去重追踪
├── wiki/                    ← 第二层：AI维护的页面
│   ├── hot.md               ←   上下文缓存（首读）
│   ├── index.md  overview.md  log.md
│   ├── courses/  concepts/  sources/  exam-prep/
├── SCHEMA.md                ← 人类可读的设计参考
└── Home.md                  ← Obsidian 仪表盘
```

### 为什么拆成 skill？

单体 SCHEMA 每次操作都被完整读入上下文（约3000 tokens）。拆成 skill 后，`/ingest` 只加载 ingest 规则，`/lint` 只加载 lint 规则。配合 `hot.md` 缓存（每次开始只读≤500词）和 manifest 去重（跳过未变文件），token 消耗大幅下降。

### Token 节省说明

- **Hot cache**（`wiki/hot.md`）：每次只读≤500词缓存，不读完整规则
- **Manifest 去重**（`raw/.manifest.json`）：文件 hash 校验，相同则跳过
- **Token 预算规则**（在 `wiki-core`）：每次最多读3-5页，局部编辑，批量更新meta

### 快速开始

#### 方式一：作为插件安装（Claude Code / Cowork）

```bash
# 把本仓库添加为 marketplace
claude plugin marketplace add 你的用户名/student-llm-wiki
# 安装插件
claude plugin install student-llm-wiki@student-llm-wiki
```

然后打开你的知识库文件夹，把课件放进 `raw/`，使用斜杠命令。

#### 方式二：不安装直接用

1. 下载/克隆本仓库，用 Obsidian 打开作为知识库，安装 [Dataview](https://github.com/blacksmithgu/obsidian-dataview) 插件
2. 用 Claude Code 打开该文件夹，或让 Cowork 项目指向它
3. 开始工作时 AI 会自动读取 `skills/wiki-core/SKILL.md`
4. 把课件 PDF 放进 `raw/{课程代码}/`，运行命令

### 命令

| 命令 | 作用 |
|------|------|
| `/ingest raw/COMP6713/L3.pdf` | 消化课件，去重+概念页+跨课程链接 |
| `/lint` | 健康检查：孤立页、断链、矛盾、confidence衰减 |
| `/review COMP9417` | 费曼复习，调整confidence，生成练习题 |
| `/exam-prep COMP4337` | 基于弱项生成练习题 |

也可以直接用自然语言说"消化这个文件"或"考考我 COMP9417"——skill 会自动触发。

### 添加自己的课程

1. 在 `raw/` 下创建文件夹（如 `raw/MATH1131/`）
2. 创建 `wiki/courses/MATH1131-overview.md`
3. 如有特殊需求，在 `skills/wiki-core/SKILL.md` 加一条域规则

### 致谢

- [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — 原始模式
- [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) — hot cache、manifest去重、skill拆分
- [Obsidian](https://obsidian.md) — 知识平台

### 许可证

MIT
