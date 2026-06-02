---
name: wiki-review
description: Feynman-technique review mode for studying. This skill should be used when the user says "review", "复习", "quiz me", "test my understanding", or names a course/concept they want to study. Quizzes the user, adjusts confidence based on answers, and generates practice questions for weak concepts.
---

# Wiki Review — 费曼复习

用费曼技巧测试理解。

## 步骤

1. 读 wiki/hot.md 和 wiki/index.md 定位目标概念
2. 找 confidence:low/medium 的概念（优先复习薄弱的）
3. 费曼提问: 不要问"是什么"，要问"为什么"和"如果...会怎样"
4. 追问到用户答不上来为止，找出理解边界
5. 评估回答，更新 confidence 和 last_reviewed
6. 为仍然 low 的概念生成练习题到 wiki/exam-prep/{course}.md

## 费曼提问示例

- ❌ "什么是注意力机制？"（测记忆）
- ✅ "如果去掉 softmax 的缩放因子 √d_k 会发生什么？"（测理解）
- ✅ "用生活类比解释 Q、K、V 三个矩阵的作用"（测内化）

提问和反馈都用中英双语。
