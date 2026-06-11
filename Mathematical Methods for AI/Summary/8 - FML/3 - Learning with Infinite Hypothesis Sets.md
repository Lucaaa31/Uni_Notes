### Motivation

> **Context:** For an infinite hypothesis set the finite-$|H|$ error bounds become uninformative, yet the axis-aligned rectangles example shows efficient learning from a finite sample is still possible. The aim is to reduce the infinite case to a finite one by projecting hypotheses over finite samples, and to find useful complexity measures for infinite hypothesis sets.

### Rademacher Complexity

> **Context:** Rademacher complexity measures how well a hypothesis class can correlate with random noise: the richer the class, the better it fits random $\pm 1$ labels. The empirical version is defined on a fixed sample, and the expected version averages it over samples.

> [!definition] Empirical and Expected Rademacher Complexity
> 
> Let $G$ be a family of functions from $Z$ to $[a,b]$, $S = (z_1, \dots, z_m)$ a sample, and $\sigma_i$ independent **Rademacher variables** uniform on $\{-1, +1\}$.
> 
> $$\widehat{\mathfrak{R}}_S(G) = \mathbb{E}_{\boldsymbol\sigma}\!\left[\sup_{g \in G} \frac{1}{m} \sum_{i=1}^{m} \sigma_i\, g(z_i)\right], \qquad \mathfrak{R}_m(G) = \mathbb{E}_{S \sim D^m}\!\left[\widehat{\mathfrak{R}}_S(G)\right]$$
> 
> The term $\frac{1}{m}\boldsymbol\sigma \cdot (g(z_1),\dots,g(z_m))$ is the correlation with random noise; the supremum picks the $g$ that best aligns with the noise vector.

> [!theorem] Rademacher Generalization Bound (Koltchinskii & Panchenko, 2002)
> 
> Let $G$ be a family of functions from $Z$ to $[0,1]$. Then for any $\delta > 0$, with probability at least $1-\delta$, for all $g \in G$:
> 
> $$\mathbb{E}[g(z)] \le \frac{1}{m}\sum_{i=1}^{m} g(z_i) + 2\mathfrak{R}_m(G) + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}$$
> 
> $$\mathbb{E}[g(z)] \le \frac{1}{m}\sum_{i=1}^{m} g(z_i) + 2\widehat{\mathfrak{R}}_S(G) + 3\sqrt{\frac{\log\frac{2}{\delta}}{2m}}$$
> 
> The first bound is **distribution-dependent**, the second **data-dependent**.

> [!definition] Loss Functions and Hypothesis Set
> 
> Let $H$ take values in $\{-1, +1\}$ and $G$ be the family of **zero-one loss** functions $G = \{(x,y) \mapsto \mathbf{1}_{h(x)\ne y} : h \in H\}$. Then:
> 
> $$\mathfrak{R}_m(G) = \tfrac{1}{2}\,\mathfrak{R}_m(H)$$
> 
> This holds because $-\sigma_i y_i$ has the same distribution as $\sigma_i$, so $y_i$ absorbs into the Rademacher variable and the constant term vanishes under the sup of a symmetric class.

> [!corollary] Rademacher Generalization Bound for $H$
> 
> Let $H$ take values in $\{-1, +1\}$. Then for any $\delta > 0$, with probability at least $1-\delta$, for any $h \in H$:
> 
> $$R(h) \le \widehat{R}(h) + \mathfrak{R}_m(H) + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}$$
> 
> $$R(h) \le \widehat{R}(h) + \widehat{\mathfrak{R}}_S(H) + 3\sqrt{\frac{\log\frac{2}{\delta}}{2m}}$$
> 
> Computing the Rademacher complexity requires solving ERM problems, typically computationally hard, which motivates relating it to easier combinatorial measures.

### Growth Function

> **Context:** The growth function is a combinatorial measure counting the maximum number of distinct labelings (dichotomies) a hypothesis set can realize on $m$ points. It provides a more computable handle on complexity than Rademacher complexity.

> [!definition] Growth Function
> 
> $$\Pi_H(m) = \max_{\{x_1,\dots,x_m\}\subseteq X}\big|\{(h(x_1),\dots,h(x_m)) : h \in H\}\big|$$
> 
> It is the maximum number of distinct dichotomies of $m$ points achievable by $H$, with $\Pi_H(m) \le 2^m$.

> [!theorem] Massart's Lemma
> 
> Let $A \subseteq \mathbb{R}^m$ be finite with $R = \max_{x\in A}\|x\|_2$. Then:
> 
> $$\mathbb{E}_{\boldsymbol\sigma}\!\left[\frac{1}{m}\sup_{x\in A}\sum_{i=1}^{m}\sigma_i x_i\right] \le \frac{R\sqrt{2\log|A|}}{m}$$

> [!corollary] Growth Function Bound on Rademacher Complexity
> 
> Let $G$ take values in $\{-1, +1\}$. Then:
> 
> $$\mathfrak{R}_m(G) \le \sqrt{\frac{2\log \Pi_G(m)}{m}}$$
> 
> It follows from Massart's lemma applied to the projection of $G$ onto the sample, where the vectors have norm $\sqrt{m}$ and number at most $\Pi_G(m)$.

> [!corollary] Growth Function Generalization Bound
> 
> Let $H$ take values in $\{-1, +1\}$. Then for any $\delta > 0$, with probability at least $1-\delta$, for any $h \in H$:
> 
> $$R(h) \le \widehat{R}(h) + \sqrt{\frac{2\log \Pi_H(m)}{m}} + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}$$

### VC Dimension

