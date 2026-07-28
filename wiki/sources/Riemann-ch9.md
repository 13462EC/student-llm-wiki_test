---
tags: [source, riemann]
course: Riemann
ingested: 2026-07-28
source_file: raw/Riemann/ch9.pdf
---
# 来源: Berger《A Panoramic View of Riemannian Geometry》第9章
# Source: Berger, Ch.9 — Spectrum & Eigenfunctions of the Laplacian

> Berger, M. *A Panoramic View of Riemannian Geometry*, Springer 2003, Chapter 9 (pp. 373–429).
> 主题: 把黎曼流形当作"量子力学世界"——研究 Laplace 算子的谱与特征函数。
> Theme: treat a Riemannian manifold as a "quantum-mechanical world" — study the spectrum & eigenfunctions of the Laplacian.

## 关键要点 Key Points

1. **Laplacian 是内蕴的 / The Laplacian is intrinsic.** 在黎曼流形 $(M,g)$ 上 $\Delta f = -\mathrm{trace}_g \mathrm{Hess}\, f$，它只依赖度量 $g$。其谱 $\mathrm{Spec}(M)=\{0=\lambda_0<\lambda_1<\lambda_2<\cdots\}$ 是离散无穷集, $\lambda_k\to\infty$。谱是 $C^0$ 稳健不变量（比 $\Delta$ 本身更稳健）。
2. **Minimax 原理 / Minimax principle.** $\lambda_k = \inf_V \sup_{f\in V} \frac{\int_M\|df\|^2}{\int_M f^2}$（$V$ 跑遍 $k{+}1$ 维子空间）。这是把特征值刻画为 Dirichlet 商的"主轴长度",也是证明谱稳健性的关键工具。
3. **热核渐近 / Heat kernel asymptotics** (Minakshisundaram 1953, McKean–Singer 1967): $K(x,x,t)\sim \frac{1}{(4\pi t)^{d/2}}\sum u_k(x)t^k$, $u_k$ 是曲率及其协变导数的万有多项式。求和后给出 **Weyl 律**: $N(\lambda)\sim \frac{\beta(d)}{(2\pi)^d}\mathrm{Vol}(M)\lambda^{d/2}$。$U_1=\frac16\int\mathrm{scalar}$; 曲面上由 Gauss–Bonnet 即 $\frac{\pi}{3}\chi(M)$ → 谱决定曲面的亏格。
4. **波方程 ↔ 测地流 / Wave equation ↔ geodesic flow** (Chazarain 1974, Duistermaat–Guillemin 1975): 分布 $\sum_k \cos(\sqrt{\lambda_k}\,t)$ 的奇支集恰好出现在周期测地线长度处 → 谱"决定"长度谱。这是"Jacob's ladder": 要看清波, 必须爬到切丛 $UM$ 上用微局部分析。
5. **第一特征值 $\lambda_1$ / First eigenvalue**: Lichnerowicz (Ricci $\ge d{-}1 \Rightarrow \lambda_1\ge d$, 等号仅球面); Cheeger 常数 $h_c$: $\lambda_1>\frac14 h_c^2$; Hersch (球面: $\frac1{\lambda_1}+\frac1{\lambda_2}+\frac1{\lambda_3}\ge\frac{3}{8\pi\,\mathrm{Area}}$)。
6. **反谱问题 / Inverse problems**: "能听出鼓的形状吗?" Milnor 1964 给出否答 (16 维等谱不等距平环); Sunada 1985 给代数充分条件; Colin de Verdière 1987: $\dim\ge3$ 时任意有限谱(含重数)可实现。但负曲率刚性强 (Guillemin–Kazhdan 1980); 标准球面 $\le6$ 维由谱唯一决定 (Tanno 1980)。

## 图表描述 Figures

- **Fig 9.1**: 沿正交测地系取二阶导求和, 给出 $\Delta f(m)$ — 建议用 Mermaid 表达"内蕴二阶导"思想, 但图本身为示意几何图, 不必重绘。
- **Fig 9.2**: Dirichlet 商在无穷维函数空间单位球面上是二次型, 特征函数=椭球主轴 — 适合作为 [[Spectrum-of-Laplacian]] 的 minimax 直觉图。
- **Fig 9.5**: $KP^n$ 谱落在以等差数列为中心的区间内 — 适合在 [[Wave-Equation-Length-Spectrum]] 中以 Mermaid 数轴呈现。
- **Fig 9.7/9.8**: 谱间隙与周期测地线上的波干涉共振 — 适合 Mermaid 流程图, 表达"Jacob's ladder"层级。
- **Fig 9.11**: Colin de Verdière 用图论构造任意有限谱 — 适合 Mermaid 结构图。

## 新概念列表 New Concepts
- [[Spectrum-of-Laplacian]]
- [[Heat-Kernel-Asymptotics]]
- [[Wave-Equation-Length-Spectrum]]
- [[First-Eigenvalue]]
- [[Isospectral-Manifolds]]

## 更新了哪些页面 Pages Updated
（首次 ingest, 无既有页面更新 / First ingest, no existing pages updated）

## 备注 Notes
- 本书约定: 除特别说明, 所有流形紧致连通。
- 本章反复强调"热方程温和但健忘, 波方程锋利但昂贵"——这是贯穿全章的方法论张力。
- 跨课程连接暂无 (wiki 中尚无其他课程)。潜在连接点: 热核 ↔ 概率论/Brown 运动; η 不变量 ↔ 拓扑指标定理; 量子混沌 ↔ 随机矩阵 (GOE)。
