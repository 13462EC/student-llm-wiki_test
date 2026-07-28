---
tags: [course-overview, riemann]
course: Riemann
updated: 2026-07-28
---
# Riemann — A Panoramic View of Riemannian Geometry (Berger)

## 综述 Summary
基于 Berger《A Panoramic View of Riemannian Geometry》一书的章节式精读; 已消化第 9 章: 黎曼流形上 Laplace 算子的谱与特征函数——把流形视为"量子力学世界"。

## 概念图谱 Concept Map
[[Spectrum-of-Laplacian]] · [[Heat-Kernel-Asymptotics]] · [[Wave-Equation-Length-Spectrum]] · [[First-Eigenvalue]] · [[Isospectral-Manifolds]]

> 本章方法论主线: **热方程(温和健忘)↔ 波方程(锋利昂贵)** 的张力。热核给出 Weyl 律与曲率不变量 $U_k$; 波方程经"Jacob's ladder"爬到单位切丛, 把谱与周期测地线(长度谱)相连。$\lambda_1$ 是曲率↔谱的最直接桥梁; 反谱问题("能听出形状吗?")是整章的试金石。

## 薄弱环节 Weak Areas
```dataview
TABLE confidence, last_reviewed FROM "wiki/concepts"
WHERE contains(courses, "Riemann") AND confidence = "low"
SORT last_reviewed ASC
```

## 来源 Sources
```dataview
LIST FROM "wiki/sources" WHERE contains(tags, "riemann") SORT file.ctime DESC
```

## 待消化章节 Pending Chapters
- `raw/Riemann/ch1.pdf` — 待 ingest
- `raw/Riemann/ch7.pdf` — 待 ingest
- `raw/Riemann/1.1-1.3.pdf` — 待 ingest

## 进度 Progress
- ✅ Ch.9 (Spectrum of the Laplacian) — 2026-07-28
