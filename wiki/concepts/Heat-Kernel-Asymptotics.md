---
tags: [concept, riemann, differential-geometry, analysis]
courses: [Riemann]
confidence: low
last_reviewed: 2026-07-28
created_at: 2026-07-28
---
# 热核渐近 Heat Kernel Asymptotics

## 直觉 Intuition

在 $M$ 上一点放一个热量 $\delta$ 函数, 看温度如何扩散。$K(x,y,t)$ = 时刻 $t$、点 $y$ 处的温度。Berger 的核心洞察: **短期行为是万有的**——只被曲率及其导数的多项式控制; **长期求和给出 Weyl 律**, 把体积从谱里读出来。热方程温和、健忘: 它能告诉你"大体形状"(体积、第一项曲率), 却记不住精细结构(测地流、间隙)——那是 [[Wave-Equation-Length-Spectrum]] 的工作。

## 详细解释 Detailed

### 热核定理 Heat kernel theorem
**Thm 165 (Minakshisundaram 1953, McKean–Singer 1967):** 紧致黎曼流形上存在 $K:M\times M\times\mathbb{R}_+^*\to\mathbb{R}$, $C^\infty$, 满足
1. 热方程 $-\partial_t F=\Delta F$, $F(x,0)=f(x)$ 的解为 $F(x)=\int_M K(x,y,t)f(y)\,dy$;
2. $K(x,y,t)=\sum_i e^{-\lambda_i t}\phi_i(x)\phi_i(y)$ (收敛级数);
3. 当 $t\to0$: $K(x,x,t)\sim \frac{1}{(4\pi t)^{d/2}}\sum_{k=0}^\infty u_k(x)t^k$, 其中 $u_k$ 是由 $x$ 处曲率张量及其协变导数给出的**万有公式**。

$u_k$ 的构造用 Élie Cartan 的法坐标哲学: Jacobi 方程可任意次求导, 结果只含曲率张量及其各阶协变导数的万有多项式。证明用 parametrix $S_0=\frac{1}{(4\pi t)^{d/2}}e^{-d(x,y)^2/4t}$, 逐步修正 $S_k$, 再用时空双重卷积"忘掉"截断函数 $\eta$, 要求 $k>d/2$ (Sobolev)。

### Weyl 律 Weyl's law
对 (3) 积分, 用 (2): $\sum_k e^{-\lambda_k t}\sim \frac{1}{(4\pi t)^{d/2}}\mathrm{Vol}(M)$ ($t\to\infty$)。Hardy–Littlewood–Karamata Tauber 定理给出计数函数
$$N(\lambda)=\#\{\lambda_i<\lambda\}=\frac{\beta(d)}{(2\pi)^d}\mathrm{Vol}(M,g)\lambda^{d/2}+o(\lambda^{d/2}),$$
等价地 $\lambda_k\sim\big(\frac{(2\pi)^d}{\beta(d)\mathrm{Vol}(M,g)}\big)^{2/d}k^{2/d}$。**谱决定 $\dim M$ 与 $\mathrm{Vol}(M)$。**

### 热不变量 Heat invariants
$$\sum_k e^{-\lambda_k t}\sim \frac{1}{(4\pi t)^{d/2}}\big(\mathrm{Vol}(M,g)+U_1 t+U_2 t^2+\cdots\big),\quad U_k=\int_M u_k.$$
- $u_1=\frac16\,\mathrm{scalar}$; 曲面上 $\frac16\int\mathrm{scalar}=\frac{\pi}{3}\chi(M)$ (Gauss–Bonnet) → **谱决定曲面亏格**。
- $u_2=\frac1{360}(2\|R\|^2-2\|\mathrm{Ricci}\|^2+5\,\mathrm{scalar}^2)$。
- $U_2$ **不是** 4 维拓扑不变量(与 4 维 Gauss–Bonnet $\chi=\frac1{32\pi^2}\int(\|R\|^2-4\|\mathrm{Ricci}\|^2+\mathrm{scalar}^2)$ 系数不符)。
- **Thm 167 (Lohkamp 1996):** $\dim\ge3$ 时, 可在固定体积与固定 $\int\mathrm{scalar}$ 下让谱前 $m$ 项任意指定, 同时 $U_{2k}\to+\infty$, $U_{2k+1}\to-\infty$ → $U_k$ 几乎无用, 负 Ricci 假设也极弱。

### Ricci 下界与下界 Lower bound via Ricci
**Thm 171–172 (Bérard–Besson–Gallot 1985):** 存在万能常数 $c=c(\inf\mathrm{Ricci},d,\mathrm{diam})$ 使
$$Z_M(t)\le\mathrm{Vol}(M)\sup_{x,y}K_M(x,y,t)\le Z_{S^d}(ct),$$
进而 $\lambda_k\ge \mathrm{univ}(\inf\mathrm{Ricci},d,\mathrm{diam})\,k^{2/d}$。证明是 Faber–Krahn 等周对称化的推广: 对整个三元热核对称化, 移植到以 $\inf\mathrm{Ricci}$ 定半径的比较球, 再用抛物最大值原理 + Sturm–Liouville。**与 Thm 164 的上界一起, $\lambda_k$ 被夹在两条 $k^{2/d}$ 渐近线之间。** 负 Ricci 时比较球非最优。

### 其他
- **Varadhan 公式 (Thm 168):** $\lim_{t\to0}t\log K(x,y,t)=-\frac{d(x,y)^2}{2}$ (割迹处行为剧变, 见 Malliavin–Stroock)。
- Brown 运动"传播速度 = Ricci 曲率"。

## 为什么重要 Why

- 热核是流形上 Fourier 分析的存在性基石, 也给出"谱 ↔ 几何"的第一批可读信息(体积、亏格)。
- Weyl 律是所有谱渐近的基准线; $U_k$ 是判断"谱含多少曲率信息"的标尺(答案: 比期望的少)。
- 下界 (Thm 172) 与 [[Spectrum-of-Laplacian]] 的上界 (Thm 164) 配对, 给出 $\lambda_k$ 的完整夹逼。

## 连接 Connections
- ← [[Spectrum-of-Laplacian]]: 提供 minimax 的上界; 本页补下界。
- → [[Wave-Equation-Length-Spectrum]]: 热方程只能到 $o(\cdot)$, 波方程把误差改进到 $O(\lambda^{(d-1)/2})$。
- → [[Isospectral-Manifolds]]: $U_k$ 用于唯一性证明(球面 $\le6$ 维; 平环)。
- 跨课程: 概率论 Brown 运动 / 随机分析 (Malliavin); 物理半经典极限 $\hbar\to0$。

## 来源 Sources
- Berger Ch.9 §§9.7 (pp. 393–401). 见 [[Riemann-ch9]]。
- Berline–Getzler–Vergne 1992 [179]; Gilkey 1995 [564]; Bérard 1986 [135].

## 待解决 Open Questions
- $U_k$ ($k\ge2$) 的几何/拓扑意义基本丧失(Lohkamp)。能否找到谱中其他更有用的曲率信息?
- 负 Ricci 时 Thm 172 的最优显式常数未知。
