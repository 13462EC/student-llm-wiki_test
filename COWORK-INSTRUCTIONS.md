# 学生知识库维护者 Student Wiki Maintainer

读 raw/ 课件，编译知识到 wiki/。永不改 raw/。首次运行读 SCHEMA.md。
Read raw/ slides, compile into wiki/. Never modify raw/. Read SCHEMA.md on first run.

## Token规则(最高优先级)
1. 每次先只读 wiki/hot.md (≤500词)
2. 需要更多上下文才读 wiki/index.md
3. 每次ingest最多读3-5个已有页面
4. 局部编辑，不要全文重写
5. 批量操作时 index/hot/log 最后更新一次
6. ingest前查 raw/.manifest.json，hash相同则跳过

## 架构
raw/{course}/ ← 只读课件
wiki/hot.md ← 上下文缓存，每次首读
wiki/index.md ← 总目录
wiki/concepts/ ← 概念页(费曼风格，中英双语，100-300行)
wiki/sources/ ← 来源摘要
wiki/courses/ ← 课程MOC
wiki/exam-prep/ ← 练习题

## 工作流
INGEST: 查manifest去重→读hot→读index→读源文件→与用户讨论→创建来源页+概念页(3-5页)→最后一次性更新index/hot/log/manifest→检查跨课程连接和矛盾
QUERY: 读hot→读index→读相关页(≤5)→综合回答→问是否保存
LINT: 孤立页+断链+矛盾+陈旧+confidence衰减(>30天未review降一级)+跨课程缺失
REVIEW: 费曼提问→调confidence+last_reviewed→为low概念出题→wiki/exam-prep/
EXAM-PREP: 扫概念→按confidence排序→为low/medium出题

## 概念页frontmatter
tags:[concept,{domain}] courses:[{codes}] confidence:{low|medium|high} last_reviewed:{date}

## 域规则
数学:LaTeX+直觉 | 安全:攻防配对+lab验证✅ | ML:场景+对比 | NLP:架构+预训练/微调区分

## 语言: 中英双语，[[链接]]英文+连字符
