> [!definition] **Dimensionality reduction** = taking data in a **high-dimensional** space and mapping it into a **low-dimensional** space.
> 
> **Why do it?**
> 
> - Reduces training (and testing) time
> - Reduces estimation error
> - Interpretability: finding meaningful structure in data, illustration / visualization
> 
> **Linear dimensionality reduction:** $x \mapsto Wx$ where $W \in \mathbb{R}^{n,d}$ and $n < d$.


---

## 1. Principal Component Analysis (PCA)

### Setup
We map $$x \mapsto Wx$$What makes $W$ a good matrix for dimensionality reduction?
**Natural criterion:** we want to be able to approximately **recover** $x$ from $y = Wx$.

PCA uses **linear recovery**: $$ \tilde{x} = Uy = UWx $$
### The PCA optimization problem
$$ \underset{W \in \mathbb{R}^{n,d}, U \in \mathbb{R}^{d,n}}{\arg\min} \sum_{i=1}^{m} \lVert x_i - UWx_i \rVert^2 $$

> [!theorem] Solution to PCA
>  Let $A = \sum_{i=1}^{m} x_i x_i^\top$ and let $u_1, \dots, u_n$ be the $n$ **leading eigenvectors** of $A$. Then the solution to the PCA problem is to set the columns of $U$ to be $u_1, \dots, u_n$ and to set $W = U^\top$.

### Proof — main ideas

1. $UW$ is of rank $n$, therefore its range is an $n$-dimensional subspace, denoted $S$.
2. The transformation $x \mapsto UWx$ moves $x$ into this subspace.
3. The point in $S$ closest to $x$ is $VV^\top x$, where the columns of $V$ form an orthonormal basis of $S$.
4. Therefore we can assume **w.l.o.g.** that $W = U^\top$ and that the columns of $U$ are orthonormal.

With orthonormal $U$ (so $U^\top U = I$), observe: $$ \begin{aligned} \lVert x - UU^\top x \rVert^2 &= \lVert x \rVert^2 - 2x^\top UU^\top x + x^\top UU^\top UU^\top x \\ &= \lVert x \rVert^2 - x^\top UU^\top x \\ &= \lVert x \rVert^2 - \operatorname{trace}!\left(U^\top x x^\top U\right) \end{aligned} $$
Therefore an **equivalent** PCA problem is: $$ \underset{U \in \mathbb{R}^{d,n}:, U^\top U = I}{\arg\max} \operatorname{trace}\left( U^\top \left( \sum_{i=1}^{m} x_i x_i^\top \right) U \right) $$
The solution is to set $U$ to be the leading eigenvectors of $A = \sum_{i=1}^{m} x_i x_i^\top$.
### Value of the objective
$$ \min_{W,,U} \sum_{i=1}^{m} \lVert x_i - UWx_i \rVert^2 = \sum_{i=n+1}^{d} \lambda_i(A) $$

i.e. the reconstruction error equals the sum of the $d - n$ **smallest** eigenvalues.

### Centering

It is common practice to **center** the examples before applying PCA:

1. Compute $\mu = \frac{1}{m}\sum_{i=1}^{m} x_i$.
2. Apply PCA to the vectors $(x_1 - \mu), \dots, (x_m - \mu)$.

> This is also related to the interpretation of PCA as **variance maximization**.

### Efficient implementation for $d \gg m$ (and Kernel PCA)

**Recall:** $A = \sum_{i=1}^{m} x_i x_i^\top = X^\top X$, where $X \in \mathbb{R}^{m,d}$ has the $i$-th row equal to $x_i^\top$.

Let $B = XX^\top$, i.e. $B_{i,j} = \langle x_i, x_j \rangle$ (the **Gram matrix**).

If $Bu = \lambda u$ then: $$ A(X^\top u) = X^\top X X^\top u = X^\top B u = \lambda (X^\top u) $$
So $\dfrac{X^\top u}{\lVert X^\top u \rVert}$ is an eigenvector of $A$ with eigenvalue $\lambda$.

- We can compute the PCA solution via the eigenvalues of $B$ instead of $A$.
- Complexity: $O(m^3 + m^2 d)$.
- It can be computed using a **kernel function** $\Rightarrow$ **Kernel PCA**.

