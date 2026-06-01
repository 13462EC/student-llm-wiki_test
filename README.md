<div align="center">

# 📚 Student LLM Wiki

**基于 Karpathy LLM Wiki 模式的学生知识库系统**
**A student-focused knowledge base powered by Karpathy's LLM Wiki pattern**

[English](#english) · [中文](#中文)

</div>

---

## English

### What is this?

A ready-to-use Obsidian vault designed for university students who want AI to **compile their course materials into a persistent, interlinked knowledge wiki**. Based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), enhanced with student-specific features and token-efficiency optimizations borrowed from [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian).

**The idea**: You drop course slides into `raw/`. AI reads them and compiles knowledge into `wiki/`. You never write wiki pages yourself — you curate sources and ask questions. Knowledge compounds over time.

### Why not just use claude-obsidian directly?

claude-obsidian is excellent for general knowledge work. This project adds what students specifically need:

| Feature | claude-obsidian | Student LLM Wiki |
|---------|----------------|-------------------|
| Confidence decay | ❌ | ✅ Concepts auto-degrade after 30 days without review |
| Feynman review mode | ❌ | ✅ AI quizzes you using the Feynman technique |
| Exam prep generation | ❌ | ✅ Auto-generates practice questions from weak concepts |
| Cross-course connections | Generic | ✅ Actively seeks links across multiple courses |
| Contradiction tracking | `[!contradiction]` callout | ✅ Same, plus dedicated lint detection |
| Course-organized sources | Generic `raw/` | ✅ `raw/{COURSE_CODE}/` structure |
| Bilingual support | English | ✅ Chinese-English bilingual throughout |
| Domain-specific rules | Generic | ✅ Per-course rules (math notation, attack-defense pairs, etc.) |

### Architecture

```
vault/
├── raw/                    ← Layer 1: Immutable sources (read-only)
│   ├── COMP4337/           ←   Course slides, papers, handouts
│   ├── COMP6713/
│   ├── COMP9417/
│   └── .manifest.json      ←   Hash-based dedup tracker
├── wiki/                   ← Layer 2: AI-maintained knowledge pages
│   ├── hot.md              ←   Context cache (≤500 words, read first)
│   ├── index.md            ←   Master catalog
│   ├── overview.md         ←   Cross-course synthesis
│   ├── log.md              ←   Append-only operation log
│   ├── courses/            ←   Course MOC pages
│   ├── concepts/           ←   Concept pages (the core)
│   ├── sources/            ←   Source summaries
│   └── exam-prep/          ←   Auto-generated practice questions
├── SCHEMA.md               ← Layer 3: Operating rules for AI
├── COWORK-INSTRUCTIONS.md  ← Compact version for Cowork Instructions field
└── Home.md                 ← Dashboard with Dataview queries
```

### Token efficiency

Previous versions burned through usage quotas fast. v3 borrows three mechanisms from claude-obsidian:

- **Hot cache** (`wiki/hot.md`): AI reads ≤500 words at session start instead of the full SCHEMA + index + overview. ~80% reduction in startup overhead.
- **Manifest dedup** (`raw/.manifest.json`): Files are hashed before ingest. Same hash = skip. No wasted tokens on re-processing.
- **Token budget rules** (top priority in SCHEMA): Max 3-5 existing pages read per ingest. Surgical edits instead of full rewrites. Batch meta-file updates (index/hot/log updated once at the end, not per-source).

### Quick start

#### Option A: Claude Desktop (Cowork)

