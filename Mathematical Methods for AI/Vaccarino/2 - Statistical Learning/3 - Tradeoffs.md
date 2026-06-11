## Choosing a Function Class

We wish to make the (expected) **generalization risk** as small as possible, while using as few computational resources as possible.

First, a suitable class $\mathcal{G}$ of prediction functions has to be chosen, driven by:
- the **complexity** of the class (e.g., is it rich enough to adequately approximate, or even contain, the optimal prediction function $g^*$?),
- the **ease of training** the learner via optimizing the training loss,
- how **accurately the training loss estimates the risk** within class $\mathcal{G}$,
- the **feature types** (categorical, continuous, etc.).

### The Core Tradeoff

The choice of a suitable function class $\mathcal{G}$ involves a tradeoff between conflicting factors.

> [!example] Example
>  A learner from a **simple class** $\mathcal{G}$ can be trained very quickly, but may not approximate $g^*$ very well, whereas a learner from a **rich class** $\mathcal{G}$ that contains $g^*$ may require a lot of computing resources to train.

To better understand the relation between **model complexity**, **computational simplicity**, and **estimation accuracy**, it is useful to decompose the generalization risk into several parts.

---
## Decomposing the Generalization Risk
Recall that for a training set $\tau$, prediction function class $\mathcal{G}$, and learner $g_\tau^{\mathcal{G}}$, the generalization risk is the expected loss:

$$\ell(g_\tau^{\mathcal{G}}) = \mathbb{E}\text{Loss}(Y, g_\tau^{\mathcal{G}}(X)).$$
We can decompose the generalization risk into **three components**:

$$\ell(g_\tau^{\mathcal{G}}) = \underbrace{\ell^*}_{\text{irreducible risk}} + \underbrace{\ell(g^{\mathcal{G}}) - \ell^*}_{\text{approximation error}} + \underbrace{\ell(g_\tau^{\mathcal{G}}) - \ell(g^{\mathcal{G}})}_{\text{statistical error}} \tag{1}$$

where:
- $\ell^* := \ell(g^*)$ is the **irreducible risk**
- $g^{\mathcal{G}} := \arg\min_{g \in \mathcal{G}} \ell(g)$ is the **best learner within class $\mathcal{G}$**

> [!note] No learner can predict a new response with a smaller risk than $\ell^*$.

### Approximation Error
The approximation error $\ell(g^{\mathcal{G}}) - \ell^*$ measures the difference between the irreducible risk and the best possible risk that can be obtained within function class $\mathcal{G}$.
- Determining a suitable class $\mathcal{G}$ and minimizing $\ell(g)$ over this class is purely a problem of **numerical and functional analysis**, as the training data $\tau$ are not present.
- For a fixed $\mathcal{G}$ that does **not** contain the optimal $g^*$, the approximation error cannot be made arbitrarily small and may be the **dominant component** in the generalization risk.
- The only way to reduce the approximation error is by **expanding the class $\mathcal{G}$** to include a larger set of possible functions.

### Statistical (Estimation) Error
The statistical error $\ell(g_\tau^{\mathcal{G}}) - \ell(g^{\mathcal{G}})$ depends on the training set $\tau$ and, in particular, on how well the learner $g_\tau^{\mathcal{G}}$ estimates the best possible prediction function $g^{\mathcal{G}}$ within class $\mathcal{G}$.

For any sensible estimator this error should **decay to zero** (in probability or expectation) as the training size tends to infinity.

> [!important] The Approximation–Estimation Tradeoff 
> Two competing demands:
> 
> - The class $\mathcal{G}$ has to be **simple enough** so that the statistical error is not too large.
> - The class $\mathcal{G}$ has to be **rich enough** to ensure a small approximation error.
> 
> Thus, there is a tradeoff between the **approximation** and **estimation** errors.

---
## Squared-Error Loss Decomposition
For the special case of the **squared-error loss**, the generalization risk equals:
$$\ell(g_\tau^{\mathcal{G}}) = \mathbb{E}(Y - g_\tau^{\mathcal{G}}(X))^2.$$

Recall the optimal prediction function is $g^*(x) = \mathbb{E}[Y \mid X = x]$. Decomposition (1) is interpreted as follows:

