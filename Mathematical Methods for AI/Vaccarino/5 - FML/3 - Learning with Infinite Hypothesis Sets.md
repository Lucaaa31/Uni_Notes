## Motivation

- With an **infinite** hypothesis set $H$, the error bounds from the previous lecture (finite $|H|$) become **uninformative**.
- Key question: *is efficient learning from a finite sample possible when $H$ is infinite?*
- The **axis-aligned rectangles** example shows it **is** possible.
- Strategy: can we reduce the infinite case to a finite one by **projecting hypotheses over finite samples**?
- Goal: find useful **measures of complexity** for infinite hypothesis sets.

---

## Rademacher Complexity

**Intuition:** measures how well a hypothesis class can **correlate with random noise**. The richer the class, the better it fits random $\pm 1$ labels.

### Empirical Rademacher Complexity

Let $G$ be a family of functions mapping from set $Z$ to $[a,b]$, and let $S = (z_1, \dots, z_m)$ be a sample.

The $\sigma_i$ (**Rademacher variables**) are independent uniform random variables taking values in $\{-1, +1\}$.

$$
\widehat{\mathfrak{R}}_S(G) = \mathbb{E}_{\boldsymbol\sigma}\!\left[\sup_{g \in G} \frac{1}{m} \sum_{i=1}^{m} \sigma_i\, g(z_i)\right]
$$

> [!note] Interpretation
> The dot product $\frac{1}{m}\boldsymbol\sigma \cdot (g(z_1),\dots,g(z_m))$ is the **correlation with random noise**. The supremum picks the $g$ that best aligns with the noise vector.

### Rademacher Complexity

The (expected) Rademacher complexity averages the empirical version over all samples of size $m$:

$$
\mathfrak{R}_m(G) = \mathbb{E}_{S \sim D^m}\!\left[\widehat{\mathfrak{R}}_S(G)\right]
$$

### Rademacher Complexity Bound

> [!theorem] Generalization bound (Koltchinskii & Panchenko, 2002)
> Let $G$ be a family of functions mapping from $Z$ to $[0,1]$. Then for any $\delta > 0$, with probability at least $1-\delta$, the following holds for all $g \in G$:
>
> $$
> \mathbb{E}[g(z)] \le \frac{1}{m}\sum_{i=1}^{m} g(z_i) + 2\mathfrak{R}_m(G) + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}
> $$
>
> $$
> \mathbb{E}[g(z)] \le \frac{1}{m}\sum_{i=1}^{m} g(z_i) + 2\widehat{\mathfrak{R}}_S(G) + 3\sqrt{\frac{\log\frac{2}{\delta}}{2m}}
> $$

