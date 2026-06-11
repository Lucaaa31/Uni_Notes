### Core Objective & Linear Maps

> **Context:** High-dimensional data frequently challenges learning algorithms due to increased computational demand and estimation errors. Linear dimensionality reduction projects sample vectors into a lower-dimensional subspace using a continuous linear operator matrix.

> [!definition] Definition
> **Dimensionality reduction** = taking data in a **high-dimensional** space and mapping it into a **low-dimensional** space.
> 
> **Why do it?**
> 
> - Reduces training (and testing) time
> - Reduces estimation error
> - Interpretability: finding meaningful structure in data, illustration / visualization
> 
> **Linear dimensionality reduction:** $x \mapsto Wx$ where $W \in \mathbb{R}^{n,d}$ and $n < d$.

### 1. Principal Component Analysis (PCA)

### Mathematical Formulations

> **Context:** To construct an optimal subspace projection, we enforce a linear reconstruction criterion that minimizes the total squared Euclidean distance between the original high-dimensional vectors and their low-dimensional reconstructions.

> [!definition] The PCA Optimization Problem
> 
> Let $X = \{x_1, \ldots, x_m\}$ be a set of vectors in $\mathbb{R}^d$. Linear dimensionality reduction maps $x \mapsto Wx$, where $W \in \mathbb{R}^{n,d}$ and $n < d$. Using a linear reconstruction matrix $U \in \mathbb{R}^{d,n}$, the approximated vector is $\tilde{x} = UWx$. The objective minimizes the sum of squared reconstruction errors:
> 
> $$\min_{W \in \mathbb{R}^{n,d}, \; U \in \mathbb{R}^{d,n}} \sum_{i=1}^{m} \| x_i - UWx_i \|^2$$

### Eigendecomposition and Equivalence Proof

> **Context:** Because the combined matrix product $UW$ forms an operator of rank $n$, its range defines an $n$-dimensional subspace. The closest point within any subspace to a target vector is its orthogonal projection, allowing the minimization task to be converted into an equivalent variance maximization problem.

> [!theorem] PCA Subspace Solution
> 
> Let $A = \sum_{i=1}^{m} x_i x_i^\top$ be the inner product matrix of the data. The optimal matrix $U$ has columns equal to the $n$ leading eigenvectors $u_1, \ldots, u_n$ of $A$, and the optimal encoding matrix is $W = U^\top$.
> 
> **Objective Boundary Value:**
> 
> At the optimal solution, the residual reconstruction error matches the sum of the remaining omitted eigenvalues:
> 
> $$\min_{W, U} \sum_{i=1}^{m} \| x_i - UWx_i \|^2 = \sum_{i=n+1}^{d} \lambda_i(A)$$

### Mean Centering Preprocessing

> **Context:** To ensure that the principal directions capture the maximum statistical variance of the dataset rather than shifting toward a displaced origin, the data coordinates must be translated to the coordinate center.

> [!definition] Centered Scatter Matrix
> 
> Let $\mu = \frac{1}{m}\sum_{i=1}^{m} x_i$ be the empirical sample mean. 
> The centering transformation translates the sample coordinates:
> 
> $$x_i \mapsto (x_i - \mu)$$
> 
> Applying PCA to centered data updates the target matrix $A$ to represent the sample covariance matrix up to a scaling constant:
> 
> $$A = \sum_{i=1}^{m} (x_i - \mu)(x_i - \mu)^\top$$

### High-Dimensional Sample Scale Inversion

> **Context:** When the feature space dimensionality vastly exceeds the sample size ($d \gg m$), computing the eigenvectors of the $d \times d$ matrix $A$ becomes computationally prohibitive. We can invert the problem by evaluating the smaller $m \times m$ Gram matrix instead.

> [!theorem] Dual Gram Matrix Eigendecomposition
> 
> Let $X \in \mathbb{R}^{m,d}$ be the data matrix where row $i$ equals $x_i^\top$, such that $A = X^\top X$. 
> Let $B = XX^\top \in \mathbb{R}^{m,m}$ define the Gram matrix where $B_{i,j} = \langle x_i, x_j \rangle$. 
> If $u$ is an eigenvector of $B$ with eigenvalue $\lambda$, then $X^\top u$ is an eigenvector of $A$ with the same eigenvalue $\lambda$.
> 
> **Proof:**
> 
> Premultiplying the eigenvector equation $Bu = \lambda u$ by $X^\top$:
> 
> $$A(X^\top u) = X^\top X X^\top u = X^\top B u = \lambda (X^\top u)$$
> 
> Normalizing the vector yields the normalized eigenvector of $A$:
> 
> $$\boxed{u_i = \frac{X^\top v_i}{\|X^\top v_i\|}}$$
> 
> _Note: This formulation lowers the computational complexity to $\mathcal{O}(m^3 + m^2 d)$ and enables Kernel PCA via inner product substitution._

### Dual Computational Logic

