### 1. Function Class Selection Criteria

### Structural Constraints

> **Context:** Choosing an appropriate function space $\mathcal{G}$ requires balancing computational constraints against the statistical capacity of the model. The selected class must be expressive enough to capture the target phenomenon while allowing stable estimation from finite data.

> [!definition] Structural Drivers of Class Selection
> 
> The choice of a predictive function space $\mathcal{G}$ is bound by four fundamental structural dimensions:
> 
> - **Approximation Capacity:** The richness of $\mathcal{G}$ determines its capacity to approximate or contain the true optimal population decision function $g^*$.
>     
> - **Optimization Feasibility:** The algebraic topology of the training loss landscape over $\mathcal{G}$ governs the convergence efficiency of optimization solvers.
>     
> - **Generalization Consistency:** The mathematical complexity of $\mathcal{G}$ influences how reliably the empirical training loss acts as an unbiased proxy for the true population risk.
>     
> - **Feature Support Compatibility:** The functional structures within $\mathcal{G}$ must align natively with the input data types (e.g., continuous metrics or discrete categorical topologies).
>     

### 2. Generalization Risk Decomposition

### Three-Component Framework

> **Context:** To understand the statistical behavior of a learning algorithm, the total population generalization risk can be factored into separate, orthogonal errors that isolate structural misspecification from empirical sampling noise.

> [!theorem] Fundamental Risk Decomposition
> 
> Let $\tau$ be an observed training set, $\mathcal{G}$ a constrained hypothesis space, and $g_\tau^{\mathcal{G}}$ the empirical minimizer of the training loss. 
> 
> The population-level generalization risk $\ell(g_\tau^{\mathcal{G}})$ factors into three distinct operational components:
> 
> $$\ell(g_\tau^{\mathcal{G}}) = \underbrace{\ell^*}_{\text{irreducible risk}} + \underbrace{\left[\ell(g^{\mathcal{G}}) - \ell^*\right]}_{\text{Approximation Error}} + \underbrace{\left[\ell(g_\tau^{\mathcal{G}}) - \ell(g^{\mathcal{G}})\right]}_{\text{Statistical Error}}$$
> 
> where the reference functions and risks are defined as:
> 
> - **Irreducible Risk ($\ell^*$):** The theoretical minimum risk boundary achieved by the absolute population optimal function $g^*$:
>     
>     $$\ell^* := \ell(g^*) = \arg\min_g \mathbb{E}\left[\mathrm{Loss}(Y, g(X))\right]$$
>     
> - **Best In-Class Function ($g^{\mathcal{G}}$):** The specific function within the restricted family $\mathcal{G}$ that minimizes true population risk:
>     
>     $$g^{\mathcal{G}} := \arg\min_{g \in \mathcal{G}} \ell(g) = \arg\min_{g \in \mathcal{G}} \mathbb{E}\left[\mathrm{Loss}(Y, g(X))\right]$$
>     

### Component Characterization

> **Context:** Each component within the risk decomposition reflects a separate mathematical aspect of the learning process, varying in its dependence on the training data and functional limits.

> [!definition] Approximation vs. Statistical Error
> 
> - **Approximation Error:** The distance $\ell(g^{\mathcal{G}}) - \ell^*$ measures the structural loss caused by restricting the model to the class $\mathcal{G}$. It is entirely independent of the training data $\tau$ and can only be reduced by expanding the function class $\mathcal{G}$.
>     
> - **Statistical (Estimation) Error:** The distance $\ell(g_\tau^{\mathcal{G}}) - \ell(g^{\mathcal{G}})$ measures the variation caused by optimizing over a finite sample $\tau$ instead of the true distribution. For statistically consistent learners, this error converges to zero as the sample size approaches infinity:
>     
>     $$\lim_{n \to \infty} \mathbb{P}\left( \left| \ell(g_\tau^{\mathcal{G}}) - \ell(g^{\mathcal{G}}) \right| > \epsilon \right) = 0$$
>     

### 3. Special Case: Squared-Error Loss

### Quadratic Simplifications

> **Context:** Under an $L_2$ error metric, the geometric properties of conditional expectations simplify the risk decomposition. For linear models, these terms can be mapped directly to expected squared differences between the respective functions.

> [!theorem] Linear Class Quadratic Decomposition
> 
> Under the squared-error loss function $\mathrm{Loss}(y, \widehat{y}) = (y - \widehat{y})^2$, the true optimal function is $g^*(x) = \mathbb{E}[Y \mid X = x]$. If $\mathcal{G}$ is restricted to the class of linear functions $g(x) = x^\top \beta$, the components simplify to a sum of explicit expected squared errors:
> 
> $$\ell(g_\tau^{\mathcal{G}}) = \mathbb{E}\left[(g_\tau^{\mathcal{G}}(X) - Y)^2\right] = \ell^* + \mathbb{E}\left[(g^{\mathcal{G}}(X) - g^*(X))^2\right] + \mathbb{E}\left[(g_\tau^{\mathcal{G}}(X) - g^{\mathcal{G}}(X))^2\right]$$
> 
> where:
> 
> - **Irreducible Variance:** $\ell^* = \mathbb{E}\left[(Y - g^*(X))^2\right]$
>     
> - **Quadratic Approximation Error:** $\mathbb{E}\left[(g^{\mathcal{G}}(X) - g^*(X))^2\right]$
>     
> - **Quadratic Statistical Error:** $\mathbb{E}\left[(g_\tau^{\mathcal{G}}(X) - g^{\mathcal{G}}(X))^2\right]$
>     

