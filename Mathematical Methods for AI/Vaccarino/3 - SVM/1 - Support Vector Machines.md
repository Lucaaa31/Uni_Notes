## Binary Classification Problem

**Training data:** sample drawn i.i.d. from $X \subseteq \mathbb{R}^N$ according to some distribution $D$: $$S = \big((x_1, y_1), \dots, (x_m, y_m)\big) \in X \times \{-1, +1\}.$$

**Problem:** find hypothesis $h : X \to \{-1, +1\}$ in $H$ (a classifier) with small generalization error $R(h)$.

- Choice of hypothesis set $H$ → learning guarantees from the previous lecture.
- Use **linear classification (hyperplanes)** if the dimension $N$ is not too large.

---

## Separable Case

### Linear Separation
![[1 - Support Vector Machines-1780691167374.webp|444]]
Classifiers: $$H = \{x \mapsto \operatorname{sgn}(w \cdot x + b) : w \in \mathbb{R}^N,\ b \in \mathbb{R}\}.$$
**Geometric margin** of the hyperplane: $$\rho = \min_{i \in [1,m]} \frac{|w \cdot x_i + b|}{\|w\|}.$$

> [!question] Which separating hyperplane?
>  Among all hyperplanes that separate the data, pick the one that **maximizes the margin**.
### Optimal (Max-Margin) Hyperplane
![[1 - Support Vector Machines-1780691245043.webp]]
$$\rho = \max_{\substack{w,b: \ y_i(w\cdot x_i + b)\ \ge\ 0}} \ \min_{i \in [1,m]} \frac{|w \cdot x_i + b|}{\|w\|}.$$

The supporting hyperplanes are $w\cdot x + b = \pm 1$, with the decision boundary at $w\cdot x + b = 0$.

### Deriving the Maximum Margin

Using **scale-invariance** we can normalize so that $\min_i |w\cdot x_i + b| = 1$:
![[1 - Support Vector Machines-1780691581760.webp]]

### Primal Optimization Problem

Constrained optimization (primal) $$\min_{w,b} \ \frac{1}{2}\|w\|^2$$ $$\text{subject to } \ y_i(w\cdot x_i + b) \ge 1, \quad i \in [1,m].$$
**Properties:**
- Convex optimization.
- **Unique solution** for a linearly separable sample.

---

## Optimal Hyperplane Equations

**Lagrangian:** for all $w, b, \alpha_i \ge 0$, $$L(w,b,\alpha) = \frac{1}{2}\|w\|^2 - \sum_{i=1}^m \alpha_i\big[y_i(w\cdot x_i + b) - 1\big].$$
**KKT conditions:** $$\nabla_w L = w - \sum_{i=1}^m \alpha_i y_i x_i = 0 \iff \boxed{w = \sum_{i=1}^m \alpha_i y_i x_i}$$ $$\nabla_b L = -\sum_{i=1}^m \alpha_i y_i = 0 \iff \boxed{\sum_{i=1}^m \alpha_i y_i = 0}$$ $$\forall i \in [1,m], \quad \boxed{\alpha_i\big[y_i(w\cdot x_i + b) - 1\big] = 0} \quad \text{(complementarity)}$$

### Support Vectors
**Complementarity** implies: 
$$
\alpha_i\big[y_i(w\cdot x_i + b) - 1\big] = 0 \implies \alpha_i = 0 ∨ y_i(w\cdot x_i + b) = 1.
$$
Support vectors: vectors $x_i$ such that
$$\alpha_i \neq 0 ∧ y_i(w\cdot x_i + b) = 1.$$

**Support vectors** are the points $x_i$ lying exactly on the margin (i.e. $y_i(w\cdot x_i+b)=1$).

> [!info] Note Support vectors are **not unique**.

---

## Moving to the Dual

Plugging $w = \sum_i \alpha_i y_i x_i$ back into $L$:

$$ L = \underbrace{\frac{1}{2}\Big\|\sum_{i=1}^m \alpha_i y_i x_i\Big\|^2 - \sum_{i,j=1}^m \alpha_i\alpha_j y_i y_j (x_i\cdot x_j)}_{-\frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j (x_i\cdot x_j)} \ -\ \underbrace{\sum_{i=1}^m \alpha_i y_i b}_{0} \ +\ \sum_{i=1}^m \alpha_i. $$

Thus: $$\boxed{L = \sum_{i=1}^m \alpha_i - \frac{1}{2}\sum_{i,j=1}^m \alpha_i\alpha_j y_i y_j (x_i \cdot x_j)}$$

