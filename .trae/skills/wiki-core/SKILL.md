---
name: wiki-core
description: Core operating rules for the student knowledge wiki. This skill should be used whenever working inside an Obsidian vault that contains a wiki/ folder with hot.md, or when the user mentions ingest, lint, review, exam-prep, or asks to build/maintain their course knowledge base. Loads the three-layer architecture, token budget rules, and bilingual page formats. Always loads first.
---

# Student LLM Wiki — Core

你是学生知识库维护者。读 `raw/`，写 `wiki/`。永不改 `raw/`。
You maintain a student knowledge wiki. Read `raw/`, write `wiki/`. Never modify `raw/`.

## 三层架构 Architecture

```
raw/{course}/    ← Layer 1: 只读课件 Read-only slides
wiki/            ← Layer 2: 你维护的知识 Knowledge you maintain
  hot.md         ←   上下文缓存(≤500词) Context cache, READ FIRST
  index.md       ←   总目录 Master catalog
  overview.md    ←   跨课程综述 Cross-course synthesis
  log.md         ←   操作日志(追加) Append-only log
  courses/       ←   课程MOC
  concepts/      ←   概念页(核心) Concept pages
  sources/       ←   来源页
  exam-prep/     ←   练习题
raw/.manifest.json  ← 去重追踪 Dedup tracker
```

## Token预算规则（最高优先级）

1. 每次session首先只读 `wiki/hot.md`（≤500词）
2. 需要更多上下文才读 `wiki/index.md`
3. 每次ingest最多读3-5个已有页面
4. 局部编辑，不全文重写
5. wiki页面100-300行，超300行拆分
6. 批量操作时 index/hot/log 最后更新一次

## 语言 Language

中英双语。标题"中文 English"。[[链接]] 用英文+连字符。

## 概念页格式

文件: wiki/concepts/{English-Name}.md
frontmatter: tags, courses, confidence(low|medium|high), last_reviewed
章节: 直觉(费曼)→详解→为什么重要→连接(含跨课程)→来源→待解决

## 域规则

- 数学: LaTeX + 直觉解释
- 安全 COMP4337: 攻防配对，lab验证标 ✅
- ML COMP9417: 适用场景 + 对比 + bias-variance
- NLP COMP6713: 架构描述 + 预训练/微调区分
- 分析 INFS5730: SAS VTA步骤

## 绝对规则

1. 永不改 raw/
2. 一概念一页，费曼风格
3. 跨课程连接是最大价值
4. 不确定标 confidence:low
5. Token预算规则高于一切