### 4. The Bias-Variance Tradeoff

### Pointwise Decomposition

> **Context:** By taking the expectation over all possible random training sets $\mathcal{T}$, the combined estimation and approximation error can be factored into a deterministic bias component and a stochastic variance component.

> [!theorem] Pointwise Bias-Variance Identity
> 
> Let $\mathcal{T}$ represent a random training set sample, and let $D(x, \mathcal{T}) := g_{\mathcal{T}}^{\mathcal{G}}(x) - g^*(x)$ define the deviation from the true function at a fixed point $x$. Taking the expectation with respect to the distribution of $\mathcal{T}$ resolves into:
> 
> $$\mathbb{E}_{\mathcal{T}}\left[ \left( g_{\mathcal{T}}^{\mathcal{G}}(x) - g^*(x) \right)^2 \right] = \left( \mathbb{E}_{\mathcal{T}}[g_{\mathcal{T}}^{\mathcal{G}}(x)] - g^*(x) \right)^2 + \mathbb{E}_{\mathcal{T}}\left[ \left( g_{\mathcal{T}}^{\mathcal{G}}(x) - \mathbb{E}_{\mathcal{T}}[g_{\mathcal{T}}^{\mathcal{G}}(x)] \right)^2 \right]$$
> 
> $$= \underbrace{(\mathbb{E} g_{\mathcal{T}}^{\mathcal{G}}(x) - g^*(x))^2}_{\text{pointwise squared bias}} + \underbrace{\text{Var}( g_{\mathcal{T}}^{\mathcal{G}}(x))}_{\text{pointwise variance}}.$$
> 
> - **Pointwise Squared Bias:** $\left(\mathbb{E}_{\mathcal{T}}[g_{\mathcal{T}}^{\mathcal{G}}(x)] - g^*(x)\right)^2$ measures the structural discrepancy between the average model prediction and the true target.
>     
> - **Pointwise Variance:** $\mathrm{Var}\left(g_{\mathcal{T}}^{\mathcal{G}}(x)\right)$ measures the sensitivity of the model's predictions to random fluctuations in the training sample.
>     

### Integrated Population Expectations

> **Context:** Integrating the pointwise identity over the entire input feature space distribution establishes the final decomposition for the expected generalization risk of a learning algorithm.

> [!definition] Expected Generalization Risk Decomposition
> 
> Let the random input vector $X$ and the random training dataset $\mathcal{T}$ be independent. The expected generalization risk $\mathbb{E}_{\mathcal{T}}\left[\ell(g_{\mathcal{T}}^{\mathcal{G}})\right]$ decomposes into three distinct integrated components:
> 
> $$\mathbb{E}_{\mathcal{T}}\left[\ell(g_{\mathcal{T}}^{\mathcal{G}})\right] = \ell^* + \underbrace{\mathbb{E}\left(\mathbb{E}[g_{\mathcal{T}}^{\mathcal{G}}(X) \mid X] - g^*(X)\right)^2}_{\text{expected squared bias}} + \underbrace{\mathbb{E}\big[\text{Var}[g_{\mathcal{T}}^{\mathcal{G}}(X) \mid X]\big]}_{\text{expected variance}}.$$
> 
> - **Integrated Squared Bias:** $\mathbb{E}_X\left[ \left( \mathbb{E}_{\mathcal{T}}[g_{\mathcal{T}}^{\mathcal{G}}(X) \mid X] - g^*(X) \right)^2 \right]$
>     
> - **Integrated Model Variance:** $\mathbb{E}_X\left[ \mathrm{Var}_{\mathcal{T}}\left( g_{\mathcal{T}}^{\mathcal{G}}(X) \mid X \right) \right]$
>     

### Structural Summary Reference Matrix

> **Context:** This reference matrix summarizes the core trade-offs involved in adjusting model complexity.

|**Risk Component**|**Dependent on Training Data T?**|**Mathematical Mitigation Strategy**|
|---|---|---|
|**Irreducible Risk ($\ell^*$)**|No|None (inherent stochastic boundary limit).|
|**Approximation Error / Bias**|No|Expand the capacity/complexity of the function class $\mathcal{G}$.|
|**Statistical Error / Variance**|Yes|Restrict the capacity/complexity of the function class $\mathcal{G}$ or increase $n$.|