### Equivalent Dual Optimization Problem

Constrained optimization:
$$\max_{\alpha} \ \sum_{i=1}^m \alpha_i - \frac{1}{2}\sum_{i,j=1}^m \alpha_i\alpha_j y_i y_j (x_i\cdot x_j)$$ $$\text{subject to: } \ \alpha_i \ge 0 \ \wedge\ \sum_{i=1}^m \alpha_i y_i = 0, \quad i \in [1,m].$$
**Solution:** $$h(x) = \operatorname{sgn}\Big(\sum_{i=1}^m \alpha_i y_i (x_i\cdot x) + b\Big),$$ $$\text{with } \ b = y_i - \sum_{j=1}^m \alpha_j y_j (x_j \cdot x_i) \ \text{ for any SV } x_i.$$

---

## Leave-One-Out Error

> [!definition] Leave-one-out error 
> Let $h_S$ be the hypothesis output by learning algorithm $L$ on a sample $S$ of size $m$. Then: $$\widehat{R}_{\text{loo}}(L) = \frac{1}{m}\sum_{i=1}^m \mathbf{1}_{h_{S-{x_i}}(x_i) \neq f(x_i)}.$$

**Property:** it is an **unbiased estimate** of the expected error of a hypothesis trained on a sample of size $m-1$: $$ \mathbb{E}_{S\sim D^m}\big[\widehat{R}_{\text{loo}}(L)\big] = \frac{1}{m}\sum_{i=1}^m \mathbb{E}_S\big[\mathbf{1}_{h_{S-{x_i}}(x_i)\neq f(x_i)}\big] = \mathbb{E}_{S'\sim D^{m-1}}\big[R(h_{S'})\big]. $$
### Leave-One-Out Analysis

> [!theorem] LOO bound for the optimal hyperplane 
> Let $h_S$ be the optimal hyperplane for a sample $S$ and $N_{\text{SV}}(S)$ the number of support vectors defining $h_S$. Then: $$\mathbb{E}_{S\sim D^m}\big[R(h_S)\big] \le \mathbb{E}_{S\sim D^{m+1}}\left[\frac{N_{\text{SV}}(S)}{m+1}\right].$$

**Proof idea:** Let $S \sim D^{m+1}$ be linearly separable and $x \in S$. If $h_{S-{x}}$ misclassifies $x$, then $x$ must be a SV for $h_S$. Therefore: $$\widehat{R}_{\text{loo}}(\text{opt.-hyp.}) \le \frac{N_{\text{SV}}(S)}{m+1}.$$

> [!warning] Notes
> 
> - Bound is on the **expectation** of the error only, not on the probability of error.
> - The argument is based on **sparsity** (number of support vectors). Later, margin-based arguments give further justification.

---

# Non-Separable Case

## Soft-Margin SVMs

**Problem:** real data is often **not linearly separable**. For any hyperplane there exists some $x_i$ with $$y_i(w\cdot x_i + b) \cancel{\ge} 1.$$
**Idea:** relax the constraints with **slack variables** $\xi_i \ge 0$: $$y_i[w\cdot x_i + b] \ge 1 - \xi_i.$$
### Soft-Margin Hyperplanes
![[1 - Support Vector Machines-1780692633244.webp]]
- Support vectors are now either points **on the margin** or **outliers**.
- Soft margin: $\rho = \frac{1}{\|w\|}$.
### Primal Optimization Problem

>Constrained optimization (soft margin) $$\min_{w,b,\xi} \ \frac{1}{2}\|w\|^2 + C\sum_{i=1}^m \xi_i$$ $$\text{subject to } \ y_i(w\cdot x_i + b) \ge 1 - \xi_i \ \wedge\ \xi_i \ge 0, \quad i \in [1,m].$$

**Properties:**
- $C \ge 0$ is the **trade-off parameter** (margin maximization vs. training-error minimization).
- Convex optimization.
- Unique solution.

> [!warning] On choosing $C$
> 
> - $C$ balances maximizing the margin against minimizing training error. It is typically chosen by cross-validation.
> - The general problem of finding a hyperplane minimizing training error is **NP-complete** (as a function of the dimension).
> - Other convex functions of the slack variables can be used; this choice (and the squared-slack variant) yields convenient formulations.

## SVM — Equivalent (Unconstrained) Problem