1. **Irreducible error:** $\ell^* = \mathbb{E}(Y - g^*(X))^2$
2. **Approximation error:** $\mathbb{E}(g^{\mathcal{G}}(X) - g^*(X))^2$
3. **Statistical error:** $\ell(g_\tau^{\mathcal{G}}) - \ell(g^{\mathcal{G}})$ has no direct interpretation as an expected squared error, _unless_ $\mathcal{G}$ is the class of **linear functions**, i.e. $g(x) = x^\top \beta$. In this case: $$\ell(g_\tau^{\mathcal{G}}) - \ell(g^{\mathcal{G}}) = \mathbb{E}(g_\tau^{\mathcal{G}}(X) - g^{\mathcal{G}}(X))^2.$$

### For Linear Prediction Functions

For a linear class $\mathcal{G}$, the generalization risk decomposes as:

$$\ell(g_\tau^{\mathcal{G}}) = \mathbb{E}(g_\tau^{\mathcal{G}}(X) - Y)^2 = \ell^* + \underbrace{\mathbb{E}(g^{\mathcal{G}}(X) - g^*(X))^2}_{\text{approximation error}} + \underbrace{\mathbb{E}(g_\tau^{\mathcal{G}}(X) - g^{\mathcal{G}}(X))^2}_{\text{statistical error}}.$$

> [!note] In this decomposition the **statistical error is the only term that depends on the training set**.

---

## Example: Polynomial Regression

Consider the polynomial regression example: ${U_i} \sim_{\text{iid}} \mathcal{U}(0,1)$ and

$$(Y_i \mid U_i = u_i) \sim \mathcal{N}(10 - 140u_i + 400u_i^2 - 250u_i^3,\ 25).$$
![[3 - Tradeoffs-1780690070874.webp]]
We use feature vectors of the form $x = [1, u, u^2, \ldots, u^{p-1}]^\top$. Let $\mathcal{G} = \mathcal{G}_p$ be the class of linear functions of such vectors and let $g^* = x^\top \beta^*$. Conditional on $X = x$, we have:

$$Y = g^* + \varepsilon(x), \qquad \varepsilon(x) \sim \mathcal{N}(0, \ell^*),$$

where $\ell^* = \mathbb{E}(Y - g^*(X))^2 = 25$ is the **irreducible error**.

### Approximation Error (Polynomial Case)
Any function $g \in \mathcal{G}_p$ can be written as $g(x) = [1, u, \ldots, u^{p-1}]\beta$, and so $g(X)$ is distributed as $[1, U, \ldots, U^{p-1}]\beta$ where $U \sim \mathcal{U}(0,1)$. Similarly $g^*$ is distributed as $[1, U, U^2, U^3]\beta^*$.

The approximation error is:
$$\int_0^1 \left( [1, u, \ldots, u^{p-1}]\beta - [1, u, u^2, u^3]\beta^* \right)^2 du.$$
To minimize, set the gradient w.r.t. $\beta$ to zero, obtaining $p$ linear equations:
$$\int_0^1 \left([1, \ldots, u^{p-1}]\beta - [1, u, u^2, u^3]\beta^*\right) u^{k}, du = 0, \quad k = 0, 1, \ldots, p-1.$$
This can be written as the matrix equation:

$$H_p \beta = H_+ \beta^*,$$

where $H_p = \int_0^1 [1, \ldots, u^{p-1}]^\top [1, \ldots, u^{p-1}] du$ is a $p \times p$ **Hilbert matrix**, and $\tilde H$ is the $p \times 4$ upper-left block of $H_{\tilde p}$, with $\tilde p = \max\{p, 4\}$.

### Solution and Resulting Errors

The solution $\beta_p$ is:

$$\beta_p = \begin{cases} \frac{65}{6}, & p = 1, \\ [-\tfrac{20}{3}, 35]^\top, & p = 2, \\ [-\tfrac{5}{2}, 10, 25]^\top, & p = 3, \\ [10, -140, 400, -250, 0, \ldots, 0]^\top, & p \geq 4. \end{cases}$$

Hence the approximation error $\mathbb{E}(g^{\mathcal{G}_p}(X) - g^*(X))^2$ is:

|$p$|Approximation error|
|---|---|
|1|$\tfrac{32225}{252} \approx 127.9$|
|2|$\tfrac{1625}{63} \approx 25.8$|
|3|$\tfrac{625}{28} \approx 22.3$|
|$\geq 4$|$0$|

In general, as the class of approximating functions $\mathcal{G}$ becomes **more complex**, the approximation error **goes down**.

