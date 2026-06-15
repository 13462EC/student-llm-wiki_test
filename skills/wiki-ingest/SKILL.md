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
7. **创建/更新概念页**（最多3-5页，不贪多）:
   - 新概念 → 新建 `wiki/concepts/{Name}.md`，confidence默认 low
   - 已有概念 → 局部编辑，补充内容，更新 `updated` 日期
8. **图表处理**: 若源含重要图表，文字描述其内容；若该概念涉及流程/架构/时序/分类等适合可视化的内容，按 wiki-diagram skill 判断标准用 Mermaid 配图
9. **跨课程连接**: 检查新概念是否在其他课程出现过。若是，在两个概念页都加 `[[链接]]`
10. **矛盾检测**: 若新内容与已有页面冲突，在概念页用 `> [!contradiction]` callout 标注
11. **最后一次性更新**: `index.md` + `hot.md` + `log.md` + `.manifest.json`（不要每步都更新）

## Manifest 格式

```json
{
  "sources": {
    "raw/COMP6713/L3.pdf": {
      "hash": "abc123",
      "ingested_at": "2026-06-01",
      "pages_created": ["wiki/sources/L3.md"],
      "pages_updated": ["wiki/concepts/Attention-Mechanism.md"]
    }
  }
}
```

## 批量 Batch ingest

多个文件时：逐个处理，但 index/hot/log/manifest **只在全部完成后更新一次**。每10个文件后向用户汇报一次进度。

## 完成报告 Report

"处理了 N 个来源。创建 X 页，更新 Y 页。发现的跨课程连接: ..."
