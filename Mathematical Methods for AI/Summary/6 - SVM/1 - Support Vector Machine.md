### 1. The Hard-Margin Linear Classifier

### Hyperplane Optimization

> **Context:** For binary classification tasks where the data distribution is linearly separable, we seek a decision boundary that maximizes the geometric distance to the closest training samples. This approach shifts the focus from merely reducing training error to maximizing structural generalization.

> [!definition] Geometric Margin and Linear Hypotheses
> 
> Let $S = ((x_1, y_1), \dots, (x_m, y_m)) \in \mathcal{X} \times \{-1, +1\}$ be an independent and identically distributed ($i.i.d.$) training sample drawn from a distribution $\mathcal{D}$, where $\mathcal{X} \subseteq \mathbb{R}^N$. We define the hypothesis class of linear classifiers $\mathcal{H}$ as:
> 
> $$\mathcal{H} = \{x \mapsto \operatorname{sgn}(w \cdot x + b) : w \in \mathbb{R}^N, b \in \mathbb{R}\}$$
>
>![[1 - Support Vector Machines-1780691167374.webp|382x217]]
> 
> The geometric margin $\rho$ of a separating hyperplane $(w, b)$ relative to the sample $S$ is the minimum scaled distance from any sample point to the decision boundary:
> 
> $$\rho = \min_{i \in [1,m]} \frac{|w \cdot x_i + b|}{\|w\|}$$

### Primal Optimization Form

> **Context:** To eliminate scale invariance, where scaling $w$ and $b$ leaves the hyperplane unchanged, we normalize the parameters so that the absolute functional distance to the nearest point is exactly $1$.

> [!theorem] Hard-Margin Primal Formulation
> 
> By applying the normalization constraint $\min_{i} |w \cdot x_i + b| = 1$, the geometric margin simplifies to $\rho = \frac{1}{\|w\|}$. 
> Maximizing this margin is equivalent to minimizing the squared norm of the weight vector. This formulation defines the convex primal optimization problem:
> 
> $$\min_{w,b} \frac{1}{2}\|w\|^2 \quad \text{subject to } y_i(w \cdot x_i + b) \ge 1, \quad \forall i \in \{1, \dots, m\}$$
> 
> **Properties:**
>- Convex optimization.
>- **Unique solution** for a linearly separable sample.

### 2. Lagrangian Duality and KKT Conditions

### Algebraic Derivation

> **Context:** Transforming the constrained primal problem into its dual form allows us to express the optimization purely in terms of inner products between training points. This transformation is a key step for scaling the method to high-dimensional spaces.
> $L(w, b, \alpha)$:
>- **$\alpha$:** Lagrange multiplier. It determines the weight of each data point.
>- **$w$:** Vector of weights. It determines the **orientation** of the hyperplane.
>- **$b$:** Bias. It determines the **offset** of the hyperplane w.r.t. the origin.


> [!theorem] Karush-Kuhn-Tucker (KKT) Optimality Conditions
> 
> To find the optimal hyperplane parameters, we introduce non-negative Lagrange multipliers $\alpha_i \ge 0$ to construct the primal Lagrangian function $L(w, b, \alpha)$:
> 
> $$L(w,b,\alpha) = \frac{1}{2}\|w\|^2 - \sum_{i=1}^m \alpha_i \left[ y_i(w \cdot x_i + b) - 1 \right]$$
> 
> Applying the KKT conditions requires setting the gradients of $L$ with respect to the primal variables $w$ and $b$ to zero:
> 
> - **Stationarity of $w$:**
>     
>     $$\nabla_w L = w - \sum_{i=1}^m \alpha_i y_i x_i = 0 \iff \boxed{w = \sum_{i=1}^m \alpha_i y_i x_i}$$
>     
> - **Stationarity of $b$:**
>     
>     $$\nabla_b L = -\sum_{i=1}^m \alpha_i y_i = 0 \iff \boxed{\sum_{i=1}^m \alpha_i y_i = 0}$$
>     
> - **Complementary Slackness Constraint:**
>     
>     $$\forall i \in \{1, \dots, m\}, \quad \boxed{\alpha_i \left[ y_i(w \cdot x_i + b) - 1 \right] = 0}$$
>     

### Support Vector Identification

> **Context:** The complementary slackness condition reveals a key property of SVMs: the final solution is sparse, meaning it depends only on a subset of the training data.

> [!definition] Support Vectors
> 
> The condition $\alpha_i [y_i(w \cdot x_i + b) - 1] = 0$ implies that either $\alpha_i = 0$ or $y_i(w \cdot x_i + b) = 1$.
> 
> - **Sparsity Property:** Points where $\alpha_i = 0$ do not influence the definition of the weight vector $w$.
>     
> - **Active Support Vectors:** Points $x_i$ where $\alpha_i > 0$ must satisfy $y_i(w \cdot x_i + b) = 1$. These points lie exactly on the margin boundaries, and they are the only samples that determine the optimal decision boundary.
>     