The first bound is **distribution-dependent**, the second is **data-dependent**.
> [!success] Proof sketch
Apply **McDiarmid's inequality** to
$$
\Phi(S) = \sup_{g\in G}\Big(\mathbb{E}[g] - \widehat{\mathbb{E}}_S[g]\Big).
$$
**Step 1 — Bounded differences.** Changing one point of $S$ changes $\Phi(S)$ by at most $\frac{1}{m}$:
$$
\begin{aligned}
\Phi(S') - \Phi(S) &= \sup_{g\in G}\{\mathbb{E}[g] - \widehat{\mathbb{E}}_{S'}[g]\} - \sup_{g\in G}\{\mathbb{E}[g] - \widehat{\mathbb{E}}_S[g]\}\\
&\le \sup_{g\in G}\Big(\{\mathbb{E}[g] - \widehat{\mathbb{E}}_{S'}[g]\} - \{\mathbb{E}[g] - \widehat{\mathbb{E}}_S[g]\}\Big)\\
&= \sup_{g\in G}\{\widehat{\mathbb{E}}_S[g] - \widehat{\mathbb{E}}_{S'}[g]\}
= \sup_{g\in G}\frac{1}{m}\big(g(z_m) - g(z'_m)\big) \le \frac{1}{m}.
\end{aligned}
$$
**Step 2 — McDiarmid.** With probability at least $1 - \frac{\delta}{2}$:
$$
\Phi(S) \le \mathbb{E}_S[\Phi(S)] + \sqrt{\frac{\log\frac{2}{\delta}}{2m}}.
$$
**Step 3 — Bound the expectation** via a *ghost sample* $S'$ and symmetrization:
$$
\begin{aligned}
\mathbb{E}_S[\Phi(S)]
&= \mathbb{E}_S\Big[\sup_{g\in G}\big(\mathbb{E}[g] - \widehat{\mathbb{E}}_S(g)\big)\Big]
= \mathbb{E}_S\Big[\sup_{g\in G}\mathbb{E}_{S'}\big[\widehat{\mathbb{E}}_{S'}(g) - \widehat{\mathbb{E}}_S(g)\big]\Big]\\
&\le \mathbb{E}_{S,S'}\Big[\sup_{g\in G}\big(\widehat{\mathbb{E}}_{S'}(g) - \widehat{\mathbb{E}}_S(g)\big)\Big]
&&\text{(sub-additivity of sup)}\\
&= \mathbb{E}_{S,S'}\Big[\sup_{g\in G}\frac{1}{m}\sum_{i=1}^{m}\big(g(z'_i) - g(z_i)\big)\Big]\\
&= \mathbb{E}_{\boldsymbol\sigma,S,S'}\Big[\sup_{g\in G}\frac{1}{m}\sum_{i=1}^{m}\sigma_i\big(g(z'_i) - g(z_i)\big)\Big]
&&\text{(swap } z_i,z'_i\text{)}\\
&\le \mathbb{E}_{\boldsymbol\sigma,S'}\Big[\sup_{g\in G}\frac{1}{m}\sum_{i=1}^{m}\sigma_i g(z'_i)\Big]
+ \mathbb{E}_{\boldsymbol\sigma,S}\Big[\sup_{g\in G}\frac{1}{m}\sum_{i=1}^{m}\sigma_i g(z_i)\Big]\\
&= 2\,\mathbb{E}_{\boldsymbol\sigma,S}\Big[\sup_{g\in G}\frac{1}{m}\sum_{i=1}^{m}\sigma_i g(z_i)\Big]
= 2\mathfrak{R}_m(G).
\end{aligned}
$$
**Step 4 — From $\mathfrak{R}_m$ to $\widehat{\mathfrak{R}}_S$.** Changing one point of $S$ makes $\widehat{\mathfrak{R}}_S(G)$ vary by at most $\frac{1}{m}$, so by McDiarmid again, with probability $\ge 1 - \frac{\delta}{2}$:
$$
\mathfrak{R}_m(G) \le \widehat{\mathfrak{R}}_S(G) + \sqrt{\frac{\log\frac{2}{\delta}}{2m}}.
$$
By the **union bound**, with probability $\ge 1 - \delta$:
$$
\Phi(S) \le 2\widehat{\mathfrak{R}}_S(G) + 3\sqrt{\frac{\log\frac{2}{\delta}}{2m}}. \qquad\blacksquare
$$

### Loss Functions - Hypothesis Set

> [!proposition]
> Let $H$ be a family of functions taking values in $\{-1, +1\}$, and let $G$ be the family of **zero-one loss** functions of $H$:
> $$ G = \{(x,y) \mapsto \mathbf{1}_{h(x)\ne y} : h \in H\}. $$
> Then:
> $$ \mathfrak{R}_m(G) = \tfrac{1}{2}\,\mathfrak{R}_m(H). $$

**Proof.** Using $\mathbf{1}_{h(x_i)\ne y_i} = \frac{1}{2}(1 - y_i h(x_i))$:

$$
\begin{aligned}
\mathfrak{R}_m(G)
&= \mathbb{E}_{S,\boldsymbol\sigma}\Big[\sup_{h\in H}\frac{1}{m}\sum_{i=1}^{m}\sigma_i\,\mathbf{1}_{h(x_i)\ne y_i}\Big] 
= \mathbb{E}_{S,\boldsymbol\sigma}\Big[\sup_{h\in H}\frac{1}{m}\sum_{i=1}^{m}\sigma_i\,\tfrac{1}{2}(1 - y_i h(x_i))\Big]\\
&= \tfrac{1}{2}\,\mathbb{E}_{S,\boldsymbol\sigma}\Big[\sup_{h\in H}\frac{1}{m}\sum_{i=1}^{m}-\sigma_i y_i h(x_i)\Big]
= \tfrac{1}{2}\,\mathbb{E}_{S,\boldsymbol\sigma}\Big[\sup_{h\in H}\frac{1}{m}\sum_{i=1}^{m}\sigma_i h(x_i)\Big].
\end{aligned}
$$

