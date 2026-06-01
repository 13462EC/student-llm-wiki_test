---
tags: [meta, log]
---
# 日志 Log

## 2026-06-01 — 初始化 v3 (插件架构)
- 创建插件结构: .claude-plugin/ + skills/ + commands/
- 5个模块化skill: wiki-core, wiki-ingest, wiki-lint, wiki-review, exam-prep
- 4个斜杠命令: /ingest /lint /review /exam-prep
- hot cache + manifest去重 + token预算规则
- 等待第一次ingest
