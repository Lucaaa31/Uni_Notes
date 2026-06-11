### 1. The Kernel Trick & Structural Motivation

### Functional Lifting Mechanics

> **Context:** When a dataset exhibits non-linear decision boundaries or is non-vectorial, it can be mapped into a high-dimensional feature space where it becomes linearly separable. The kernel trick enables evaluating inner products in this transformed space without explicitly computing the coordinate mappings.

> [!definition] Dual Inner Product Representation
> 
> Let $\mathcal{X}$ denote the input domain. Instead of explicitly defining an evaluation mapping $\Phi: \mathcal{X} \to \mathbb{H}$ into a high-dimensional or infinite-dimensional Hilbert space $\mathbb{H}$ and evaluating the dot product $\langle \Phi(x), \Phi(y) \rangle_{\mathbb{H}}$, a kernel function $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ is introduced such that:
> 
> $$K(x, y) = \Phi(x) \cdot \Phi(y)$$
> ![[2 - Kernel Methods-1780849617850.webp|509x207]]
> 
> This implicit representation provides four functional capabilities:
> 
> - **Dimension-Independent Dot Products:** Efficient calculation of inner products in high dimensions.
>     
> - **Non-linear Decision Boundaries:** Linear separation can be achieved in the lifted space $\mathbb{H}$, corresponding to non-linear boundaries in the original space $\mathcal{X}$.
>     
> - **Non-vectorial Input Support:** Native handling of non-numeric structures (e.g., strings, graphs, parse trees) by defining similarity measures directly over those domains.
>     
> - **Implicit Feature Expansion:** Flexible modeling within complex feature spaces without the computational cost of explicit transformation.
>     

### 2. Positive Definite Symmetric (PDS) Foundations

### Matrix Characterization

> **Context:** For a kernel to represent a valid inner product in some Hilbert space, its empirical evaluation matrix over any finite sample must satisfy strict symmetry and non-negativity properties.

> [!definition] Positive Definite Symmetric Kernels
> 
> A kernel function $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ is **Positive Definite Symmetric (PDS)** if, for any finite sequence of samples $\{x_1, \dots, x_m\} \subseteq \mathcal{X}$, the corresponding Gram matrix $\mathbf{K} = [K(x_i, x_j)]_{ij} \in \mathbb{R}^{m \times m}$ is a Symmetric Positive Semi-Definite (SPSD) matrix.
> 
> A Gram matrix $\mathbf{K}$ is SPSD if it satisfies $\mathbf{K} = \mathbf{K}^\top$ alongside either of these equivalent algebraic conditions:
> 
> - **Eigenvalue Boundary:** Every eigenvalue $\lambda_i$ of the matrix satisfies $\lambda_i \ge 0$.
>     
> - **Quadratic Form Non-negativity:** For any arbitrary coefficient vector $\mathbf{c} \in \mathbb{R}^m$:
>     
>     $$\mathbf{c}^\top \mathbf{K} \mathbf{c} = \sum_{i=1}^{m} \sum_{j=1}^{m} c_i c_j K(x_i, x_j) \geq 0$$
>     

### Standard Operational Kernels

> **Context:** Different kernel functions correspond to different implicit feature spaces, providing a variety of non-linear shapes and behaviors.

> [!definition] Polynomial, Normalized, and Radial Basis Operators
> 
> - **Polynomial Kernel:** For $x, y \in \mathbb{R}^N$ with parameters $c > 0$ and $d \in \mathbb{N}^+$:
>     
>     $$K(x, y) = (x \cdot y + c)^d$$
>     
>     _Expanding this function using the binomial theorem reveals the underlying feature map $\Phi(x)$, demonstrating how it captures polynomial feature interactions up to degree $d$. This map makes non-linear problems like XOR linearly separable in the higher-dimensional space._
>     
> - **Normalized Kernel:** The normalized kernel $\tilde{K}$ scales the inner product by the norms of the points in the feature space:
>     
>     $$\tilde{K}(x, x') = \begin{cases} 0 & \text{if } (K(x,x) = 0) \vee (K(x',x') = 0) \\ \dfrac{K(x, x')}{\sqrt{K(x,x) K(x',x')}} & \text{otherwise} \end{cases}$$
>     
>     _If $K$ is PDS, the normalized version $\tilde{K}$ is also PDS and satisfies $\tilde{K}(x, x) = 1$ for all non-zero entries._
>     
> - **Gaussian Radial Basis Function (RBF) Kernel:** A stationary kernel mapping points based on their squared Euclidean distance:
>     
>     $$K(x, y) = \exp\left(-\frac{\|x - y\|^2}{2\sigma^2}\right), \quad \sigma \neq 0$$
>     
>     _The Gaussian kernel corresponds to an infinite-dimensional feature space, meaning it can approximate highly complex, smooth decision boundaries._
>     