### Pseudocode

```text
PCA
input:
    matrix of m examples  X ∈ ℝ^{m,d}
    number of components  n

if (m > d):
    A = Xᵀ X
    let u₁, …, uₙ be the eigenvectors of A with largest eigenvalues
else:
    B = X Xᵀ
    let v₁, …, vₙ be the eigenvectors of B with largest eigenvalues
    for i = 1, …, n:
        uᵢ = (1 / ‖Xᵀ vᵢ‖) · Xᵀ vᵢ

output: u₁, …, uₙ
```

### Demonstration
![[2 - Dimensionality Reduction-1780671395732.webp]]

---

## PCA — The Statistical / "ISLR" View

> [!summary]
> 
> - PCA produces a low-dimensional representation of a dataset. It finds a sequence of linear combinations of the variables that have **maximal variance** and are **mutually uncorrelated**.
> - Besides producing derived variables for supervised learning, PCA also serves as a tool for **data visualization**.

### First principal component
The first principal component of features $X_1, X_2, \dots, X_p$ is the **normalized** linear combination $$ Z_1 = \phi_{11}X_1 + \phi_{21}X_2 + \dots + \phi_{p1}X_p $$ that has the **largest variance**. _Normalized_ means $\sum_{j=1}^{p}\phi_{j1}^2 = 1$.

- The elements $\phi_{11}, \dots, \phi_{p1}$ are the **loadings** of the first principal component.
- Together they form the **loading vector** $\phi_1 = (\phi_{11};\phi_{21};\dots;\phi_{p1})^\top$.
- The sum-of-squares constraint ($\lVert \phi_1 \rVert = 1$) is needed — otherwise the loadings could be made arbitrarily large, giving arbitrarily large variance.

> [!example] PCA: example
> ![[2 - Dimensionality Reduction-1780671831854.webp|456x268]]
> The population size (**pop**) and ad spending (ad) **for** 100 different cities are shown as purple circles. The green solid line indicates the first principal component direction, and the blue dashed line indicates the second principal component direction.