> [!tip] Why this works
> $-\sigma_i y_i$ has the same distribution as $\sigma_i$ (both uniform $\pm1$), so the $y_i$ "absorbs" into the Rademacher variable and the constant term vanishes under the sup of a symmetric class.

### Generalization Bounds — Rademacher

> [!corollary]
> Let $H$ be a family of functions taking values in $\{-1, +1\}$. Then for any $\delta > 0$, with probability at least $1-\delta$, for any $h \in H$:
>
> $$
> R(h) \le \widehat{R}(h) + \mathfrak{R}_m(H) + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}
> $$
>
> $$
> R(h) \le \widehat{R}(h) + \widehat{\mathfrak{R}}_S(H) + 3\sqrt{\frac{\log\frac{2}{\delta}}{2m}}
> $$

> [!warning] Practical issue
> Computing $\mathbb{E}_{\boldsymbol\sigma}\big[\sup_{h\in H}\frac{1}{m}\sum_i \sigma_i h(x_i)\big]$ requires solving **ERM** problems — typically **computationally hard**.
> → Motivates a relation with **combinatorial measures** that are easier to compute.

---

## Growth Function

> [!definition] Growth function
> The growth function $\Pi_H : \mathbb{N} \to \mathbb{N}$ for a hypothesis set $H$ is:
> $$
> \forall m \in \mathbb{N},\quad \Pi_H(m) = \max_{\{x_1,\dots,x_m\}\subseteq X}\big|\{(h(x_1),\dots,h(x_m)) : h \in H\}\big|.
> $$

$\Pi_H(m)$ is the **maximum number of distinct labelings (dichotomies)** of $m$ points achievable using $H$. Note $\Pi_H(m) \le 2^m$.

### Massart's Lemma
 Let $A \subseteq \mathbb{R}^m$ be a finite set with $R = \max_{x\in A}\|x\|_2$. Then:
 $$
 \mathbb{E}_{\boldsymbol\sigma}\!\left[\frac{1}{m}\sup_{x\in A}\sum_{i=1}^{m}\sigma_i x_i\right] \le \frac{R\sqrt{2\log|A|}}{m}.
 $$

> [!success] **Proof.** 
> For any $t > 0$, using Jensen, then sup-as-sum bound, then Hoeffding's inequality on each $\sigma_i x_i \in [-|x_i|, |x_i|]$:
$$
\begin{aligned}
\exp\!\Big(t\,\mathbb{E}_{\boldsymbol\sigma}\big[\sup_{x\in A}\textstyle\sum_i \sigma_i x_i\big]\Big)
&\le \mathbb{E}_{\boldsymbol\sigma}\Big[\exp\big(t\sup_{x\in A}\textstyle\sum_i \sigma_i x_i\big)\Big] &&\text{(Jensen)}\\
&= \mathbb{E}_{\boldsymbol\sigma}\Big[\sup_{x\in A}\exp\big(t\textstyle\sum_i \sigma_i x_i\big)\Big]\\
&\le \sum_{x\in A}\mathbb{E}_{\boldsymbol\sigma}\Big[\exp\big(t\textstyle\sum_i \sigma_i x_i\big)\Big]
= \sum_{x\in A}\prod_{i=1}^{m}\mathbb{E}_{\boldsymbol\sigma}\big[\exp(t\sigma_i x_i)\big]\\
&\le \sum_{x\in A}\exp\!\Big(\frac{\sum_{i=1}^m t^2(2|x_i|)^2}{8}\Big)
\le |A|\,e^{\frac{t^2 R^2}{2}}. &&\text{(Hoeffding)}
\end{aligned}
$$

