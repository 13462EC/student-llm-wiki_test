---
name: wiki-lint
description: Health-check the wiki and apply confidence decay. This skill should be used when the user says "lint", "检查wiki", "check the wiki", or wants to find orphan pages, broken links, contradictions, or stale concepts. Runs the 30-day confidence decay rule and reports issues for the user to fix.
---

# Wiki Lint

健康检查知识库。

## 检查项

1. 孤立页: 没有任何 [[链接]] 指向的概念页
2. 断链: [[链接]] 指向不存在的页面
3. 矛盾: 不同页面对同一概念的冲突描述
4. 陈旧内容: confidence:low 且长期未更新
5. Confidence 衰减: last_reviewed 距今 >30天 → 降一级（high→medium→low）
6. 跨课程缺失: 同一概念出现在多门课但没有互链

## 流程

1. 读 wiki/index.md 获取所有页面
2. 按需读取页面检查（遵守token预算，分批）
3. 报告发现，询问用户要修复哪些（不要自动全改）
4. 修复后，批量更新 glossary/overview

## 报告格式

🏝️ 孤立页 (N): ...
🔗 断链 (N): ...
⚡ 矛盾 (N): ...
📉 衰减降级 (N): [概念] high→medium (45天未复习)
🌉 缺失跨课连接 (N): ...
