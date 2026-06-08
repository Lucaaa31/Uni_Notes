## Motivation

Kernel methods are motivated by four key needs:

1. **Efficient computation** of inner products in high dimensions
2. **Non-linear decision boundaries** — linear separation fails in most real problems
3. **Non-vectorial inputs** — handling strings, graphs, trees, etc.
4. **Flexible feature selection** — implicitly work in complex feature spaces

> [!tip] Core Idea 
> ![[2 - Kernel Methods-1780849617850.webp]]
> Instead of explicitly mapping data to a high-dimensional feature space and computing dot products, define a **kernel function** $$K(x, y) = \Phi(x) \cdot \Phi(y)$$ that computes this inner product directly and efficiently.

---

## Kernels

### PDS Condition

> [!definition] Positive Definite Symmetric (PDS) Kernel 
> A kernel $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ is **positive definite symmetric (PDS)** if for any ${x_1, \ldots, x_m} \subseteq \mathcal{X}$, the matrix $$\mathbf{K} = [K(x_i, x_j)]_{ij} \in \mathbb{R}^{m \times m}$$ is **symmetric positive semi-definite (SPSD)**.

A matrix $\mathbf{K}$ is SPSD if it is symmetric and satisfies **one** of these equivalent conditions:

- All eigenvalues are **non-negative**
- For any $\mathbf{c} \in \mathbb{R}^{m+1}$:$\displaystyle\mathbf{c}^\top \mathbf{K} \mathbf{c} = \sum_{i,j=1}^{m} c_i c_j K(x_i, x_j) \geq 0$

---

### Polynomial Kernels

> [!definition] Polynomial Kernel 
> For $x, y \in \mathbb{R}^N$: $$K(x, y) = (x \cdot y + c)^d, \quad c > 0$$

**Example** — $N = 2$, $d = 2$:

$$K(x, y) = (x_1 y_1 + x_2 y_2 + c)^2 = \begin{bmatrix} x_1^2 \\ x_2^2 \\ \sqrt{2} x_1 x_2 \\ \sqrt{2c} x_1 \\ \sqrt{2c} x_2 \\ c \end{bmatrix} \cdot \begin{bmatrix} y_1^2 \\ y_2^2 \\ \sqrt{2} y_1 y_2 \\ \sqrt{2c} y_1 \\ \sqrt{2c} y_2 \\ c \end{bmatrix}$$

This shows the **implicit feature map** $\Phi$ — we never need to compute it explicitly!

#### XOR Problem

Using a degree-2 polynomial kernel with $c = 1$, the XOR problem (linearly non-separable in $\mathbb{R}^2$) becomes **linearly separable** in the lifted feature space. The decision boundary is $x_1 x_2 = 0$.

![[2 - Kernel Methods-1780849847815.webp]]

---

### Normalized Kernels

> [!definition] Normalized Kernel
> The **normalized kernel** associated to $K$ is: $$\tilde{K}(x, x') = \begin{cases} 0 & \text{if } (K(x,x) = 0) \vee (K(x',x') = 0) \\ \dfrac{K(x, x')}{\sqrt{K(x,x), K(x',x')}} & \text{otherwise} \end{cases}$$

**Key properties:**
- If $K$ is PDS, then $\tilde{K}$ is PDS
- By definition, $\tilde{K}(x, x) = 1$ for all $x$ with $K(x,x) \neq 0$

**Proof that $\tilde{K}$ is PDS:**

$$\sum_{i,j=1}^m c_i c_j \tilde{K}(x_i, x_j) = \sum_{i,j} c_i c_j \frac{\langle \Phi(x_i), \Phi(x_j) \rangle}{|\Phi(x_i)|_H |\Phi(x_j)|_H} = \left| \sum_{i=1}^m \frac{c_i \Phi(x_i)}{|\Phi(x_i)|_H} \right|^2_H \geq 0$$

---

### Other Standard PDS Kernels

#### Gaussian (RBF) Kernel

$$K(x, y) = \exp\left(-\frac{\|x - y\|^2}{2\sigma^2}\right), \quad \sigma \neq 0$$

> [!note] This is the **normalized kernel** of $(x, x') \mapsto \exp!\left(\frac{x \cdot x'}{\sigma^2}\right)$.

#### Sigmoid Kernel

$$K(x, y) = \tanh(a(x \cdot y) + b), \quad a, b \geq 0$$