Taking logs:
$$
\mathbb{E}_{\boldsymbol\sigma}\Big[\sup_{x\in A}\sum_{i=1}^m \sigma_i x_i\Big] \le \frac{\log|A|}{t} + \frac{tR^2}{2}.
$$
Minimizing over $t$ with $t = \dfrac{\sqrt{2\log|A|}}{R}$ gives:
$$
\mathbb{E}_{\boldsymbol\sigma}\Big[\sup_{x\in A}\sum_{i=1}^m \sigma_i x_i\Big] \le R\sqrt{2\log|A|}. \qquad\blacksquare
$$

### Growth Function Bound on Rademacher Complexity

> [!corollary]
> Let $G$ be a family of functions taking values in $\{-1, +1\}$. Then:
> $$
> \mathfrak{R}_m(G) \le \sqrt{\frac{2\log \Pi_G(m)}{m}}.
> $$

> [!success] **Proof.**
> Apply Massart's lemma to the projection of $G$ onto the sample. Here the vectors $(g(z_1),\dots,g(z_m)) \in \{-1,+1\}^m$ have norm $R = \sqrt{m}$, and the number of distinct such vectors is at most $\Pi_G(m)$:
$$
\widehat{\mathfrak{R}}_S(G)
\le \frac{\sqrt{m}\,\sqrt{2\log\big|\{(g(z_1),\dots,g(z_m)) : g\in G\}\big|}}{m}
\le \frac{\sqrt{m}\,\sqrt{2\log\Pi_G(m)}}{m}
= \sqrt{\frac{2\log\Pi_G(m)}{m}}. \quad\blacksquare
$$

### Generalization Bound — Growth Function

> [!corollary]
> Let $H$ be a family of functions taking values in $\{-1, +1\}$. Then for any $\delta > 0$, with probability at least $1-\delta$, for any $h \in H$:
> $$
> R(h) \le \widehat{R}(h) + \sqrt{\frac{2\log \Pi_H(m)}{m}} + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}.
> $$

> [!question] Next problem
> How do we compute the growth function? → relationship with the **VC-dimension** (Vapnik–Chervonenkis dimension).

---

## VC Dimension

> [!definition] VC-dimension
> The VC-dimension of a hypothesis set $H$ is:
> $$
> \mathrm{VCdim}(H) = \max\{m : \Pi_H(m) = 2^m\}.
> $$

The VC-dimension is the size of the **largest set that can be fully shattered** by $H$. It is a purely **combinatorial** notion.

> [!info] Proving VC-dimension $= d$
> - **Lower bound** ($\mathrm{VCdim}(H) \ge d$): exhibit *one* set of $d$ points that $H$ shatters. (Easier.)
> - **Upper bound** ($\mathrm{VCdim}(H) \le d$): prove that *no* set of $d+1$ points can be shattered. (Usually harder.)

### Examples

| Hypothesis set | VC-dimension |
|---|---|
| Intervals on $\mathbb{R}$ | $2$ |
| Hyperplanes in $\mathbb{R}^d$ | $d+1$ |
| Axis-aligned rectangles in the plane | $4$ |
| Convex $d$-gons in the plane | $2d+1$ |
| Convex polygons (general) | $+\infty$ |
| Sine functions $\{t\mapsto \sin(\omega t)\}$ | $+\infty$ |

**Intervals of the real line.**
- Any 2 points can be shattered (four interval types realize all dichotomies).
![[3 - Learning with Infinite Hypothesis Sets 2-1780867650401.webp]]
- No set of 3 points: the dichotomy "$+\,-\,+$" is **not realizable** by a single interval.
![[3 - Learning with Infinite Hypothesis Sets 2-1780867673488.webp]]
- Hence $\mathrm{VCdim} = 2$.

**Hyperplanes in $\mathbb{R}^d$.**
- Any $d+1$ points in general position (e.g. 3 non-collinear points in $\mathbb{R}^2$) can be shattered.
 ![[3 - Learning with Infinite Hypothesis Sets 2-1780867688204.webp]]