$$\min_{w,b} \ \frac{1}{2}\|w\|^2 + C\sum_{i=1}^m \big(1 - y_i(w\cdot x_i + b)\big)_+.$$

**Loss functions:**

- **Hinge loss:** $\quad L(h(x), y) = (1 - yh(x))_+$
- **Quadratic hinge loss:** $\quad L(h(x), y) = (1 - yh(x))_+^2$

> [!info] Hinge Loss intuition
> ![[1 - Support Vector Machines-1780692757687.webp]]
The 0/1 loss is a step; the **hinge loss** $\xi^1$ is its convex (piecewise-linear) upper bound, and the **quadratic hinge loss** $\xi^2$ is a smooth convex upper bound. Both are surrogates that make optimization tractable while bounding the 0/1 loss.

---

## SVM Equations (Soft Margin)

**Lagrangian:** for all $w, b, \alpha_i \ge 0, \beta_i \ge 0$, $$L(w,b,\xi,\alpha,\beta) = \frac{1}{2}\|w\|^2 + C\sum_{i=1}^m \xi_i - \sum_{i=1}^m \alpha_i\big[y_i(w\cdot x_i + b) - 1 + \xi_i\big] - \sum_{i=1}^m \beta_i \xi_i.$$
**KKT conditions:** $$\nabla_w L = w - \sum_{i=1}^m \alpha_i y_i x_i = 0 \iff \boxed{w = \sum_{i=1}^m \alpha_i y_i x_i}$$ $$\nabla_b L = -\sum_{i=1}^m \alpha_i y_i = 0 \iff \boxed{\sum_{i=1}^m \alpha_i y_i = 0}$$ $$\nabla_{\xi_i} L = C - \alpha_i - \beta_i = 0 \iff \boxed{\alpha_i + \beta_i = C}$$ $$\forall i\in[1,m], \quad \boxed{\alpha_i\big[y_i(w\cdot x_i+b) - 1 + \xi_i\big] = 0}, \qquad \boxed{\beta_i \xi_i = 0}$$

---

# Margin Guarantees

## High-Dimension

**Learning guarantee** for hyperplanes in dimension $N$: with probability at least $1-\delta$, $$R(h) \le \widehat{R}(h) + \sqrt{\frac{2(N+1)\log\frac{em}{N+1}}{m}} + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}.$$

> [!warning] Problem with the dimension-based bound
> 
> - The bound is **uninformative for $N \approx m$** (or $N \ge m$).
> - Yet SVMs are remarkably successful in high dimension.
> - **Can we justify this theoretically?** → analyze the underlying real-valued scoring function (margin-based analysis).

## Confidence Margin

> [!definition] Confidence margin
> The confidence margin of a real-valued function $h$ at $(x,y) \in X \times Y$ is $$\rho_h(x,y) = yh(x).$$

- $|h(x)|$ is interpreted as the hypothesis' **confidence** in its prediction.
- If correctly classified, $|\rho_h(x,y)|$ coincides with $|h(x)|$.
- **Relationship with geometric margin** for linear $h : x \mapsto w\cdot x + b$, for $x$ in the sample: $$|\rho_h(x,y)| \le \rho_{\text{geom}}\|w\|.$$

### Confidence Margin Loss

> [!definition] $\rho$-margin loss
> For a confidence margin parameter $\rho > 0$, the $\rho$-margin loss function $\Phi_\rho$ ramps linearly from $1$ (at $y h(x) \le 0$) down to $0$ (at $y h(x) \ge \rho$).

![[1 - Support Vector Machines-1780693022207.webp|432]]
For a sample $S = (x_1, \dots, x_m)$ and real-valued hypothesis $h$, the **empirical margin loss** is: $$\widehat{R}_\rho(h) = \frac{1}{m}\sum_{i=1}^m \Phi_\rho\big(y_i h(x_i)\big) \le \frac{1}{m}\sum_{i=1}^m \mathbf{1}_{y_i h(x_i) < \rho}.$$

---

## General Margin Bound

> [!theorem] General margin bound 
>Let $H$ be a set of real-valued functions. Fix $\rho > 0$. For any $\delta > 0$, with probability at least $1-\delta$, for all $h \in H$: $$R(h) \le \widehat{R}_\rho(h) + \frac{2}{\rho}\mathfrak{R}_m(H) + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}$$ $$R(h) \le \widehat{R}_\rho(h) + \frac{2}{\rho}\widehat{\mathfrak{R}}_S(H) + 3\sqrt{\frac{\log\frac{2}{\delta}}{2m}}.$$

