---
tags: [home]
---
# 🏠 知识库 — Student LLM Wiki

> Obsidian浏览，Claude编译，课件是燃料，wiki是产出。

## ⚡ 命令 Commands

| 命令 | 作用 |
|---|---|
| `/ingest raw/COMPXXXX/L1.pdf` | 消化课件(自动去重) |
| `/lint` | 健康检查+confidence衰减 |
| `/review COMPXXXX` | 费曼复习 |
| `/exam-prep COMPXXXX` | 弱项出题 |

> 将你的课件 PDF 放入 `raw/{课程代码}/`，然后运行 `/ingest`，课程总览页会自动生成。

## 📚 课程 Courses

```dataview
TABLE file.mtime AS 最近更新
FROM "wiki/courses"
SORT file.mtime DESC
```

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
