---
name: exam-prep
description: Generate practice exam questions from weak concepts. This skill should be used when the user says "exam-prep", "备考", "make practice questions", "generate a quiz", or is preparing for an exam in a specific course. Scans concept pages, prioritizes low-confidence ones, and writes a practice question set.
---

# Exam Prep — 出题

基于薄弱概念生成练习题。

## 步骤

1. 读 wiki/index.md 找到目标课程所有概念页
2. 读 frontmatter，按 confidence 排序（low 优先）
3. 为每个 low/medium 概念生成 1-2 道题
4. 保存到 wiki/exam-prep/{course}.md

## 题目类型（混合，不要全是记忆题）

- 概念应用题: 给场景，问该用哪个方法
- 对比题: 两个相关概念的区别和适用场景
- 推导题（ML课）: 要求推导或解释公式
- 攻防题（安全课）: 给攻击，问防御，反之亦然
- 陷阱题: 针对常见误解设计

## 原则

- 优先 confidence:low
- 每题标注来自哪个概念页
- 参考答案完整，鼓励用户先自己答
