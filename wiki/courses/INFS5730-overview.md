---
tags: [course-overview, infs5730, analytics]
updated: 2026-06-01
---
# INFS5730 — 社交媒体分析 Social Media Analytics

## 综述 Summary
从社交媒体非结构化文本中提取商业洞察。SAS Visual Text Analytics：概念提取、主题建模、情感分析。

## 概念图谱 Concept Map
*等待 ingest*

## 薄弱环节 Weak Areas
```dataview
TABLE confidence, last_reviewed FROM "wiki/concepts"
WHERE contains(courses, "INFS5730") AND confidence = "low"
SORT last_reviewed ASC
```

## 来源 Sources
```dataview
LIST FROM "wiki/sources" WHERE contains(tags, "infs5730") SORT ingested DESC
```
