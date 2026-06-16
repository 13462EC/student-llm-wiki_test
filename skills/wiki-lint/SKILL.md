---
name: wiki-lint
description: Health-check the wiki and apply confidence decay. This skill should be used when the user says "lint", "检查wiki", "check the wiki", or wants to find orphan pages, broken links, contradictions, or stale concepts. Runs the 30-day confidence decay rule and reports issues for the user to fix.
---

# Wiki Lint

健康检查知识库。
Health-check the knowledge base.

## 检查项 Checks

1. **孤立页 Orphans**: 没有任何 `[[链接]]` 指向的概念页
2. **断链 Broken links**: `[[链接]]` 指向不存在的页面
3. **矛盾 Contradictions**: 不同页面对同一概念的冲突描述。同时写入 `wiki/contradictions.md`（用 wiki-ingest 中的矛盾记录格式）并更新 `overview.md` 的矛盾摘要
4. **陈旧内容 Stale**: confidence:low 且长期未更新的页面
5. **Confidence 衰减 Decay**:
   - 若概念页 `last_reviewed` 距今 >30天，且期间未被新页面引用 → confidence 降一级（high→medium→low）
   - 列出所有降级的概念
6. **跨课程缺失 Missing links**: 同一概念出现在多门课但没有互链
7. **课程总览缺失 Missing overviews**: `index.md` 中每门课是否都有对应的 `wiki/courses/{course}-overview.md`；缺少则自动创建（用 wiki-ingest 中的课程总览页格式）
8. **术语表缺失 Glossary gaps**: 检查 `wiki/glossary.md` 中缺少哪些已有概念页的术语条目，补充完整

## 流程 Process

1. 读 `wiki/index.md` 获取所有页面列表
2. 按需读取页面检查（遵守token预算，分批检查）
3. 报告发现，**询问用户要修复哪些**（不要自动全改）
4. 修复后，批量维护（无需用户确认，直接更新）：
   - `wiki/courses/{course}-overview.md`（缺少的课程总览页直接新建）
   - `wiki/glossary.md`（补充缺失的术语条目）
   - `wiki/contradictions.md`（追加新发现的矛盾）
   - `wiki/connections-log.md`（补录缺失的跨课连接）
   - `wiki/overview.md`（同步更新矛盾摘要、连接摘要、课程列表）
   - `wiki/log.md`（追加格式: `## {YYYY-MM-DD} — lint: 孤立N 断链N 衰减N 矛盾N 补连N 补总览N 补术语N`）

## 报告格式 Report

```
🏝️ 孤立页 (N): ...
🔗 断链 (N): ...
⚡ 矛盾 (N): ...
📉 衰减降级 (N): [概念] high→medium (45天未复习)
🌉 缺失跨课连接 (N): [概念A] 和 [概念B] 应互链
📂 缺失课程总览 (N): wiki/courses/{course}-overview.md
📖 术语表缺失条目 (N): [概念名]
```