> [!warning] The sigmoid kernel is **not** always PDS — it depends on the choice of $a$ and $b$.

---

### Reproducing Kernel Hilbert Space (RKHS)

> [!theorem] RKHS Theorem _(Aronszajn, 1950)_ 
> Let $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ be a PDS kernel. Then there exists a Hilbert space $H$ and a mapping $\Phi: \mathcal{X} \to H$ such that: $$\forall, x, y \in \mathcal{X},\quad K(x, y) = \Phi(x) \cdot \Phi(y)$$

**Proof:**

Define $\Phi(x): \mathcal{X} \to \mathbb{R}^\mathcal{X}$ by $\Phi(x)(y) = K(x, y)$ (the kernel centered at $x$).

Let $$H_0 = \left\{ \sum_{i \in I} a_i \Phi(x_i) : a_i \in \mathbb{R}, x_i \in \mathcal{X}, |I| < \infty \right\}$$

Define inner product for $f = \sum_i a_i \Phi(x_i)$ and $g = \sum_j b_j \Phi(y_j)$:

$$\langle f, g \rangle = \sum_{i,j} a_i b_j K(x_i, y_j)$$

**Reproducing Property:** $$\forall, f \in H_0, \forall x \in \mathcal{X}: \quad f(x) = \langle f, \Phi(x) \rangle$$

**Key steps to show $\langle \cdot, \cdot \rangle$ is an inner product:**

1. **Bilinear & symmetric** ✓ (by construction)
2. **Positive semi-definite** ✓ (since $K$ is PDS)
3. **Definite** — uses Cauchy-Schwarz for PDS kernels: $$K(x,x) K(y,y) - K(x,y)^2 \geq 0 \implies [f(x)]^2 \leq \langle f, f \rangle \cdot K(x,x)$$

$H_0$ is completed to form the full **Hilbert space $H$** (the RKHS).

> [!note] Feature Spaces 
> Feature spaces associated to $K$ are **not unique** in general. $\Phi$ is called a **feature mapping**.

---

## Kernel-Based Algorithms

### SVMs with PDS Kernels

**Dual optimization problem:**

$$\max_{\boldsymbol{\alpha}} \sum_{i=1}^m \alpha_i - \frac{1}{2} \sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j \underbrace{K(x_i, x_j)}_{\Phi(x_i) \cdot \Phi(x_j)}$$

$$\text{subject to: } 0 \leq \alpha_i \leq C \quad \wedge \quad \sum_{i=1}^m \alpha_i y_i = 0, \quad i \in [1, m]$$

**Solution (hypothesis function):**

$$h(x) = \text{sgn}\left(\sum_{i=1}^m \alpha_i y_i K(x_i, x) + b\right)$$

$$b = y_i - \sum_{j=1}^m \alpha_j y_j K(x_j, x_i) \quad \text{for any } x_i \text{ with } 0 < \alpha_i < C$$

> [!tip] Key Insight
> The kernel replaces every dot product $\Phi(x_i) \cdot \Phi(x_j)$ — we **never** need to compute $\Phi$ explicitly!

---

### Rademacher Complexity of Kernel-Based Hypotheses

> [!theorem] Theorem
>  Let $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ be a PDS kernel, $\Phi: \mathcal{X} \to \mathbb{H}$ a feature mapping, $S \subseteq {x : K(x,x) \leq R^2}$ a sample of size $m$, and $H = \{\mathbf{x} \mapsto \mathbf{w} \cdot \Phi(x) : |\mathbf{w}|_\mathbb{H} \leq \Lambda\}$. Then: $$\hat{\mathfrak{R}}_S(H) \leq \frac{\Lambda\sqrt{\text{Tr}[\mathbf{K}]}}{m} \leq \sqrt{\frac{R^2 \Lambda^2}{m}}$$

**Proof sketch:**

$$\hat{\mathfrak{R}}_S(H) = \frac{1}{m} \mathbb{E}_\sigma \left[\sup_{\|\mathbf{w}\| \leq \Lambda} \mathbf{w} \cdot \sum_{i=1}^m \sigma_i \Phi(x_i)\right] \leq \frac{\Lambda}{m} \mathbb{E}_\sigma\left[\left\|\sum_{i=1}^m \sigma_i \Phi(x_i)\right\|\right]$$