1. Download and unzip this repo
2. Open the `vault/` folder as an Obsidian vault. Install the [Dataview](https://github.com/blacksmithgu/obsidian-dataview) plugin.
3. In Claude Desktop → Cowork → Create project → point to the same `vault/` folder
4. Paste the contents of `COWORK-INSTRUCTIONS.md` into the Instructions field
5. Drop your course PDFs into `raw/{COURSE_CODE}/`
6. Tell Cowork:

```
Read SCHEMA.md, then ingest raw/COMP6713/Lecture3-Attention.pdf
```

#### Option B: Claude Code (terminal)

1. Clone this repo and `cd vault/`
2. Open it as an Obsidian vault
3. Start Claude Code in the same directory
4. Claude will read `SCHEMA.md` automatically

#### Option C: Claudian (Obsidian plugin)

1. Open the vault in Obsidian with [Claudian](https://github.com/Enigmora/claudian) installed
2. Tell Claudian to read `SCHEMA.md` first
3. Use the same commands

### Commands

| Command | What it does |
|---------|-------------|
| `ingest raw/COMP6713/L3.pdf` | Digest a source file. Creates source page + concept pages. Auto-deduplicates. |
| `lint` | Health check: orphan pages, broken links, contradictions, confidence decay (>30 days → auto-downgrade) |
| `review COMP9417` | Feynman review mode: AI quizzes you, adjusts confidence, generates practice questions for weak concepts |
| `exam-prep COMP4337` | Generate practice questions from all low/medium confidence concepts in a course |

### Concept page format

Every concept page follows this structure:

```markdown
---
tags: [concept, nlp]
courses: [COMP6713]
confidence: medium
last_reviewed: 2026-06-01
---
# 注意力机制 Attention Mechanism

## 直觉 Intuition
(Feynman-style explanation for a smart outsider)

## 详细解释 Detailed Explanation
(Technical details, formulas, algorithms)

## 为什么重要 Why It Matters

## 连接 Connections
- [[Transformer-Architecture]] — Attention is the core component
- [[Word-Embeddings]] — Attention operates on vector representations

## 来源 Sources
- [[L3-Attention]]

## 待解决 Open Questions
- ?
```

### Customization

**Adding courses**: Create a new folder under `raw/` and a new overview page under `wiki/courses/`. Update `SCHEMA.md` domain rules if the course has special requirements.

**Changing language**: Modify the language rules section in `SCHEMA.md`. The system supports any bilingual combination.

**Adjusting confidence decay**: Change the 30-day threshold in the LINT workflow section of `SCHEMA.md`.

### Credits

- [Andrej Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — the original pattern
- [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) — hot cache, manifest dedup, and token budget concepts
- [Obsidian](https://obsidian.md) — the knowledge platform

### License

MIT

---

## 中文

### 这是什么？

一个面向大学生的即用型 Obsidian 知识库模板，让 AI 将你的**课件自动编译成持久化、互相链接的知识 wiki**。基于 [Karpathy 的 LLM Wiki 模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)，融合了 [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) 的 token 优化机制，并新增学生专属功能。

**核心理念**：你把课件 PDF 丢进 `raw/`，AI 读取并编译知识到 `wiki/`。你不需要自己写任何 wiki 页面——你只负责选材料和提问。知识会像复利一样随时间增长。

### 学生专属功能

- **Confidence 衰减**：概念超过30天未复习自动降级，薄弱环节自动浮现
- **费曼复习模式**：AI 用费曼技巧向你提问，测试你是否真正理解
- **自动出题**：基于 confidence:low 的概念生成练习题
- **跨课程连接**：主动寻找不同课程之间的概念关联（这是最有价值的）
- **矛盾标注**：不同资料对同一概念的不同描述会被标记
- **中英双语**：所有 wiki 页面双语输出
- **域特定规则**：安全课标注攻防配对+lab验证，ML课标注bias-variance，NLP课区分预训练/微调

### Token 节省机制

上一版处理三周PPT就用完了5小时额度。v3 通过三个机制解决：

- **Hot cache**（`wiki/hot.md`）：每次 session 只读≤500词缓存，不再读完整 SCHEMA
- **Manifest 去重**（`raw/.manifest.json`）：相同文件不重复处理
- **Token 预算规则**：每次最多读3-5个已有页面，局部编辑不全文重写，批量操作只在最后更新一次 meta 文件

### 快速开始

1. 下载解压，用 Obsidian 打开 `vault/` 文件夹，安装 Dataview 插件
2. 在 Claude Desktop 的 Cowork 中创建项目，指向同一文件夹
3. 把 `COWORK-INSTRUCTIONS.md` 的内容粘贴到 Instructions 框
4. 把课件放进 `raw/{课程代码}/`
5. 对 Cowork 说：`读 SCHEMA.md，然后 ingest raw/COMP6713/Lecture3.pdf`

### 命令

| 命令 | 作用 |
|------|------|
| `ingest raw/COMP6713/L3.pdf` | 消化课件（自动去重） |
| `lint` | 健康检查 + confidence 衰减检测 |
| `review COMP9417` | 费曼复习模式 |
| `exam-prep COMP4337` | 基于弱项自动出题 |

### 致谢

- [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — 原始模式
- [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) — hot cache、manifest 去重、token 预算概念
- [Obsidian](https://obsidian.md) — 知识平台

### 许可证

MIT
