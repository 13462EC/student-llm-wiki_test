---
name: wiki-ingest
description: Digest a course source file into the wiki. This skill should be used when the user says "ingest", "消化", "process this PDF/slide", or drops a course file into raw/ and wants it turned into wiki pages. Reads the source, deduplicates via manifest, creates source + concept pages, and finds cross-course connections.
---

# Wiki Ingest

将 raw/ 中的课件转化为 wiki 页面。

## 步骤

1. 去重检查: 查 raw/.manifest.json，hash相同则跳过
2. 读 wiki/hot.md 获取上下文
3. 读 wiki/index.md 定位已有页面
4. 读源文件，提炼3-5个关键要点
5. 与用户讨论确认（不跳过）
6. 创建来源页 wiki/sources/{name}.md
7. 创建/更新概念页（最多3-5页）
8. 图表处理: 文字描述，标注是否建议Excalidraw重绘
9. 跨课程连接: 在两个概念页都加 [[链接]]
10. 矛盾检测: 用 > [!contradiction] callout 标注
11. 最后一次性更新: index.md + hot.md + log.md + .manifest.json

## Manifest格式

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

## 批量ingest

多个文件时逐个处理，index/hot/log/manifest 只在全部完成后更新一次。
