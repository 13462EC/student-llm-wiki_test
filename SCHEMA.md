# SCHEMA — 设计参考 Design Reference

> ⚠️ 本文件现在只是**人类可读的设计参考**。
> 真正的操作规则已拆分到 `skills/` 目录下的各个 SKILL.md。
> This file is now a human-readable reference only.
> The actual operating rules live in the `skills/` directory.

## 系统如何工作 How It Works

这是一个 Claude Code / Cowork 插件，实现 Karpathy LLM Wiki 模式的学生版。

| 层 Layer | 内容 | 位置 |
|---|---|---|
| Layer 1 数据源 | 只读课件 | `raw/{course}/` |
| Layer 2 知识 | AI维护的wiki | `wiki/` |
| Layer 3 规则 | 模块化skill | `skills/*/SKILL.md` |

## Skills（按需加载，省token）

| Skill | 触发 | 作用 |
|---|---|---|
| `wiki-core` | 总是首先加载 | 架构 + token预算规则 + 页面格式 |
| `wiki-ingest` | "ingest" / 拖入课件 | 消化课件，去重，建概念页 |
| `wiki-lint` | "lint" / "检查" | 健康检查 + confidence衰减 |
| `wiki-review` | "review" / "复习" | 费曼提问 + 更新confidence |
| `exam-prep` | "exam-prep" / "备考" | 弱项出题 |

## Commands（斜杠命令）

`/ingest [文件]` · `/lint` · `/review [课程]` · `/exam-prep [课程]`

## 为什么拆成skill？ Why split into skills?

单体 SCHEMA 每次都被完整读入上下文（3000+ tokens）。拆成skill后，做ingest就只加载ingest的规则，做lint就只加载lint的规则。配合 `wiki/hot.md` 缓存，大幅降低token消耗。

A monolithic SCHEMA was loaded fully into context every time (~3000 tokens). Split into skills, only the relevant skill loads per operation. Combined with the `wiki/hot.md` cache, this cuts token usage significantly.