### Statistical Error (Polynomial Case)

Since $g_\tau(x) = x^\top \widehat{\beta}$, the statistical error can be written as:

$$\int_0^1 \left([1, \ldots, u^{p-1}](\widehat{\beta} - \beta_p)\right)^2 du = (\widehat{\beta} - \beta_p)^\top H_p (\widehat{\beta} - \beta_p). \tag{2}$$

The statistical error depends on the estimate $\widehat{\beta}$, which in turn depends on the training set $\tau$.

In general, as the class of approximating functions $\mathcal{G}$ becomes **more complex**, the statistical error **increases**.

### Generalization Risk Behavior
![[3 - Tradeoffs-1780690444943.webp]]

> [!summary] Key picture
> The generalization risk for a particular training set is the sum of the irreducible error, the approximation error, and the statistical error.
> 
> - The **approximation error decreases to zero** as $p$ increases.
> - The **statistical error tends to increase** after $p = 4$.

---

## Expected Statistical Error

We can understand the statistical error better by considering its expected behavior, averaged over many training sets. For the squared-error loss (and general $\mathcal{G}$):

$$\ell(g_\tau^{\mathcal{G}}) = \mathbb{E}(g_\tau^{\mathcal{G}}(X) - Y)^2 = \ell^* + \mathbb{E}\left(g_\tau^{\mathcal{G}}(X) - g^*(X)\right)^2 = \ell^* + \mathbb{E}D^2(X, \tau),$$

where $D(x, \tau) := g_\tau^{\mathcal{G}}(x) - g^*(x)$.

In this decomposition, the statistical error and approximation error are **combined**.

---
## Bias–Variance Tradeoff

The expectation of $D^2(x, \mathcal{T})$ for a **random** training set $\mathcal{T}$ is:

$$\mathbb{E}\left(g_{\mathcal{T}}^{\mathcal{G}}(x) - g^*(x)\right)^2 = \mathbb{E}D^2(x, \mathcal{T}) = (\mathbb{E}D(x, \mathcal{T}))^2 + \text{Var}D(x, \mathcal{T})$$

$$= \underbrace{(\mathbb{E} g_{\mathcal{T}}^{\mathcal{G}}(x) - g^*(x))^2}_{\text{pointwise squared bias}} + \underbrace{\text{Var} g_{\mathcal{T}}^{\mathcal{G}}(x)}_{\text{pointwise variance}}.$$

- **Pointwise squared bias** — measures how close $g_{\mathcal{T}}^{\mathcal{G}}(x)$ is on average to the true $g^*(x)$. It can be **reduced by making $\mathcal{G}$ more complex**.
- **Pointwise variance** — measures the squared deviation of $g_{\mathcal{T}}^{\mathcal{G}}(x)$ from its expected value $\mathbb{E} g_{\mathcal{T}}^{\mathcal{G}}(x)$. It can be **reduced by making $\mathcal{G}$ less complex**.

> [!important] We are thus seeking learners that provide an **optimal bias–variance tradeoff**.

### Expected Generalization Risk

The expected generalization risk can be written as:

$$\mathbb{E}\ell(g_{\mathcal{T}}^{\mathcal{G}}) = \ell^* + \mathbb{E}D^2(X, \mathcal{T}),$$

where $X$ and $\mathcal{T}$ are independent. It therefore decomposes as:

$$\mathbb{E}\ell(g_{\mathcal{T}}^{\mathcal{G}}) = \ell^* + \underbrace{\mathbb{E}\left(\mathbb{E}[g_{\mathcal{T}}^{\mathcal{G}}(X) \mid X] - g^*(X)\right)^2}_{\text{expected squared bias}} + \underbrace{\mathbb{E}\big[\text{Var}[g_{\mathcal{T}}^{\mathcal{G}}(X) \mid X]\big]}_{\text{expected variance}}.$$

---

## Summary

|Component|Depends on training set?|Reduced by|
|---|:-:|---|
|Irreducible risk $\ell^*$|No|Cannot be reduced|
|Approximation error / **bias**|No|Making $\mathcal{G}$ **more** complex|
|Statistical error / **variance**|Yes|Making $\mathcal{G}$ **less** complex|

> [!abstract] Big idea Increasing model complexity lowers bias (approximation error) but raises variance (statistical error). The goal is the sweet spot that minimizes total generalization risk.