Applying Jensen's inequality and $\mathbb{E}[\sigma_i \sigma_j] = 0$ for $i \neq j$:

$$\leq \frac{\Lambda}{m} \left[\mathbb{E}_\sigma!\left[\sum_{i=1}^m |\Phi(x_i)|^2\right]\right]^{1/2} = \frac{\Lambda}{m}\left[\sum_{i=1}^m K(x_i, x_i)\right]^{1/2} = \frac{\Lambda\sqrt{\text{Tr}[\mathbf{K}]}}{m}$$

---

### Representer Theorem

> [!theorem] Theorem
>  Let $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ be a PDS kernel with RKHS $H$. For any non-decreasing $G: \mathbb{R} \to \mathbb{R}$ and any loss $L: \mathbb{R}^m \to \mathbb{R} \cup \{+\infty\}$, the problem $$\arg\min_{h \in H} G(\|h\|_H) + L(h(x_1), \ldots, h(x_m))$$ admits a solution of the form: $$h^* = \sum_{i=1}^m \alpha_i K(x_i, \cdot)$$

**Proof idea:**

Decompose $H = H_1 \oplus H_1^\perp$ where $H_1 = \text{span}(\{K(x_i, \cdot)\})$.

For any $h = h_1 + h^\perp$:
- **Loss term:** $h(x_i) = \langle h, K(x_i, \cdot) \rangle = h_1(x_i)$ — only $h_1$ contributes
- **Regularization:** $G(\|h_1\|_H) \leq G(\|h\|_H)$ since $G$ is non-decreasing and $|h_1| \leq |h|$

Therefore $F(h_1) \leq F(h)$, confirming the solution lies in $H_1$.

> [!note] This theorem justifies why SVM solutions are finite combinations of kernel evaluations at training points.

---

## Closure Properties of PDS Kernels

> [!theorem] Closure Theorem PDS kernels are closed under:
> 
> 1. **Sum**: $K_1 + K_2$
> 2. **Product**: $K_1 \cdot K_2$
> 3. **Tensor product**: $(K_1 \otimes K_2)(x_1, y_1, x_2, y_2) = K_1(x_1, x_2), K_2(y_1, y_2)$
> 4. **Pointwise limit**: $K = \lim_{n \to \infty} K_n$
> 5. **Composition with power series** with non-negative coefficients: $f(K)$ where $f(x) = \sum_{n=0}^\infty a_n x^n$, $a_n \geq 0$

**Proofs:**

**Sum:** $\mathbf{c}^\top \mathbf{K} \mathbf{c} \geq 0$ and $\mathbf{c}^\top \mathbf{K'} \mathbf{c} \geq 0 \implies \mathbf{c}^\top (\mathbf{K} + \mathbf{K'}) \mathbf{c} \geq 0$ ✓

**Product:** Write $\mathbf{K} = \mathbf{M}\mathbf{M}^\top$, then:

$$\sum_{i,j} c_i c_j (K_{ij} K'_{ij}) = \sum_k \begin{bmatrix} c_1 M_{1k} \\ \vdots \\ c_m M_{mk} \end{bmatrix}^\top \mathbf{K'} \begin{bmatrix} c_1 M_{1k} \\ \vdots \\ c_m M_{mk} \end{bmatrix} \geq 0$$

**Pointwise limit:** $(\forall n, \mathbf{c}^\top \mathbf{K}_n \mathbf{c} \geq 0) \implies \lim_{n \to \infty} \mathbf{c}^\top \mathbf{K}_n \mathbf{c} = \mathbf{c}^\top \mathbf{K} \mathbf{c} \geq 0$ ✓

**Power series:** $K^n$ is PDS (closure under product) $\implies \sum_{n=0}^N a_n K^n$ is PDS (closure under sum) $\implies$ limit is PDS ✓

> [!example] Important Consequence 
> For any PDS kernel $K$: $\exp(K)$ is PDS (since $e^x = \sum_{n=0}^\infty \frac{x^n}{n!}$, all coefficients positive).

---

## Sequence Kernels

> [!definition] **Sequence kernels** are kernels defined over pairs of strings. Used in:
> 
> - Computational biology (protein/DNA analysis)
> - Text and speech classification

**Core idea:** Two sequences are similar when they share common substrings or subsequences.

**Example — Bigram Kernel:** $$K(x, y) = \sum_{\text{bigram } u} \text{count}_x(u) \times \text{count}_y(u)$$