### Computation of principal components
Suppose we have an $n \times p$ data set $\mathbf{X}$ ($n$ = $\#$ examples = $m$, $p$ = $\#$ features = $d$). Assume each variable is **centered to mean zero**.

We look for the linear combination of sample feature values $$ z_{i1} = \phi_{11}x_{i1} + \phi_{21}x_{i2} + \dots + \phi_{p1}x_{ip} \qquad (1) $$ for $i = 1, \dots, n$ that has the **largest sample variance**, subject to $\sum_{j=1}^{p}\phi_{j1}^2 = 1$.

Since each $x_{ij}$ has mean zero, so does $z_{i1}$, hence the sample variance is $\frac{1}{n}\sum_{i=1}^{n} z_{i1}^2$.

Plugging in $(1)$, the first loading vector solves: $$ \underset{\phi_{11}, \dots, \phi_{p1}}{\text{maximize}}; \frac{1}{n}\sum_{i=1}^{n}\left( \sum_{j=1}^{p}\phi_{j1}x_{ij}\right)^2 \quad \text{subject to}\quad \sum_{j=1}^{p}\phi_{j1}^2 = 1 $$

- Solved via a **singular-value decomposition (SVD)** of $\mathbf{X}$ — a standard linear-algebra technique.
- $Z_1$ is the **first principal component**, with realized values $z_{11}, \dots, z_{n1}$ (the **scores**).

> [!note] Eigendecomposition connection
>  $\mathbf{X}^\top \mathbf{X}$ is symmetric, real and **PSD**, so it diagonalizes as $\mathbf{X}^\top \mathbf{X} = U^\top \Lambda U$ with $\Lambda = \operatorname{diag}(\lambda_1, \dots, \lambda_p)$ and $\lambda_1 \ge \lambda_2 \ge \dots \ge \lambda_p \ge 0$. 
>  The principal directions are the eigenvectors; the variances relate to the eigenvalues.

### Geometry of PCA

- The loading vector $\phi_1$ defines a **direction** in feature space along which the data vary the most.
- Projecting the $n$ points $x_1, \dots, x_n$ onto this direction yields the **scores** $z_{11}, \dots, z_{n1}$ themselves.

### Further principal components

- The **second** principal component is the linear combination of $X_1, \dots, X_p$ with maximal variance **among all combinations uncorrelated with $Z_1$**.
- Its scores take the form $$z_{i2} = \phi_{12}x_{i1} + \phi_{22}x_{i2} + \dots + \phi_{p2}x_{ip},$$with loading vector $\phi_2$.
- Constraining $Z_2$ to be uncorrelated with $Z_1$ is **equivalent** to constraining $\phi_2 \perp \phi_1$ (orthogonal). And so on.
- The directions $\phi_1, \phi_2, \phi_3, \dots$ are the ordered **right singular vectors** of $\mathbf{X}$; the component variances are $\frac{1}{n}$ times the squares of the singular values.
- There are at most $\min(n-1,, p)$ principal components.

### Illustration — `USArrests` data

- For each of the 50 US states: arrests per 100,000 residents for **Assault**, **Murder**, **Rape**, plus **UrbanPop** (% urban population).
- Score vectors have length $n = 50$; loading vectors have length $p = 4$.
- PCA performed after **standardizing** each variable (mean 0, std 1).

> [!info] Biplot 
> A **biplot** displays both the scores (blue state names) and the loadings (orange arrows, with axes on the top and right) on one figure.
> ![[2 - Dimensionality Reduction-1780676051317.webp|406x406]]
> Example: the loading for **Rape** is $0.54$ on PC1 and $0.17$ on PC2, so the word "Rape" sits at $(0.54, 0.17)$.

**PCA loadings table:**

|Variable|PC1|PC2|
|---|:-:|:-:|
|Murder|0.5358995|−0.4181809|
|Assault|0.5831836|−0.1879856|
|UrbanPop|0.2781909|0.8728062|
|Rape|0.5434321|0.1673186|

> [!example] Reading the table 
> Each variable is reconstructed as a linear combination of the components, e.g. $$\text{Murder} = 0.536,\text{PC1} - 0.418,\text{PC2} + (\dots)\text{PC3} + (\dots)\text{PC4}$$ PC1 loads roughly equally on the three crime variables (an overall "crime rate" axis); PC2 is dominated by **UrbanPop** (an "urbanization" axis).

### PCA as the closest hyperplane

- The first loading vector defines the **line** in $p$-dimensional space **closest** to the $n$ observations (in average squared Euclidean distance).
- This extends beyond the first component: the first **two** principal components span the **plane** closest to the $n$ observations.
- General form: find a $k$-dimensional subspace $S$ **minimizing** $$ \sum_{i=1}^{m} \lVert x_i - \operatorname{proj}_S(x_i) \rVert_2^2 $$

### Scaling matters

- If variables are in **different units** → scale each to std 1 (**recommended**).
- If in the **same units** → you may or may not scale.

**Alternative: min–max scaler** $$ x_i \mapsto \frac{x_i - \min(x_i)}{\max(x_i) - \min(x_i)} $$
![[2 - Dimensionality Reduction-1780676253745.webp|697]]
Unscaled PCA can be dominated by whichever variable has the largest raw magnitude (e.g. Assault in `USArrests`), distorting the components.

### Proportion of Variance Explained (PVE)

> To understand the **strength of each component**, we are interested in knowing the proportion of variance explained (PVE) by each one.

Total variance (variables centered): $$ \sum_{j=1}^{p}\operatorname{Var}(X_j) = \sum_{j=1}^{p}\frac{1}{n}\sum_{i=1}^{n} x_{ij}^2 $$

Variance explained by the $m$-th component: $$ \operatorname{Var}(Z_m) = \frac{1}{n}\sum_{i=1}^{n} z_{im}^2 $$

It can be shown that $\sum_{j=1}^{p}\operatorname{Var}(X_j) = \sum_{m=1}^{M}\operatorname{Var}(Z_m)$, with $M = \min(n-1, p)$.

The **PVE** of the $m$-th component: $$ \text{PVE}_m = \frac{\sum_{i=1}^{n} z_{im}^2}{\sum_{j=1}^{p}\sum_{i=1}^{n} x_{ij}^2} $$

Eigenvalue form with covariance  $\Sigma = \frac{1}{n}\mathbf{X}^\top\mathbf{X}$ having eigenvalues $\lambda_i$, the total variance is $\operatorname{trace}(\Sigma) = \sum_i \lambda_i$ and $$\text{PVE}_m = \frac{\lambda_m}{\sum_i \lambda_i}, \qquad \text{CPVE}_k = \frac{\sum_{i=1}^{k}\lambda_i}{\sum_{j=1}^{p}\lambda_j}$$ The PVEs sum to one; we sometimes plot the **cumulative** PVE.

### How many components to use?

- **No simple answer** — cross-validation is not (directly) available, because PCA is unsupervised: there's no held-out "label" to predict and validate against.
- _When could CV work?_ When PCA is embedded in a downstream **supervised** task (e.g. principal-component regression), CV on the supervised error can choose the number of components.
- Practical heuristic: use the **scree plot** and look for an **"elbow"** where marginal PVE drops off.

### Demonstration — face images

- $50 \times 50$ images from the Yale dataset.
- Reconstruction to **10 dimensions** retains most recognizable facial structure 
(before vs. after look very similar).
![[2 - Dimensionality Reduction-1780676499521.webp|482]]

- Reducing to $\mathbb{R}^2$ separates different individuals into distinct clusters.
![[2 - Dimensionality Reduction-1780676519159.webp]]

---

## 2. Random Projections

### What counts as "successful" reduction here?

In PCA success = squared distance between $x$ and its reconstruction. But sometimes we **don't care about reconstruction** — we only want $y_1, \dots, y_m$ to **retain certain properties** of $x_1, \dots, x_m$.

**One option: do not distort distances.** We'd like, for all $i, j$: $$ \lVert x_i - x_j \rVert \approx \lVert y_i - y_j \rVert \quad\Longleftrightarrow\quad \frac{\lVert Wx_i - Wx_j \rVert}{\lVert x_i - x_j \rVert} \approx 1 $$

Equivalently, for all $x \in Q$ where $Q = {x_i - x_j : i, j \in [m]}$: $$ \frac{\lVert Wx \rVert}{\lVert x \rVert} \approx 1 $$

### Random projections preserve norms

**Random projection:** the transformation $x \mapsto Wx$, where $W$ is a **random matrix**.

Analyze with $W_{i,j} \sim \mathcal{N}(0, 1/n)$. Let $w_i$ be the $i$-th row of $W$: $$ \begin{aligned} \mathbb{E}\left[\lVert Wx \rVert^2\right] &= \sum_{i=1}^{n}\mathbb{E}\left[\langle w_i, x\rangle^2\right] = \sum_{i=1}^{n} x^\top \mathbb{E}\left[w_i w_i^\top\right] x \\ &= n, x^\top\left(\tfrac{1}{n}I\right)x = \lVert x \rVert^2 \end{aligned} $$
So the projection preserves the squared norm **in expectation**. In fact $\lVert Wx \rVert^2$ has a (scaled) $\chi^2_n$ distribution, and by a measure-concentration inequality: $$ \mathbb{P}!\left[\left| \frac{\lVert Wx \rVert^2}{\lVert x \rVert^2} - 1 \right| > \epsilon \right] \le 2, e^{-\epsilon^2 n / 6} $$

> [!theorem] Johnson–Lindenstrauss Lemma 
> Let $Q$ be a finite set of vectors in $\mathbb{R}^d$. Let $\delta \in (0,1)$ and let $n$ be an integer such that $$ \epsilon = \sqrt{\frac{6\log(2|Q|/\delta)}{n}} \le 3 . $$ Then with probability at least $1 - \delta$ over a random matrix $W \in \mathbb{R}^{n,d}$ with $W_{i,j}\sim\mathcal{N}(0,\frac{1}{n})$: $$ \max_{x \in Q}\left| \frac{\lVert Wx \rVert^2}{\lVert x \rVert^2} - 1 \right| < \epsilon . $$

---

## 3. Compressed Sensing

### Prior assumption: sparsity

$$ x \approx U\alpha$$
$$U \text{ orthonormal}$$
$$\lVert\alpha\rVert_0 \overset{\text{def}}{=} |\{i : \alpha_i \ne 0\}| \le s \text{ for some } s \ll d $$
- E.g. **natural images** are approximately sparse in a **wavelet basis**.

### How to "store" $x$?

- Compute $\alpha = U^\top x$ and save only the **non-zero** elements of $\alpha$.
- Requires order of $s \log(d)$ storage.

> [!question] The key idea 
> Why acquire all $d$ coordinates of $x$ when most will be thrown away? **Can't we directly measure only the part that survives?** — This is the premise of compressed sensing.

### The three "surprising" results

1. Any sparse signal can be **fully reconstructed** if compressed by $x \mapsto Wx$, where $W$ satisfies the **Restricted Isometry Property (RIP)**. A RIP matrix has low norm-distortion for any sparse-representable vector.
2. The reconstruction is computable in **polynomial time** by solving a **linear program**.
3. A **random** $n \times d$ matrix likely satisfies RIP provided $n$ exceeds order $s \log(d)$.

### Restricted Isometry Property (RIP)

A matrix $W \in \mathbb{R}^{n,d}$ is $(\epsilon, s)$-**RIP** if for all $x \ne 0$ with $\lVert x \rVert_0 \le s$: $$ \left| \frac{\lVert Wx \rVert_2^2}{\lVert x \rVert_2^2} - 1 \right| \le \epsilon $$

### Lossless compression of sparse vectors

> [!theorem] RIP ⇒ lossless recovery
>  Let:
>  -  $\epsilon < 1$ 
>  - $W$ be a $(\epsilon, 2s)$-RIP matrix.
>  - $x$ satisfy $\lVert x \rVert_0 \le s$,
>  - $y = Wx$
>  -  $\tilde{x} \in \arg\min_{v:, Wv = y} \lVert v \rVert_0$.
>  Then, $$\tilde{x} = x$$

**Proof (by contradiction):**
1. Assume $\tilde{x} \ne x$.
2. Since $x$ satisfies the constraints defining $\tilde{x}$, we have $\lVert \tilde{x} \rVert_0 \le \lVert x \rVert_0 \le s$.
3. Therefore $\lVert x - \tilde{x} \rVert_0 \le 2s$.
4. Applying RIP to $(x - \tilde{x})$: $\left| \dfrac{\lVert W(x-\tilde{x})\rVert^2}{\lVert x-\tilde{x}\rVert^2} - 1 \right| \le \epsilon$.
5. But $W(x-\tilde{x}) = Wx - W\tilde{x} = y - y = 0$, so $|0 - 1| \le \epsilon$, i.e. $1 \le \epsilon$. **Contradiction** (since $\epsilon < 1$). $\blacksquare$

### Efficient reconstruction

If we further assume $\epsilon < \dfrac{1}{1+\sqrt{2}}$, then: $$ x = \underset{v: Wv = y}{\arg\min} \lVert v \rVert_0 = \underset{v: Wv = y}{\arg\min} \lVert v \rVert_1 $$

- The right-hand side ($\ell_1$ minimization) is a **linear programming** problem — solvable efficiently.
- **Summary:** we can reconstruct all sparse vectors efficiently from $O(s\log d)$ measurements.

---

## PCA vs. Random Projections

|Aspect|PCA|Random Projections|
|---|---|---|
|Guarantee|Perfect recovery if all examples lie in an $n$-dim subspace|Perfect recovery for all $O(n/\log d)$-sparse vectors|
|Reconstruction|Linear|**Non**-linear (e.g. $\ell_1$ minimization)|
|Prior knowledge|Data lies near a low-dim subspace|Data is sparse in some basis|

**Contrasting cases:**
- If the data is $e_1, \dots, e_d$ (standard basis vectors): **random projections perfect, PCA fails**.
- If $d$ is very large and the data lies exactly on an $n$-dim subspace: **PCA perfect, random projections may fail**.

---

## Summary

> [!summary]
> 
> - **Linear dimensionality reduction:** $x \mapsto Wx$.
> - **PCA** is optimal when reconstruction is **linear** and error is **squared distance**; solution = leading eigenvectors of $\sum_i x_i x_i^\top$.
> - **Random projections** preserve **distances** (Johnson–Lindenstrauss).
> - **Random projections** give **exact reconstruction for sparse vectors** — but with a **non-linear** reconstruction (compressed sensing).