**Proof sketch:**
1. Let $\widetilde{H} = \{z = (x,y) \mapsto yh(x) : h \in H\}$ and consider $$\widetilde{\mathcal{H}} = \{\Phi_\rho \circ f : f \in \widetilde{H}\}$$, with values in $[0,1]$.
2. By the Rademacher bound, with probability $\ge 1-\delta$, for all $g \in \widetilde{\mathcal{H}}$: $$\mathbb{E}[g(z)] \le \frac{1}{m}\sum_{i=1}^m g(z_i) + 2\mathfrak{R}_m(\widetilde{\mathcal{H}}) + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}.$$
3. Hence: $$\mathbb{E}[\Phi_\rho(y h(x))] \le \widehat{R}_\rho(h) + 2\mathfrak{R}_m(\Phi_\rho \circ \widetilde{H}) + \sqrt{\frac{\log\frac{1}{\delta}}{2m}}.$$
4. Since $\Phi_\rho$ is $\tfrac{1}{\rho}$-Lipschitz, by **Talagrand's lemma**: $$\mathfrak{R}_m(\Phi_\rho \circ \widetilde{H}) \le \frac{1}{\rho}\mathfrak{R}_m(\widetilde{H}) = \frac{1}{\rho m}\mathbb{E}_{\sigma,S}\Big[\sup_{h\in H}\sum_{i=1}^m \sigma_i y_i h(x_i)\Big] = \frac{1}{\rho}\mathfrak{R}_m(H).$$
5. Since $\mathbf{1}_{y h(x) < 0} \le \Phi_\rho(y h(x))$, the first statement follows; the second is analogous. 

---

## Rademacher Complexity of Linear Hypotheses

> [!theorem] Rademacher complexity of linear classifiers 
> Let $S \subseteq \{x : \|x\| \le R\}$ be a sample of size $m$ and $H = \{x \mapsto w\cdot x : \|w\| \le \Lambda\}$. Then: $$\widehat{\mathfrak{R}}_S(H) \le \sqrt{\frac{R^2 \Lambda^2}{m}}.$$

**Proof:** $$ \begin{aligned} \widehat{\mathfrak{R}}_S(H) &= \frac{1}{m}\mathbb{E}_\sigma\Big[\sup_{\|w\|\le\Lambda}\sum_{i=1}^m \sigma_i w\cdot x_i\Big] = \frac{1}{m}\mathbb{E}_\sigma\Big[\sup_{\|w\|\le\Lambda} w\cdot \sum_{i=1}^m \sigma_i x_i\Big] \\ &\le \frac{\Lambda}{m}\mathbb{E}_\sigma\Big[\Big\|\sum_{i=1}^m \sigma_i x_i\Big\|\Big] \le \frac{\Lambda}{m}\Big[\mathbb{E}_\sigma\Big[\Big\|\sum_{i=1}^m \sigma_i x_i\Big\|^2\Big]\Big]^{1/2} \\ &\le \frac{\Lambda}{m}\Big[\mathbb{E}_\sigma\Big[\sum_{i=1}^m \|x_i\|^2\Big]\Big]^{1/2} \le \frac{\Lambda\sqrt{m R^2}}{m} = \sqrt{\frac{R^2 \Lambda^2}{m}}.\end{aligned} $$

---

## Margin Bound — Linear Classifiers

> [!theorem] Corollary (margin bound for linear classifiers) 
> Let $\rho > 0$ and $H = \{x \mapsto w\cdot x : \|w\| \le \Lambda\}$. 
> Assume $X \subseteq \{x : \|x\| \le R\}$. Then, for any $\delta > 0$, with probability at least $1-\delta$, for any $h \in H$: $$R(h) \le \widehat{R}_\rho(h) + 2\sqrt{\frac{R^2 \Lambda^2/\rho^2}{m}} + 3\sqrt{\frac{\log\frac{2}{\delta}}{2m}}.$$

**Proof:** Follows directly from the general margin bound and the Rademacher bound $\widehat{\mathfrak{R}}_S(H)$ for linear classifiers. 

### High-Dimensional Feature Space

> [!important] Key observations
> 
> - The generalization bound does **not** depend on the dimension, only on the **margin** ($R\Lambda/\rho$).
> - This suggests seeking a **large-margin hyperplane in a higher-dimensional feature space**.

**Computational problems:**

- Taking dot products in a high-dimensional feature space can be very costly.
- Solution based on **kernels**.