---

### Weighted Transducers
![[2 - Kernel Methods-1780850701479.webp]]
A **weighted transducer** $T$ maps pairs of strings $(x, y)$ to weights by summing the weights of all accepting paths with input $x$ and output $y$:

$$T(x, y) = \text{sum of weights of all accepting paths labeled } (x, y)$$

**Example:** $$T(\text{abb}, \text{baa}) = 0.1 \times 0.2 \times 0.3 \times 0.1 + 0.5 \times 0.3 \times 0.6 \times 0.1$$

---

### Rational Kernels

A kernel $K: \Sigma^* \times \Sigma^* \to \mathbb{R}$ is **rational** if $K = T$ for some weighted transducer $T$.

**Composition of transducers** $T_1$ and $T_2$: $$(T_1 \circ T_2)(x, y) = \sum_{z \in \Delta^*} T_1(x, z) T_2(z, y)$$

**Inverse of transducer** $T^{-1}$: obtained by swapping input and output labels.

---

### PDS Rational Kernels

For any weighted transducer $T$, the function $K = T \circ T^{-1}$ is a **PDS rational kernel**.

**Proof:**

$$K(x, y) = \sum_z T(x, z) T(y, z)$$

Define $K_n(x, y) = \sum_{|z| \leq n} T(x,z), T(y,z)$. Then $K_n$ is PDS since $\mathbf{K}_n = \mathbf{A}\mathbf{A}^\top$ where $A_{ij} = K_n(x_i, z_j)$. Taking $n \to \infty$ (pointwise limit) gives $K$ PDS. ✓

---

### Counting Transducers
![[2 - Kernel Methods-1780850961234.webp|697]]
A **counting transducer** $T_X$ counts occurrences of pattern $X$ in a string $Z$:

- $X$ can be a string or a regular expression (automaton)
- $Z \circ T_X$ gives the count of $X$ in $Z$

**Examples:**

- $T_{\text{bigram}}$ — counts bigrams
![[2 - Kernel Methods-1780851035729.webp]]
- $T_{\text{gappy bigram}}$ — counts bigrams with gaps, penalized by $\lambda^{\text{gap length}}$, $\lambda \in (0,1)$
![[2 - Kernel Methods-1780851059749.webp]]

All standard sequence kernels in computational biology / NLP are **special instances of PDS rational kernels**. There is one general algorithm (composition + shortest-distance) — no need for kernel-specific dynamic programming proofs.

---

### Composition Algorithm

The composition of two weighted transducers is also a weighted transducer.

**$\varepsilon$-free construction:**