- Some dichotomies on $d+2$ points are unrealizable.
![[3 - Learning with Infinite Hypothesis Sets 2-1780867709113.webp]]
- Hence $\mathrm{VCdim} = d+1$.

**Axis-aligned rectangles.**
- A specific set of 4 points can be shattered.
![[3 - Learning with Infinite Hypothesis Sets 2-1780867731954.webp]]
- No set of 5 points: label *negatively* the point not on the bounding sides → unrealizable.
![[3 - Learning with Infinite Hypothesis Sets 2-1780867747433.webp]]
- Hence $\mathrm{VCdim} = 4$.

**Convex polygons.**
- $2d+1$ points placed on a circle can be shattered by a convex $d$-gon (placing points on the circle maximizes realizable dichotomies).
![[3 - Learning with Infinite Hypothesis Sets 2-1780867777130.webp]]
- General convex polygons (unbounded number of vertices) → $\mathrm{VCdim} = +\infty$.

**Sine functions.**
- Any finite set of points on the real line can be shattered by $\{t\mapsto \sin(\omega t):\omega\in\mathbb{R}\}$ for suitable frequencies.
- Hence $\mathrm{VCdim} = +\infty$.
![[3 - Learning with Infinite Hypothesis Sets 2-1780867791431.webp]]

### Sauer's Lemma

> [!theorem] Sauer's Lemma
> Let $H$ be a hypothesis set with $\mathrm{VCdim}(H) = d$. Then for all $m \in \mathbb{N}$:
> $$
> \Pi_H(m) \le \sum_{i=0}^{d}\binom{m}{i}.
> $$

**Proof (induction on $m + d$).** Base cases hold for $m = 1$ with $d = 0$ or $d = 1$. Assume the claim for $(m-1, d-1)$ and $(m-1, d)$.

Fix $S = \{x_1,\dots,x_m\}$ with $\Pi_H(m)$ dichotomies, and let $G = H|_S$ be the set of concepts $H$ induces by restriction to $S$. Define over $S' = \{x_1,\dots,x_{m-1}\}$:
![[3 - Learning with Infinite Hypothesis Sets 2-1780867840683.webp]]

Then $|G_1| + |G_2| = |G|$.

- Since $\mathrm{VCdim}(G_1) \le d$, by induction: $\;|G_1| \le \Pi_{G_1}(m-1) \le \sum_{i=0}^{d}\binom{m-1}{i}.$
- By definition of $G_2$: if a set $Z \subseteq S'$ is shattered by $G_2$, then $Z \cup \{x_m\}$ is shattered by $G$. Thus $\mathrm{VCdim}(G_2) \le \mathrm{VCdim}(G) - 1 = d-1$, giving $\;|G_2| \le \sum_{i=0}^{d-1}\binom{m-1}{i}.$

Combining and using Pascal's rule $\binom{m-1}{i} + \binom{m-1}{i-1} = \binom{m}{i}$:

$$
|G| \le \sum_{i=0}^{d}\binom{m-1}{i} + \sum_{i=0}^{d-1}\binom{m-1}{i}
= \sum_{i=0}^{d}\left[\binom{m-1}{i} + \binom{m-1}{i-1}\right]
= \sum_{i=0}^{d}\binom{m}{i}. \quad\blacksquare
$$

### Sauer's Lemma — Consequence

> [!corollary]
> Let $H$ be a hypothesis set with $\mathrm{VCdim}(H) = d$. Then for all $m \ge d$:
> $$
> \Pi_H(m) \le \left(\frac{em}{d}\right)^d = O(m^d).
> $$

> [!success] **Proof.** For $m \ge d$, $\left(\frac{m}{d}\right)^{d-i} \ge 1$, so:
$$
\begin{aligned}
\sum_{i=0}^{d}\binom{m}{i}
&\le \sum_{i=0}^{d}\binom{m}{i}\left(\frac{m}{d}\right)^{d-i}
\le \sum_{i=0}^{m}\binom{m}{i}\left(\frac{m}{d}\right)^{d-i}\\
&= \left(\frac{m}{d}\right)^{d}\sum_{i=0}^{m}\binom{m}{i}\left(\frac{d}{m}\right)^{i}
= \left(\frac{m}{d}\right)^{d}\left(1 + \frac{d}{m}\right)^{m}
\le \left(\frac{m}{d}\right)^{d}e^{d}. \quad\blacksquare
\end{aligned}
$$

 Remarkable dichotomy of the growth function
 Either:
 - $\mathrm{VCdim}(H) = d < +\infty$ and $\Pi_H(m) = O(m^d)$ (**polynomial**), or
 - $\mathrm{VCdim}(H) = +\infty$ and $\Pi_H(m) = 2^m$ (**exponential**).

 There is **nothing in between** — the growth function never grows like, e.g., $m^{\log m}$.