> **Context:** Practical PCA packages implement an algorithmic switch: they evaluate the cross-product matrix directly if features are few, or exploit the Gram identity if samples are few.
### Subspace Algorithmic Flow

```
input: Matrix X ∈ ℝ^{m,d}, Target Components n

if (m > d):
    Compute A = Xᵀ X
    Extract u₁, …, uₙ as leading eigenvectors of A
else:
    Compute B = X Xᵀ
    Extract v₁, …, vₙ as leading eigenvectors of B
    for i = 1 to n:
        u_i = (1 / ‖Xᵀ v_i‖) · Xᵀ v_i

output: Projection matrix columns u₁, …, uₙ
```

### Variance Maximization Metric

> **Context:** To evaluate how much structural information is retained by an $n$-dimensional projection, we compute the ratio of the variance captured by the selected components relative to the total variance across all dimensions.

> [!definition] Proportion of Variance Explained (PVE)
> 
> Assuming mean-centered data columns, the total sample variance equals the trace of the covariance matrix. For the $m$-th principal component score $z_{im} = \phi_{m}^\top x_i$, the Proportion of Variance Explained is:
> 
> $$\mathrm{PVE}_m = \frac{\sum_{i=1}^{n} z_{im}^2}{\sum_{j=1}^{p}\sum_{i=1}^{n} x_{ij}^2} = \frac{\lambda_m}{\sum_{i=1}^{d} \lambda_i}$$
> 
> The Cumulative Proportion of Variance Explained ($\mathrm{CPVE}_k$) across $k$ components is:
> 
> $$\boxed{\mathrm{CPVE}_k = \frac{\sum_{i=1}^{k} \lambda_i}{\sum_{j=1}^{d} \lambda_j}}$$

### 2. Random Projections

### Distance Preservation Guarantees

> **Context:** If the objective of dimensionality reduction is preserving pairwise Euclidean distances rather than reconstructing individual data vectors, the target subspace does not need to be calculated from data variances; it can be constructed using random mapping matrices instead.

> [!theorem] Johnson-Lindenstrauss Lemma
> 
> Let $Q$ be a finite set of vectors in $\mathbb{R}^d$. Let $\delta \in (0,1)$ and let $n$ be an integer such that:
> 
> $$\epsilon = \sqrt{\frac{6\log(2|Q|/\delta)}{n}} \le 3$$
> 
> Then, with a probability of at least $1 - \delta$, a random matrix $W \in \mathbb{R}^{n,d}$ with entries drawn independently from $W_{i,j} \sim \mathcal{N}\left(0, \frac{1}{n}\right)$ satisfies:
> 
> $$\max_{x \in Q} \left| \frac{\| Wx \|^2}{\| x \|^2} - 1 \right| < \epsilon$$


### 3. Compressed Sensing

### Sparse Signal Recovery

> **Context:** If a high-dimensional vector is sparse—meaning it contains only a small number of non-zero coefficients in a given basis—it can be compressed and perfectly reconstructed from a small set of random linear measurements.

> [!definition] Restricted Isometry Property (RIP)
> 
> A measurement sampling matrix $W \in \mathbb{R}^{n,d}$ satisfies the $(\epsilon, s)$-RIP condition if, for all vectors $x \neq 0$ whose zero-norm complies with the sparsity limit $\| x \|_0 \le s$, the structural norm distortion satisfies:
> 
> $$\left| \frac{\| Wx \|_2^2}{\| x \|_2^2} - 1 \right| \le \epsilon$$

### Exact Reconstruction Boundaries

> **Context:** When a sampling matrix satisfies the Restricted Isometry Property for a given sparsity threshold, compression preserves enough information to make lossless recovery mathematically certain.

> [!theorem] Lossless Sparsity Recovery
> 
> Let $W$ be an $(\epsilon, 2s)$-RIP matrix with $\epsilon < 1$. If a vector $x$ satisfies $\| x \|_0 \le s$ and we observe the compressed vector $y = Wx$, then solving the zero-norm optimization problem uniquely recovers the original vector.

### Framework Comparatives

> **Context:** PCA and Random Projections represent fundamentally different approaches to dimensionality reduction, relying on distinct prior assumptions and reconstruction techniques.

### Architectural Comparison

| **Structural Aspect**          | **Principal Component Analysis (PCA)**                             | **Random Projections & Compressed Sensing**                                        |
| ------------------------------ | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| **Recovery Guarantee**         | Exact linear recovery if data lies in an $n$-dimensional subspace. | Exact recovery for sparse vectors via non-linear optimization.                     |
| **Reconstruction Model**       | Linear transformation mapping: $\tilde{x} = UU^\top x$.            | Non-linear optimization (e.g., $\ell_1$ minimization linear programming).          |
| **Prior Structural Knowledge** | Assumes data points cluster near a low-dimensional subspace.       | Assumes data vectors are sparse ($\\                                               |
| **Limiting Failure Cases**     | Fails if data vectors are orthogonal standard basis elements.      | Fails if high-dimensional data lies exactly on a low-dimensional coordinate plane. |