### 3. Reproducing Kernel Hilbert Spaces (RKHS)

### Theoretical Construction

> **Context:** The connection between a PDS kernel and its corresponding feature space is formalized by the existence of a unique Hilbert space where the kernel acts as an evaluation operator.

> [!theorem] Aronszajn RKHS Existence Theorem
> 
> For every positive definite symmetric kernel $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$, there exists a unique Hilbert space $\mathbb{H}$ of real-valued functions on $\mathcal{X}$ where $K$ satisfies the reproducing property.
> 
>$$\forall, x, y \in \mathcal{X},\quad K(x, y) = \Phi(x) \cdot \Phi(y)$$
>**Key steps to show $\langle \cdot, \cdot \rangle$ is an inner product:**
>1. **Bilinear & symmetric** ✓ (by construction)
>2. **Positive semi-definite** ✓ (since $K$ is PDS)
>3. **Definite** — uses Cauchy-Schwarz for PDS kernels: $$K(x,x) K(y,y) - K(x,y)^2 \geq 0 \implies [f(x)]^2 \leq \langle f, f \rangle \cdot K(x,x)$$
>
>$H_0$ is completed to form the full **Hilbert space $H$** (the RKHS).

### 4. Generalization Bounds & Optimization Theory

### Kernelized Support Vector Machines

> **Context:** Incorporating PDS kernels into the soft-margin SVM optimization framework enables non-linear classification by replacing all explicit vector inner products with kernel evaluations.

> [!theorem] Kernelized Dual Form
> 
> Replacing the standard dot products $x_i \cdot x_j$ with a PDS kernel $K(x_i, x_j)$ converts the dual optimization problem into a non-linear classifier:
> 
> $$\max_{\boldsymbol{\alpha}} \sum_{i=1}^m \alpha_i - \frac{1}{2} \sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j \underbrace{K(x_i, x_j)}_{\Phi(x_i) \cdot \Phi(x_j)}$$
> 
> $$\text{subject to: } 0 \leq \alpha_i \leq C \quad \text{and} \quad \sum_{i=1}^m \alpha_i y_i = 0, \quad \forall i \in \{1, \dots, m\}$$
> 
> The final decision function is evaluated as a linear combination of kernel terms over the support vectors:
> 
> $$h(x) = \operatorname{sgn}\left(\sum_{i=1}^m \alpha_i y_i K(x_i, x) + b\right)$$
> 
> where the bias term $b$ is computed from any support vector satisfying $0 < \alpha_i < C$:
> 
> $$b = y_i - \sum_{j=1}^m \alpha_j y_j K(x_j, x_i)$$
> The kernel replaces every dot product $\Phi(x_i) \cdot \Phi(x_j)$ — we **never** need to compute $\Phi$ explicitly!

### Rademacher Complexity of Kernel Classes

> **Context:** The generalization capacity of a kernel-based hypothesis class can be bounded by the trace of its empirical Gram matrix, showing that performance guarantees depend on the kernel properties rather than the raw dimensionality of the feature space.

> [!theorem] Trace-Based Rademacher Bound
> 
> Let $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ be a PDS kernel with an associated feature mapping $\Phi: \mathcal{X} \to \mathbb{H}$. Let $S = \{x_1, \dots, x_m\}$ be a sample where the kernel is bounded by $K(x,x) \le R^2$, and let $\mathcal{H} = \{x \mapsto \mathbf{w} \cdot \Phi(x) : \|\mathbf{w}\|_{\mathbb{H}} \le \Lambda\}$ be the hypothesis class. The empirical Rademacher complexity satisfies:
> 
> $$\widehat{\mathfrak{R}}_S(\mathcal{H}) \le \frac{\Lambda \sqrt{\mathrm{Tr}[\mathbf{K}]}}{m} \le \sqrt{\frac{R^2 \Lambda^2}{m}}$$
> 

### The Representer Theorem

> **Context:** The Representer Theorem provides a theoretical foundation for empirical risk minimization in reproducing kernel Hilbert spaces. It guarantees that the optimal model for any arbitrary loss function can be expressed as a finite linear combination of kernel evaluations at the training points.