### Generalization Bound — VC Dimension

> [!corollary]
> Let $H$ be a family of functions taking values in $\{-1,+1\}$ with VC dimension $d$. Then for any $\delta > 0$, with probability at least $1-\delta$, for any $h \in H$:
> $$
> R(h) \le \widehat{R}(h) + \sqrt{\frac{2d\log\frac{em}{d}}{m}} + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}.
> $$

**Proof.** Combine the growth-function bound with Sauer's lemma.
General asymptotic form:
$$
R(h) \le \widehat{R}(h) + O\!\left(\sqrt{\frac{\log(m/d)}{m/d}}\right).
$$

### Comparison — Standard VC Bound

> [!theorem] Standard VC Bound 
> Let $H$ be a family of functions taking values in $\{-1,+1\}$ with VC dimension $d$. Then for any $\delta>0$, with probability at least $1-\delta$, for any $h\in H$:
> $$
> R(h) \le \widehat{R}(h) + \sqrt{\frac{8d\log\frac{2em}{d} + 8\log\frac{4}{\delta}}{m}}.
> $$

> [!success] Proof:
Derived from the growth-function bound via:
$$
\Pr\!\left[\big|R(h) - \widehat{R}(h)\big| > \epsilon\right] \le 4\,\Pi_H(2m)\exp\!\left(-\frac{m\epsilon^2}{8}\right).
$$

---

## Lower Bounds

### VCdim Lower Bound — Realizable Case

> [!theorem] Ehrenfeucht et al., 1988
> Let $H$ be a hypothesis set with VC dimension $d > 1$. Then for any learning algorithm $L$:
> $$
> \exists D,\ \exists f \in H,\quad \Pr_{S\sim D^m}\!\left[R_D(h_S, f) > \frac{d-1}{32m}\right] \ge \frac{1}{100}.
> $$