> **Context:** The VC-dimension reduces the growth function to a single combinatorial number: the size of the largest set that $H$ can fully shatter (realize all $2^m$ dichotomies). It is the central complexity parameter for infinite hypothesis sets.

> [!definition] VC-Dimension
> 
> $$\mathrm{VCdim}(H) = \max\{m : \Pi_H(m) = 2^m\}$$
> 
> The size of the largest set fully shattered by $H$ — a purely combinatorial notion.
> 
> - **Lower bound** ($\geq d$): exhibit *one* set of $d$ points that $H$ shatters (easier).
>     
> - **Upper bound** ($\leq d$): prove that *no* set of $d+1$ points can be shattered (usually harder).
>     

> [!definition] VC-Dimension Examples
> 
> | Hypothesis set | VC-dimension |
> | --- | --- |
> | Intervals on $\mathbb{R}$ | $2$ |
> | Hyperplanes in $\mathbb{R}^d$ | $d+1$ |
> | Axis-aligned rectangles in the plane | $4$ |
> | Convex $d$-gons in the plane | $2d+1$ |
> | Convex polygons (general) | $+\infty$ |
> | Sine functions $\{t\mapsto \sin(\omega t)\}$ | $+\infty$ |

> [!theorem] Sauer's Lemma
> 
> Let $H$ have $\mathrm{VCdim}(H) = d$. Then for all $m \in \mathbb{N}$:
> 
> $$\Pi_H(m) \le \sum_{i=0}^{d}\binom{m}{i}$$

> [!corollary] Sauer's Lemma Consequence
> 
> Let $H$ have $\mathrm{VCdim}(H) = d$. Then for all $m \ge d$:
> 
> $$\Pi_H(m) \le \left(\frac{em}{d}\right)^d = O(m^d)$$

> [!definition] Dichotomy of the Growth Function
> 
> Either:
> 
> - $\mathrm{VCdim}(H) = d < +\infty$ and $\Pi_H(m) = O(m^d)$ (**polynomial**), or
>     
> - $\mathrm{VCdim}(H) = +\infty$ and $\Pi_H(m) = 2^m$ (**exponential**).
>     
> 
> There is **nothing in between** — the growth function never grows like, e.g., $m^{\log m}$.

> [!corollary] VC-Dimension Generalization Bound
> 
> Let $H$ take values in $\{-1,+1\}$ with VC dimension $d$. Then for any $\delta > 0$, with probability at least $1-\delta$, for any $h \in H$:
> 
> $$R(h) \le \widehat{R}(h) + \sqrt{\frac{2d\log\frac{em}{d}}{m}} + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}$$
> 
> General asymptotic form: $R(h) \le \widehat{R}(h) + O\!\left(\sqrt{\frac{\log(m/d)}{m/d}}\right)$.

> [!theorem] Standard VC Bound
> 
> Let $H$ take values in $\{-1,+1\}$ with VC dimension $d$. Then for any $\delta>0$, with probability at least $1-\delta$, for any $h\in H$:
> 
> $$R(h) \le \widehat{R}(h) + \sqrt{\frac{8d\log\frac{2em}{d} + 8\log\frac{4}{\delta}}{m}}$$
> 
> It derives from the growth-function bound via $\Pr[|R(h) - \widehat{R}(h)| > \epsilon] \le 4\,\Pi_H(2m)\exp(-m\epsilon^2/8)$.

### Lower Bounds

> **Context:** Lower bounds show the VC-dimension is not merely a convenient upper-bound tool but the right complexity parameter: no algorithm can do substantially better, both in the realizable and non-realizable settings.

> [!theorem] VCdim Lower Bound — Realizable Case (Ehrenfeucht et al., 1988)
> 
> Let $H$ have VC dimension $d > 1$. Then for any learning algorithm $L$:
> 
> $$\exists D,\ \exists f \in H,\quad \Pr_{S\sim D^m}\!\left[R_D(h_S, f) > \frac{d-1}{32m}\right] \ge \frac{1}{100}$$
> 
> The idea is to choose a distribution on which $L$ can do no better than tossing a coin on the unseen points.

> [!definition] Agnostic PAC-Learnability
> 
> A concept class $C$ is **PAC-learnable** if there exists an algorithm $L$ such that for all $c\in C$, all $\epsilon>0$, $\delta>0$, and all distributions $D$:
> 
> $$\Pr_{S\sim D}\!\left[R(h_S) - \inf_{h\in H} R(h) \le \epsilon\right] \ge 1-\delta$$
> 
> for samples of size $m = \mathrm{poly}(1/\epsilon, 1/\delta)$.

> [!theorem] VCdim Lower Bound — Non-Realizable Case (Anthony & Bartlett, 1999)
> 
> Let $H$ have VC dimension $d > 1$. Then for any learning algorithm $L$, there exists a distribution $D$ over $X\times\{0,1\}$ such that:
> 
> $$\Pr_{S\sim D^m}\!\left[R_D(h_S) - \inf_{h\in H} R_D(h) > \sqrt{\frac{d}{320m}}\right] \ge \frac{1}{64}$$
> 
> Equivalently, the **sample complexity** satisfies $m \ge \dfrac{d}{320\,\epsilon^2}$.

> [!definition] Upper vs. Lower Bounds
> 
> - **Upper bound** (VC corollary): $R(h) \le \widehat{R}(h) + O\!\big(\sqrt{d/m}\big)$.
>     
> - **Lower bound** (non-realizable): need $m = \Omega\!\big(d/\epsilon^2\big)$.
>     
> 
> Together these show the VC-dimension $d$ is the **right** complexity parameter: it controls sample complexity from both directions.
