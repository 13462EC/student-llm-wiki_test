# SCHEMA v3 — 学生知识库 Student Knowledge Wiki

你是知识库维护者。读 raw/，写 wiki/。永不改 raw/。
You maintain this wiki. Read raw/, write wiki/. Never modify raw/.

## 架构 Architecture

```
raw/{course}/    ← 只读课件 Read-only slides/papers
wiki/
  hot.md         ← 上下文缓存(≤500词) Context cache, READ FIRST
  index.md       ← 总目录 Master catalog
  overview.md    ← 跨课程综述 Cross-course synthesis
  log.md         ← 操作日志(追加) Append-only log
  courses/       ← 课程MOC Course overviews
  concepts/      ← 概念页(核心) Concept pages
  sources/       ← 来源页 Source summaries
  exam-prep/     ← 练习题 Practice questions
raw/.manifest.json  ← 去重追踪 Dedup tracker
```

## Token预算规则 Token Budget Rules

这些规则是最高优先级，违反它们会浪费用户的额度：
These rules are TOP PRIORITY — violating them wastes the user's quota:

1. **每次session首先只读 `wiki/hot.md`**（≤500词）。如果它包含所需上下文，不要读其他页面
2. **需要更多上下文时，读 `wiki/index.md`**，从中定位相关页面
3. **每次ingest最多读3-5个已有页面**。超过5个说明你读得太广了
4. **用局部编辑替代全文重写**。更新一个字段时不要重写整页
5. **wiki页面保持100-300行**。超过300行必须拆分
6. **批量ingest时，index/hot/log只在最后更新一次**，不是每个源文件更新一次
7. **ingest前检查 `.manifest.json`**，已处理过的文件跳过

## 去重机制 Dedup (.manifest.json)

```json
{
  "sources": {
    "raw/COMP6713/L3-Attention.pdf": {
      "hash": "md5-of-file",
      "ingested_at": "2026-06-01",
      "pages_created": ["wiki/sources/L3-Attention.md"],
      "pages_updated": ["wiki/concepts/Attention-Mechanism.md"]
    }
  }
}
```

Ingest前计算文件hash。hash相同则跳过，报告"已处理，用force重新消化"。

## Hot Cache (wiki/hot.md)

每次session结束或ingest完成后更新。内容≤500词，包含：
- 最近处理的3个来源
- 最近创建/更新的概念
- 当前薄弱概念(confidence:low)
- 待处理事项

## 语言 Language

中英双语。标题"中文 English"。[[链接]]用英文+连字符。

## 页面格式 Page Formats

### 概念页 wiki/concepts/{Name}.md
```yaml
---
tags: [concept, {domain}]
courses: [COMP6713]
confidence: low|medium|high
last_reviewed: 2026-06-01
---
```
节: 直觉Intuition → 详解Detail → 为什么重要Why → 连接Connections(含跨课程) → 来源Sources → 待解决Open
- 直觉部分用费曼风格，给聪明的外行人听
- 100-300行，超过拆分

### 来源页 wiki/sources/{name}.md
关键要点(3-5个) → 图表描述 → 新概念列表 → 更新了哪些页面

### 课程MOC wiki/courses/{CODE}-overview.md
课程综述 → 概念图谱(按主题分组) → 薄弱环节(Dataview) → 来源列表

## 工作流 Workflows

### INGEST "ingest {path}"
1. 检查 .manifest.json，hash相同则跳过
2. 读 hot.md 获取上下文
3. 读 index.md 定位已有页面
4. 读原始文件，提炼要点
5. **与用户讨论确认**（不跳过）
6. 创建来源页 + 创建/更新概念页(3-5页，不贪多)
7. 如果发现图表，文字描述并标注是否建议Excalidraw重绘
8. 最后一次性更新: index.md, hot.md, log.md, .manifest.json
9. 如发现跨课程连接，在两个概念页都加[[链接]]，记入log
10. 如发现矛盾，用 `> [!contradiction]` callout标注在概念页

### QUERY 用户提问时
1. 读 hot.md → 读 index.md → 读相关页(≤5个)
2. 综合回答，引用[[页面]]
3. 问是否保存到 wiki/analyses/ (仅高价值回答)

### LINT "lint"
1. 孤立页(无inlink) → 断链 → 矛盾 → 陈旧内容
2. **Confidence衰减**: last_reviewed距今>30天且未被新引用 → 降一级
3. 跨课程连接缺失检查
4. 报告后问用户修复哪些
5. 批量更新glossary/overview等低优先级页面(平时不更新这些)

### REVIEW "review {course|concept}"
1. 找confidence:low/medium的概念
2. 费曼提问测试理解
3. 更新confidence和last_reviewed
4. 为仍然low的概念生成练习题 → wiki/exam-prep/{course}.md

### EXAM-PREP "exam-prep {course}"
扫描概念页 → 按confidence排序 → 为low/medium出题 → 保存到 wiki/exam-prep/

## 域规则 Domain Rules (简)

- **数学**: LaTeX + 直觉解释
- **安全COMP4337**: 攻防配对，lab验证标 ✅
- **ML COMP9417**: 适用场景+对比+bias-variance
- **NLP COMP6713**: 架构描述+预训练/微调区分
- **分析INFS5730**: SAS VTA步骤+数据类型

## 绝对规则 Hard Rules

1. 永不改raw/
2. 每次记log（批量时最后记一次）
3. 费曼风格，一概念一页
4. 跨课程连接是最大价值
5. 不确定标 confidence:low
6. **token预算规则高于一切其他规则**
