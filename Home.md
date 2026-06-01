---
tags: [home]
---
# 🏠 知识库 v3 — Student LLM Wiki

> Obsidian浏览，Cowork编译，课件是燃料，wiki是产出。

## ⚡ 指令 Commands

| 说 | 做 |
|---|---|
| `ingest raw/COMP6713/L3.pdf` | 消化课件(自动去重) |
| `lint` | 健康检查+confidence衰减 |
| `review COMP9417` | 费曼复习 |
| `exam-prep COMP4337` | 弱项出题 |

## 📚 课程

- [[COMP4337-overview]]  [[COMP6713-overview]]
- [[COMP9417-overview]]  [[INFS5730-overview]]

## 🔴 薄弱概念

```dataview
TABLE courses, confidence, last_reviewed
FROM "wiki/concepts" WHERE confidence = "low"
SORT last_reviewed ASC
```

## 🟡 即将衰减 (>20天未复习)

```dataview
TABLE courses, last_reviewed
FROM "wiki/concepts"
WHERE confidence = "medium" AND (date(today) - date(last_reviewed)).days > 20
SORT last_reviewed ASC
```

## 🏝️ 孤岛

```dataview
LIST FROM "wiki/concepts" WHERE length(file.inlinks) = 0
```

## 📈 最近更新

```dataview
TABLE updated FROM "wiki" WHERE updated SORT updated DESC LIMIT 8
```