### Dual Optimization Problem

> **Context:** Substituting the optimality conditions for $w$ and $b$ back into the primal Lagrangian yields an alternative optimization problem focused entirely on the Lagrange multipliers.

> [!theorem] Quadratic Dual Formulation
> 
> Substituting $w = \sum_{i} \alpha_i y_i x_i$ and the constraint $\sum_{i} \alpha_i y_i = 0$ into the primal Lagrangian removes the dependency on the primal parameters:
> 
> $$L = \frac{1}{2}\left\| \sum_{i=1}^m \alpha_i y_i x_i \right\|^2 - \sum_{i=1}^m \sum_{j=1}^m \alpha_i \alpha_j y_i y_j (x_i \cdot x_j) + \sum_{i=1}^m \alpha_i$$
> 
> This yields the equivalent quadratic dual optimization problem:
> 
> $$\boxed{\max_{\alpha} \sum_{i=1}^m \alpha_i - \frac{1}{2}\sum_{i=1}^m \sum_{j=1}^m \alpha_i \alpha_j y_i y_j (x_i \cdot x_j)}$$
> 
> $$\text{subject to: } \alpha_i \ge 0 \quad \text{and} \quad \sum_{i=1}^m \alpha_i y_i = 0, \quad \forall i \in \{1, \dots, m\}$$
> 
> The resulting classifier uses the sign of a weighted sum of inner products:
> 
> $$h(x) = \operatorname{sgn}\left( \sum_{i=1}^m \alpha_i y_i (x_i \cdot x) + b \right)$$
> 
> where the bias term $b$ can be computed using any support vector $x_i$ ($y_i(w \cdot x_i + b) = 1$):
> 
> $$b = y_i - \sum_{j=1}^m \alpha_j y_j (x_j \cdot x_i)$$

### 3. Leave-One-Out Error Properties

### Sparsity Bounds

> **Context:** The leave-one-out (LOO) error provides an unbiased estimate of a model's generalization error. For support vector machines, this error is bounded by the proportion of support vectors, highlighting the link between solution sparsity and performance.

> [!theorem] Leave-One-Out Generalization Boundary
> 
> Let $h_S$ be the optimal maximum-margin hyperplane trained on a sample $S$ of size $m$, and let $N_{\text{SV}}(S)$ represent the count of active support vectors. The expected generalization error $R(h)$ satisfies the following upper bound:
> 
> $$\mathbb{E}_{S \sim \mathcal{D}^m}\left[ R(h_S) \right] \le \mathbb{E}_{S \sim \mathcal{D}^{m+1}}\left[ \frac{N_{\text{SV}}(S)}{m+1} \right]$$
> 

### 4. The Non-Separable Case & Soft-Margin SVMs

### Slack Variable Relaxation

> **Context:** When the data distribution cannot be perfectly separated by a linear boundary, the strict optimization constraints are relaxed using slack variables. This allows the model to tolerate bounded training errors in order to maintain a stable margin.

> [!definition] Soft-Margin Formulations
> 
> To allow violations of the margin, we introduce non-negative slack variables $\xi_i \ge 0$. The original classification constraints are modified as follows:
> 
> $$y_i(w \cdot x_i + b) \ge 1 - \xi_i, \quad \forall i \in \{1, \dots, m\}$$
>
>![[1 - Support Vector Machines-1780692633244.webp|382x228]]
> 
> Under this formulation, the geometric margin remains $\rho = \frac{1}{\|w\|}$, but the data points can now fall within the margin or on the incorrect side of the decision boundary.

### Primal Objective Functionals

> **Context:** The soft-margin optimization objective balances maximizing the margin width against minimizing total penalty violations, controlled by a regularization parameter $C$.

> [!theorem] Soft-Margin Primal Optimization
> 
> The soft-margin optimization problem introduces a regularization parameter $C \ge 0$ to control the trade-off between margin size and training error:
> 
> $$\min_{w,b,\xi} \frac{1}{2}\|w\|^2 + C\sum_{i=1}^m \xi_i \quad \text{subject to } y_i(w \cdot x_i + b) \ge 1 - \xi_i \;\; \wedge \;\; \xi_i \ge 0$$
> 
> This constrained formulation can be rewritten as an equivalent unconstrained optimization problem that minimizes a combination of $L_2$ regularization and the Hinge Loss function:
> 
> $$\min_{w,b} \frac{1}{2}\|w\|^2 + C\sum_{i=1}^m \left( 1 - y_i(w \cdot x_i + b) \right)_+$$
> 
> where $(z)_+ = \max(0, z)$ denotes the standard **Hinge Loss** function.

### Soft-Margin Optimality Conditions

> **Context:** Introducing two sets of Lagrange multipliers—$\alpha_i$ for the margin constraints and $\beta_i$ for the non-negativity of the slack variables—yields the soft-margin optimality conditions.