> [!theorem] Generalized Representer Theorem
> 
> Let $K$ be a PDS kernel with an associated RKHS $\mathbb{H}$. Let $G: \mathbb{R} \to \mathbb{R}$ be a strictly non-decreasing regularization function, and let $L: \mathbb{R}^m \to \mathbb{R} \cup \{+\infty\}$ be an arbitrary loss function. Any optimizer $h^*$ of the regularized empirical risk objective:
> 
> $$\arg\min_{h \in \mathbb{H}} \left\{ G(\|h\|_{\mathbb{H}}) + L\big(h(x_1), \dots, h(x_m)\big) \right\}$$
> 
> can be expressed in the form:
> 
> $$h^* = \sum_{i=1}^m \alpha_i K(x_i, \cdot)$$
> 

### Algebric Closure Properties

> **Context:** The space of PDS kernels is closed under several standard algebraic operations, allowing the construction of specialized kernels by combining simpler baseline functions.

> [!theorem] PDS Closure Identities
> 
> Let $K_1$ and $K_2$ be valid PDS kernels over domain $\mathcal{X}$. The following compositions are also guaranteed to be PDS kernels:
> 
> - **Summation:** $K(x,y) = K_1(x,y) + K_2(x,y)$
>     
> - **Product:** $K(x,y) = K_1(x,y) \cdot K_2(x,y)$
>     
> - **Tensor Product:** $(K_1 \otimes K_2)(x_1, y_1, x_2, y_2) = K_1(x_1, x_2)K_2(y_1, y_2)$
>     
> - **Pointwise Limit:** $K(x,y) = \lim_{n \to \infty} K_n(x,y)$ if each $K_n$ is PDS.
>     
> - **Power Series Composition:** $K'(x,y) = \sum_{n=0}^\infty a_n \left(K_1(x,y)\right)^n$ for any sequence of non-negative coefficients $a_n \ge 0$.
>     
> 
> As a consequence of the power series expansion $e^x = \sum_{n=0}^\infty \frac{x^n}{n!}$, if $K(x,y)$ is a valid PDS kernel, then its exponential composition $\exp(K(x,y))$ is also a valid PDS kernel.

### 5. Sequence Kernels & Rational Transducers

### Formal String Automata

> **Context:** To apply kernel methods to variable-length non-vectorial discrete structures, such as text sequences or biological data, similarity can be measured by counting shared structural patterns using weighted finite-state transducers.

> [!definition] Rational and Sequence Kernels
> 
> Let $\Sigma^*$ represent the set of all finite-length strings over a discrete alphabet $\Sigma$. A sequence similarity metric can be defined using structural matching functions:
> 
> - **Bigram Count Kernel:** A kernel that counts the occurrences of shared adjacent character pairs (bigrams) $u$ within strings $x$ and $y$:
>     
>     $$K(x, y) = \sum_{u \in \Sigma^2} \mathrm{count}_x(u) \times \mathrm{count}_y(u)$$
>     
> - **Weighted Transducer Mapping:** A weighted finite-state transducer $T$ maps an input string $x$ to an output string $y$. The total transition weight $T(x,y)$ is computed by summing the combined path weights across all accepting paths that read $x$ and output $y$:
>     
>     $$T(x, y) = \sum_{\pi \in \mathrm{Paths}(x,y)} \prod_{e \in \pi} w(e)$$
>     
> - **Rational Kernel Form:** A sequence kernel $K: \Sigma^* \times \Sigma^* \to \mathbb{R}$ is defined as **rational** if its evaluation corresponds to the transition weight computed by a valid weighted transducer $T$, such that $K = T$.
>     

### Rational PDS Validations

> **Context:** To guarantee that a transducer-based sequence kernel satisfies the PDS condition, we can define the kernel using an composition operator that pairs a transducer with its inverse.

> [!theorem] Inverse Composition PDS Property
> 
> Let $T$ be a weighted finite-state transducer, and let $T^{-1}$ denote its inverse operator, obtained by swapping the input and output labels on every transition. The composition kernel defined by $K = T \circ T^{-1}$ is guaranteed to be a valid PDS rational kernel:
> 
> $$K(x, y) = (T \circ T^{-1})(x,y) = \sum_{z \in \Sigma^*} T(x, z) T(y, z)$$


### Computational String Transducers

> **Context:** Many standard sequence comparison methods can be formalized as rational kernels by designing specific counting configurations within the transducer topology.

![[2 - Kernel Methods-1780851035729.webp]]
![[2 - Kernel Methods-1780851059749.webp]]

