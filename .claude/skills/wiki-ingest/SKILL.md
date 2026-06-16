---
name: wiki-ingest
description: Digest a course source file into the wiki. This skill should be used when the user says "ingest", "消化", "process this PDF/slide", or drops a course file into raw/ and wants it turned into wiki pages. Reads the source, deduplicates via manifest, creates source + concept pages, and finds cross-course connections.
---

# Wiki Ingest

将 `raw/` 中的课件转化为 wiki 页面。
Turn a source file in `raw/` into wiki pages.

## 步骤 Steps

1. **去重检查**: 计算文件 `md5sum {file} | cut -d' ' -f1`。查 `raw/.manifest.json`，hash相同则跳过，报告"已处理，用force重新消化"
2. **读上下文**: 读 `wiki/hot.md`（不是完整SCHEMA）
3. **定位已有页**: 读 `wiki/index.md` 找相关概念页
4. **读源文件**，提炼3-5个关键要点
5. **与用户讨论确认**（不跳过这步）
6. **创建来源页** `wiki/sources/{name}.md`
6.5. **创建/更新课程总览页** `wiki/courses/{COURSE}-overview.md`:
   - 不存在则新建（用下方"课程总览页格式"）
   - 已有则在"概念图谱"章节追加本次新概念的 `[[链接]]`
7. **创建/更新概念页**（最多3-5页，不贪多）:
   - 新概念 → 新建 `wiki/concepts/{Name}.md`，confidence默认 low，**设置 `created_at` 为今天日期**
   - 已有概念 → 局部编辑，补充内容，更新 `last_reviewed` 日期（不改 `created_at`）
8. **图表处理**: 若源含重要图表，文字描述其内容；若该概念涉及流程/架构/时序/分类等适合可视化的内容，按 wiki-diagram skill 判断标准用 Mermaid 配图
9. **跨课程连接**: 检查新概念是否在其他课程出现过。若是：
   - 在两个概念页都加 `[[链接]]`
   - 在 `wiki/connections-log.md` 追加一条记录（用下方"连接日志格式"）
10. **矛盾检测**: 若新内容与已有页面冲突：
    - 在概念页用 `> [!contradiction]` callout 标注
    - 在 `wiki/contradictions.md` 追加一条记录（用下方"矛盾记录格式"）
11. **最后一次性更新**: `index.md` + `hot.md` + `log.md` + `overview.md` + `.manifest.json`（不要每步都更新）
    - `overview.md` 更新内容：有新课程则加入课程列表，有新连接则更新"跨课程连接"摘要，有新矛盾则更新"矛盾与张力"摘要
    - `wiki/glossary.md` 将本次新建概念追加到术语表（英文、中文、领域、`[[页面链接]]`）
    - `log.md` 追加一条结构化日志记录（用下方"日志格式"）

## Manifest 格式

```json
{
  "sources": {
    "raw/COMP6713/L3.pdf": {
      "hash": "abc123",
      "course": "COMP6713",
      "ingested_at": "2026-06-01",
      "pages_created": ["wiki/sources/L3.md"],
      "concepts_created": ["Attention-Mechanism"],
      "concepts_updated": ["Transformer"],
      "connections_found": 2,
      "contradictions_found": 0
    }
  }
}
```

## 批量 Batch ingest

多个文件时：逐个处理，但 index/hot/log/manifest **只在全部完成后更新一次**。每10个文件后向用户汇报一次进度。

## 完成报告 Report

"处理了 N 个来源。创建 X 页，更新 Y 页。发现的跨课程连接: ..."

## 课程总览页格式 Course Overview Format

文件: `wiki/courses/{COURSE}-overview.md`
```markdown
---
tags: [course-overview, {course-code}]
course: {COURSE-CODE}
updated: YYYY-MM-DD
---
# {COURSE-CODE} — {课程名 Course Name}

## 综述 Summary
{一句话描述课程主题}

## 概念图谱 Concept Map
[[Concept-A]] · [[Concept-B]] · ...

## 薄弱环节 Weak Areas
\`\`\`dataview
TABLE confidence, last_reviewed FROM "wiki/concepts"
WHERE contains(courses, "{COURSE-CODE}") AND confidence = "low"
SORT last_reviewed ASC
\`\`\`

## 来源 Sources
\`\`\`dataview
LIST FROM "wiki/sources" WHERE contains(tags, "{course-code}") SORT file.ctime DESC
\`\`\`
```

## 连接日志格式 Connection Log Format

追加到 `wiki/connections-log.md`：
```markdown
## {YYYY-MM-DD} — {概念名}
**课程A**: {COURSE-A} → [[Concept-Page-A]]
**课程B**: {COURSE-B} → [[Concept-Page-B]]
**连接本质**: 用一句话描述为什么这两个概念有关
**深度**: 表面相似 / 共享数学基础 / 同一思想的不同应用
```

## 日志格式 Log Entry Format

追加到 `wiki/log.md`：
```markdown
## {YYYY-MM-DD} — ingest: {source-file}
- 创建概念: [[Concept-A]], [[Concept-B]]
- 更新概念: [[Concept-C]]
- 跨课连接: N条（见 connections-log.md）
- 矛盾: N条（见 contradictions.md）
```

## 矛盾记录格式 Contradiction Format

追加到 `wiki/contradictions.md`：
```markdown
## {YYYY-MM-DD} — {矛盾标题}
**页面A**: [[page-a]] 说 "..."
**页面B**: [[page-b]] 说 "..."
**张力**: 描述矛盾的本质
**状态**: 未解决
**解决方案**: （待填）
```