- States are identified with **pairs** $(q_1, q_1')$
- Transitions defined by:

$$E = \bigcup_ {\substack{(q_1, a, b, w_1, q_2) \in E_1 \\ (q_1', b, c, w_2, q_2') \in E_2}} \{\big((q_1, q_1'),, a,, c,, w_1 \otimes w_2,, (q_2, q_2') \big)\}$$
![[2 - Kernel Methods-1780851224206.webp|697]]
**Complexity:** $O(|T_1| \cdot |T_2|)$ in general, linear in some cases.

**Handling $\varepsilon$-transitions:** Use an $\varepsilon$-filter $F$ to avoid redundant $\varepsilon$-paths: $$T = \tilde{T}_1 \circ F \circ \tilde{T}_2$$
![[2 - Kernel Methods-1780851253220.webp]]

---

### Kernels for Other Discrete Structures
The same rational kernel framework extends to:

- **Images**
- **Graphs**
- **Parse trees**
- **Automata**
- **Weighted automata**

---

## Negative Definite Kernels
> [!definition] NDS Kernel 
> A function $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ is **negative definite symmetric (NDS)** if it is symmetric and for all ${x_1, \ldots, x_m} \subseteq \mathcal{X}$ and $\mathbf{c} \in \mathbb{R}^m$ with $\mathbf{1}^\top \mathbf{c} = 0$: $$\mathbf{c}^\top \mathbf{K} \mathbf{c} \leq 0$$

If $K$ is PDS, then $-K$ is NDS — but the converse does **not** hold in general.

---

### Key Example

**Squared distance in Hilbert space** is NDS. For $\sum c_i = 0$:

$$\begin{aligned}
\sum_{i,j=1}^{m} c_i c_j \|\mathbf{x}_i - \mathbf{x}_j\|^2 
&= \sum_{i,j=1}^{m} c_i c_j (\mathbf{x}_i - \mathbf{x}_j) \cdot (\mathbf{x}_i - \mathbf{x}_j) \\
&= \sum_{i,j=1}^{m} c_i c_j (\|\mathbf{x}_i\|^2 + \|\mathbf{x}_j\|^2 - 2\mathbf{x}_i \cdot \mathbf{x}_j) \\
&= \sum_{i,j=1}^{m} c_i c_j (\|\mathbf{x}_i\|^2 + \|\mathbf{x}_j\|^2) - 2 \sum_{i=1}^{m} c_i \mathbf{x}_i \cdot \sum_{j=1}^{m} c_j \mathbf{x}_j \\
&\le \sum_{i,j=1}^{m} c_i c_j (\|\mathbf{x}_i\|^2 + \|\mathbf{x}_j\|^2) \\
&= \sum_{j=1}^{m} c_j \left( \sum_{i=1}^{m} c_i (\|\mathbf{x}_i\|^2) \right) + \sum_{i=1}^{m} c_i \left( \sum_{j=1}^{m} c_j (\|\mathbf{x}_j\|^2) \right) = 0.
\end{aligned}$$

---

### NDS Kernels — Key Properties

> [!theorem] Embedding Property
> Let $K$ be an NDS kernel with $K(x,y) = 0 \iff x = y$. Then there exists a Hilbert space $H$ and $\Phi: \mathcal{X} \to H$ such that: $$\forall x, y \in \mathcal{X}: \quad K(x, y) = \|\Phi(x) - \Phi(y)\|^2$$ Under this hypothesis, $\sqrt{K}$ defines a **metric** on $\mathcal{X}$.

> [!theorem] PDS–NDS Connection 
> Let $K$ be a symmetric kernel. Then:
> 
> 1. $K$ is NDS $\iff$ $\exp(-tK)$ is PDS for all $t > 0$
> 2. Define $$\tilde{K}(x,y) = K(x, x_0) + K(y, x_0) - K(x,y) - K(x_0, x_0)$$ for a fixed $x_0$. Then $K$ is NDS $\iff$ $\tilde{K}$ is PDS.

---

### Application Example

> [!example] **Gaussian kernel is PDS**
> Since $\|x - y\|^2$ is NDS, by Schoenberg's theorem: $$K(x, y) = \exp\left(-t|x - y|^2\right) \text{ is PDS for all } t > 0$$
>  **$\exp(-|x-y|^p)$ is NOT PDS for $p > 2$:** If it were PDS for some $t > 0$, then by substituting $x \to t^{1/p} x$: $$\sum_{i,j = 1} c_i c_j e^{-|t^{1/p}x_i - t^{1/p}x_j|^p} \geq 0$$ This would imply $|x-y|^p$ is NDS for $p > 2$ — contradiction (shown in homework).

---
## Appendix: Mercer's Condition

> [!theorem] Mercer's Condition 
> Let $\mathcal{X}$ be a compact subset of $\mathbb{R}^N$ and $K \in L^\infty(\mathcal{X} \times \mathcal{X})$ symmetric. Then $K$ admits a uniformly convergent expansion: $$K(x, y) = \sum_{n=0}^\infty a_n \psi_n(x) \psi_n(y), \quad a_n > 0$$ if and only if for any $c \in L^2(\mathcal{X})$: $$\iint_{\mathcal{X} \times \mathcal{X}} c(x) c(y) K(x, y) dx dy \geq 0$$

This is the integral operator characterization of PDS kernels (equivalent to the matrix definition for finite sets).
## Summary

|Property|Details|
|---|---|
|**PDS kernels**|Rich mathematical theory; extends linear algorithms to non-linear settings|
|**Flexibility**|Any PDS kernel can be plugged into SVM, regression, ranking, clustering, etc.|
|**Efficiency**|Compute $K(x,y)$ without explicitly computing $\Phi(x)$|
|**Generalization**|Rademacher complexity depends on $\text{Tr}[\mathbf{K}]$, not $\dim(\mathcal{F})$|
|**Construction**|Closed under sum, product, limit, power series; rational kernels for sequences|
|**NDS connection**|$K$ NDS $\iff$ $e^{-tK}$ PDS (Schoenberg); links metrics to kernels|