> [!definition] Transducer Composition Mechanics
> 
> The composition of two weighted transducers $T_1$ and $T_2$ creates a new transducer ($T_1 \circ T_2$) that models intermediate paths. For $\epsilon$-free transducers, the composition is structured as follows:
> 
> - **State Space:** States in the composed machine are defined as pairs of states from the component machines: $(q_1, q_1') \in Q_1 \times Q_2$.
>     
> - **Transition Mapping Rule:** A transition exists if the output label of the first machine matches the input label of the second machine:
>     
>     $$E = \bigcup_{ \substack{(q_1, a, b, w_1, q_2) \in E_1 \\ (q_1', b, c, w_2, q_2') \in E_2} } \left\{ \big( (q_1, q_1'), a, c, w_1 \otimes w_2, (q_2, q_2') \big) \right\}$$
>     
>     The overall computational complexity of this composition is bounded by $\mathcal{O}(|T_1| \cdot |T_2|)$. When handling machines with $\epsilon$-transitions, an intermediate $\epsilon$-filter automaton $F$ is introduced to prevent redundant path generation: $T = T_1 \circ F \circ T_2$.
>     

### 6. Negative Definite Symmetric (NDS) Kernels

### Alternative Matrix Topology

> **Context:** Some similarity measures and distance metrics do not satisfy the PDS condition directly but can instead be characterized as negative definite symmetric functions. This family of functions retains a close connection to PDS kernels through exponential transformations.

> [!definition] Negative Definite Symmetric Kernels
> 
> A function $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ is **Negative Definite Symmetric (NDS)** if it is symmetric ($K(x,y) = K(y,x)$) and its Gram matrix satisfies a conditional non-positivity constraint for all finite sequences $\{x_1, \dots, x_m\} \subseteq \mathcal{X}$ and coefficient vectors $\mathbf{c} \in \mathbb{R}^m$ whose elements sum to zero:
> 
> $$\mathbf{c}^\top \mathbf{K} \mathbf{c} = \sum_{i=1}^{m} \sum_{j=1}^{m} c_i c_j K(x_i, x_j) \le 0, \quad \text{for all } \mathbf{c} \text{ such that } \sum_{i=1}^m c_i = 0$$
> 
> _While every valid PDS kernel multiplied by $-1$ is inherently NDS, the converse is not true in general; the NDS definition applies to a broader class of conditional distance metrics._

### Distance Metric Embedding Identity

> **Context:** NDS kernels are closely linked to geometric distance metrics, meaning any valid NDS kernel can be mapped to a squared distance in an underlying Hilbert space.

> [!theorem] Hilbert Distance Embedding
> 
> Let $K$ be an NDS kernel that satisfies $K(x,y) = 0 \iff x = y$. There exists an associated Hilbert space $\mathbb{H}$ and a corresponding coordinate feature mapping $\Phi: \mathcal{X} \to \mathbb{H}$ such that the kernel evaluates to the squared distance between the mapped points:
> 
> $$K(x, y) = \|\Phi(x) - \Phi(y)\|^2_{\mathbb{H}}$$
> 
> _Under these conditions, the square root function $\sqrt{K(x,y)}$ satisfies the triangle inequality and defines a valid metric over the input domain $\mathcal{X}$._

### The Schoenberg Connection

> **Context:** Schoenberg’s theorem establishes a bridge between NDS and PDS kernels, showing how geometric distances can be transformed into valid PDS inner products using exponential mappings.

> [!theorem] Schoenberg's Core Equivalence Theorem
> 
> Let $K: \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ be a symmetric kernel function. The following statements are equivalent:
> 
> 1. The kernel $K$ satisfies the Negative Definite Symmetric (NDS) condition.
>     
> 2. For all positive scaling factors $t > 0$, the exponentially transformed kernel is PDS:
>     
>     $$K_{\mathrm{PDS}}(x,y) = \exp(-t K(x,y)) \in \mathrm{PDS}$$


### Appendix: Mercer's Integral Condition

### Continuous Operators

> **Context:** While the SPSD matrix definition applies to finite sample sets, Mercer's condition provides an equivalent formulation for continuous input domains by analyzing the properties of continuous integral operators.

> [!theorem] Mercer's Theorem
> 
> Let $\mathcal{X} \subseteq \mathbb{R}^N$ be a compact set, and let $K \in L^\infty(\mathcal{X} \times \mathcal{X})$ be a continuous symmetric function. The kernel admits a uniformly and absolutely convergent eigenfunction expansion:
> 
> $$K(x, y) = \sum_{n=0}^\infty a_n \psi_n(x) \psi_n(y), \quad \text{with coefficients } a_n > 0$$
> 
> if and only if the associated linear integral operator is positive semi-definite for all continuous functions $c \in L^2(\mathcal{X})$:
> 
> $$\iint_{\mathcal{X} \times \mathcal{X}} c(x) c(y) K(x, y) \, \mathrm{d}x \, \mathrm{d}y \geq 0$$