> [!theorem] Soft-Margin KKT Formulation
> 
> Let $\alpha_i \ge 0$ and $\beta_i \ge 0$ be the Lagrange multipliers for the soft-margin constraints. The Lagrangian function is:
> 
> $$L(w,b,\xi,\alpha,\beta) = \frac{1}{2}\|w\|^2 + C\sum_{i=1}^m \xi_i - \sum_{i=1}^m \alpha_i\left[y_i(w \cdot x_i + b) - 1 + \xi_i\right] - \sum_{i=1}^m \beta_i \xi_i$$
> 
> Differentiating with respect to the primal variables yields the following optimality conditions:
> 
> - **Weight Vector:** $w = \sum_{i=1}^m \alpha_i y_i x_i$
>     
> - **Bias Constraint:** $\sum_{i=1}^m \alpha_i y_i = 0$
>     
> - **Slack Multiplier Balance:** $\nabla_{\xi_i} L = C - \alpha_i - \beta_i = 0 \iff \alpha_i + \beta_i = C$
>     
> - **Complementary Slackness:** $\alpha_i [y_i(w \cdot x_i + b) - 1 + \xi_i] = 0$ and $\beta_i \xi_i = 0$
>     
> 
> _These conditions imply an upper bound on the multipliers: $\alpha_i \le C$. Points where $0 < \alpha_i < C$ lie exactly on the margin boundary ($\xi_i = 0$), while points with $\alpha_i = C$ represent margin violations ($\xi_i > 0$)._

### 5. Dimension-Independent Margin Guarantees

### Confidence Margin Framework

> **Context:** Standard VC-dimension generalization bounds depend directly on the feature dimension $N$, making them uninformative in high-dimensional spaces. To explain the success of SVMs in these settings, we analyze the margins of real-valued scoring functions.

> [!definition] Confidence Margins and Loss Functions
> 
> The confidence margin of a real-valued scoring hypothesis $h$ on an observation pair $(x, y)$ is defined as $\rho_h(x, y) = y h(x)$. For a fixed margin parameter $\rho > 0$, the $\rho$-margin loss function $\Phi_\rho$ is defined as:
> 
> $$\Phi_\rho(z) = \begin{cases} 1 & \text{if } z \le 0 \\ 1 - \frac{z}{\rho} & \text{if } 0 < z < \rho \\ 0 & \text{if } z \ge \rho \end{cases}$$
> 
> Given a training sample $S$, the empirical margin risk is bounded by the fraction of points that fail to clear the margin $\rho$:
> 
> $$\widehat{R}_\rho(h) = \frac{1}{m}\sum_{i=1}^m \Phi_\rho\big(y_i h(x_i)\big) \le \frac{1}{m}\sum_{i=1}^m \mathbb{I}_{\{y_i h(x_i) < \rho\}}$$
> ![[1 - Support Vector Machines-1780693022207.webp|432]]

### Rademacher Complexity Analysis

> **Context:** To prove dimension-independent bounds, we first derive an analytical upper bound on the empirical Rademacher complexity of bounded-norm linear operators.

> [!theorem] Rademacher Complexity of Bounded Linear Operators
> 
> Let $S \subseteq \{x : \|x\| \le R\}$ be a training sample of size $m$, and let $\mathcal{H} = \{x \mapsto w \cdot x : \|w\| \le \Lambda\}$ represent the hypothesis space of bounded linear functions. The empirical Rademacher complexity satisfies:
> 
> $$\widehat{\mathfrak{R}}_S(\mathcal{H}) \le \sqrt{\frac{R^2 \Lambda^2}{m}}$$
> 


### General Margin Bound Theorem

> **Context:** Using Talagrand’s Contraction Lemma, we can combine the Rademacher complexity bound with the margin loss function to establish a generalization guarantee for linear classifiers.

> [!theorem] Bounded-Norm Generalization Bounds
> 
> Let $\mathcal{H} = \{x \mapsto w \cdot x : \|w\| \le \Lambda\}$ be a class of linear functions defined over a bounded input domain $\mathcal{X} \subseteq \{x : \|x\| \le R\}$. For any $\delta > 0$ and a fixed margin parameter $\rho > 0$, with probability at least $1-\delta$, every hypothesis $h \in \mathcal{H}$ satisfies the following generalization boundary:
> 
> $$R(h) \le \widehat{R}_\rho(h) + 2\sqrt{\frac{R^2 \Lambda^2 / \rho^2}{m}} + 3\sqrt{\frac{\log\frac{2}{\delta}}{2m}}$$
> 
> 
> **Key Insight:** This generalization bound does not depend on the feature dimension $N$, but rather on the ratio $\frac{R\Lambda}{\rho}$. 
> This structural property justifies the use of maximum-margin hyperplanes in high-dimensional or infinite-dimensional feature spaces.