> [!success] **Proof idea:** choose a distribution $D$ such that $L$ can do no better than **tossing a coin** for some points.
**Construction.** Let $X = \{x_0, x_1, \dots, x_{d-1}\}$ be a fully shattered set. For any $\epsilon > 0$, define $D$ on $X$ by concentrating most mass on $x_0$ and spreading a little over the rest:
$$
\Pr_D[x_0] = 1 - 8\epsilon, \qquad \forall i\in[1,d-1],\ \Pr_D[x_i] = \frac{8\epsilon}{d-1}.
$$
**WLOG** $L$ makes no error on $x_0$ (it has overwhelming mass).
For a sample $S$, let $\overline{S}$ be its elements falling in $X_1 = \{x_1,\dots,x_{d-1}\}$, and let $\mathcal{S}$ be the set of samples of size $m$ with at most $(d-1)/2$ points in $X_1$.
**Averaging over a uniform random labeling $f \sim U$.** Fix $S \in \mathcal{S}$. Using $|X - \overline{S}| \ge (d-1)/2$:
$$
\begin{aligned}
\mathbb{E}_{f\sim U}[R_D(h_S, f)]
&= \sum_f \sum_{x\in X} \mathbf{1}_{h(x)\ne f(x)}\Pr[x]\Pr[f]
\ge \sum_f \sum_{x\notin \overline{S}} \mathbf{1}_{h(x)\ne f(x)}\Pr[x]\Pr[f]\\
&= \sum_{x\notin\overline{S}}\Big(\sum_f \mathbf{1}_{h(x)\ne f(x)}\Pr[f]\Big)\Pr[x]
= \frac{1}{2}\sum_{x\notin\overline{S}}\Pr[x]
\ge \frac{1}{2}\cdot\frac{d-1}{2}\cdot\frac{8\epsilon}{d-1} = 2\epsilon.
\end{aligned}
$$
(On unseen points the best any $h$ can do against a uniform random label is be wrong with probability $\tfrac12$.)
**Extracting a fixed bad labeling.** Since $\mathbb{E}_{S,f\sim U}[R_D(h_S,f)] \ge 2\epsilon$, there exists a labeling $f_0$ with $\mathbb{E}_S[R_D(h_S,f_0)] \ge 2\epsilon$. Since $\Pr_D[X-\{x_0\}] \le 8\epsilon$, we also have $R_D(h_S,f_0)\le 8\epsilon$. Therefore:
$$
2\epsilon \le \mathbb{E}_S[R_D(h_S,f_0)] \le 8\epsilon\Pr_{S\in\mathcal{S}}[R_D\ge\epsilon] + \big(1-\Pr_{S\in\mathcal{S}}[R_D\ge\epsilon]\big)\epsilon.
$$
Collecting terms:
$$
\Pr_{S\in\mathcal{S}}[R_D(h_S,f_0)\ge\epsilon] \ge \frac{1}{7\epsilon}(2\epsilon-\epsilon) = \frac{1}{7}.
$$
So over **all** samples:
$$
\Pr_S[R_D(h_S,f_0)\ge\epsilon] \ge \Pr_{S\in\mathcal{S}}[R_D\ge\epsilon]\Pr[\mathcal{S}] \ge \frac{1}{7}\Pr[\mathcal{S}].
$$
**Lower bound $\Pr[\mathcal{S}]$ via Chernoff.** The probability that more than $(d-1)/2$ points fall in $X_1$ satisfies, for any $\gamma > 0$:
$$
1 - \Pr[\mathcal{S}] = \Pr[S_m \ge 8\epsilon m(1+\gamma)] \le e^{-8\epsilon m \frac{\gamma^2}{3}}.
$$
With $\epsilon = \frac{d-1}{32m}$ and $\gamma = 1$:
$$
\Pr[S_m \ge \tfrac{d-1}{2}] \le e^{-(d-1)/12} \le e^{-1/12} \le 1 - 7\delta \quad\text{for } \delta \le .01.
$$
Hence $\Pr[\mathcal{S}] \ge 7\delta$ and finally:
$$
\Pr_S[R_D(h_S,f_0) \ge \epsilon] \ge \delta. \qquad\blacksquare
$$

### Agnostic PAC Model

> [!definition] PAC-learnability
> A concept class $C$ is **PAC-learnable** if there exists a learning algorithm $L$ such that for all $c\in C$, all $\epsilon>0$, $\delta>0$, and all distributions $D$:
> $$
> \Pr_{S\sim D}\!\left[R(h_S) - \inf_{h\in H} R(h) \le \epsilon\right] \ge 1-\delta,
> $$
> for samples of size $m = \mathrm{poly}(1/\epsilon, 1/\delta)$, for a fixed polynomial.

### VCdim Lower Bound — Non-Realizable Case

> [!theorem] Anthony & Bartlett, 1999
> Let $H$ be a hypothesis set with VC dimension $d > 1$. Then for any learning algorithm $L$, there exists a distribution $D$ over $X\times\{0,1\}$ such that:
> $$
> \Pr_{S\sim D^m}\!\left[R_D(h_S) - \inf_{h\in H} R_D(h) > \sqrt{\frac{d}{320m}}\right] \ge \frac{1}{64}.
> $$

Equivalently, the **sample complexity** satisfies:
$$
m \ge \frac{d}{320\,\epsilon^2}.
$$

> [!summary] Upper vs lower bounds
> - **Upper bound** (VC corollary): $R(h) \le \widehat{R}(h) + O\!\big(\sqrt{d/m}\big)$.
> - **Lower bound** (non-realizable): need $m = \Omega\!\big(d/\epsilon^2\big)$.
> Together these show the VC-dimension $d$ is the **right** complexity parameter: it controls sample complexity from both directions.

