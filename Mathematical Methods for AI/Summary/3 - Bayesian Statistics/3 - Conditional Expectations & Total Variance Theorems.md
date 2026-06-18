### Density Framework Foundations

> **Context:** In multivariate continuous frameworks, analyzing the joint probability density function $f(x_1, \ldots, x_n)$ allows us to isolate the behavior of a subset of variables. By conditioning the joint density on a known marginal density, we establish the mathematical foundation for conditional distributions.

> [!definition] Conditional Densities
> 
> For a bivariate random vector $(X, Y)'$ governed by a continuous joint density function $f(x, y)$, the conditional density profiles are defined as:
> 
> - **Conditional density of $Y$ given $X=x$:**
>     
>     $$f_{Y \mid X}(y \mid x) = \frac{f(x,y)}{f_X(x)}$$
>     
> - **Conditional density of $X$ given $Y=y$:**
>     
>     $$\boxed{f_{X|Y}(x|y)} = \frac{f(x,y)}{f_Y(y)} = \frac{\text{joint}}{\text{marginal density of } Y \text{ computed in } y}$$
>     
>     where $f_X(x)$ and $f_Y(y)$ represent the respective marginal density functions.
>     

### Dual Nature Expectations

> **Context:** A conditional expectation can take two distinct forms depending on how the conditioning variable is treated. If conditioned on a fixed realization $x$, it evaluates to a specific numerical value; if conditioned on the random variable $X$ itself, it retains its stochastic nature and behaves as a random variable.

> [!definition] Conditional Expectation Formulations
> 
> - **As a Static Value ($E(Y \mid X = x)$):** The evaluated mean output over the conditional density profile, yielding a fixed number:
>     
>     $$E(Y \mid X=x) = \begin{cases} \sum_k y_k f_{Y \mid X}(y_k \mid x) & \text{if } Y \text{ is discrete} \\ \int_{-\infty}^{+\infty} y f_{Y \mid X}(y \mid x) \, dy & \text{if } Y \text{ is continuous} \end{cases}$$
>     
> - **As a Random Variable ($E(Y \mid X)$):** A function of the random variable $X$. Because its evaluation depends on the realization that $X$ will take, $E(Y \mid X)$ is itself a random variable possessing its own sampling distribution.
>     
> 
> **Generalized Functional Extension:**
> 
> For any measurable function $g(\cdot)$, the conditional expectation of the transformed variable is defined as:
> 
> $$E(g(Y) \mid X=x) = \begin{cases} \sum_k g(y_k) f_{Y \mid X}(y_k \mid x) & \text{if } Y \text{ is discrete} \\ \int_{-\infty}^{+\infty} g(y) f_{Y \mid X}(y \mid x) \, dy & \text{if } Y \text{ is continuous} \end{cases}$$

### Unconditional Mean Recovery

> **Context:** Because the conditional expectation $E(Y \mid X)$ is a valid random variable, we can evaluate its expectation over the distribution of $X$. The Tower Property proves that averaging all localized conditional means recovers the global unconditional mean of $Y$.

> [!theorem] Tower Property (Law of Total Expectation)
> 
> The expected value of the conditional expectation of $Y$ given $X$ is equal to the unconditional expected value of $Y$:
> 
> $$E\big(E(Y \mid X)\big) = E(Y)$$
> 

### Operator Algebraic Constraints

> **Context:** Conditional expectations share many algebraic properties with standard expectations, such as linearity. However, they possess a unique property regarding variables that are deterministic functions of the conditioning set: any function completely determined by $X$ behaves as a constant and can be factored out.

> [!definition] Fundamental Properties of Conditional Expectations
> 
> Let $a, b$ be constants, and $X, Y, Z$ be random variables. The operator obeys the following structural rules:
> 
> - **Constant Conditioning:**
>     
>     $$E(a \mid X) = a$$
>     
> - **Linearity:**
>     
>     $$\boxed{E(aX + bY \mid Z) = aE(X \mid Z) + bE(Y \mid Z)}$$
>     
> - **Factoring Known Quantities (Pull-Out Property):**
>     
>     If a function $g(\cdot)$ depends exclusively on the conditioning variable $X$, it acts as a known constant within the conditional subspace and can be factored outside the operator:
>     
>     $$\boxed{E(g(X) \cdot Y \mid X) = g(X) E(Y \mid X)}$$
>     

### Localized Dispersion Profiles

> **Context:** Just as the conditional expectation tracks the localized mean of a sliced distribution, the conditional variance quantifies the dispersion or spread remaining in $Y$ after the structural information of $X$ has been fully accounted for.

> [!definition] Conditional Variance
> 
> The conditional variance of $Y$ given $X$ is a random variable defined as the conditional expected squared deviation of $Y$ around its conditional mean:
> 
> $$\boxed{\mathrm{Var}(Y \mid X) = E\left[ (Y - E(Y \mid X))^2 \;\middle|\; X \right]}$$
> 
> _Note: By expanding the quadratic expression and applying conditional linearity, it can be expressed in computing form as:_
> 
> $$\mathrm{Var}(Y \mid X) = E(Y^2 \mid X) - [E(Y \mid X)]^2$$

### Variance Component Decomposition

> **Context:** The total variations within a response variable $Y$ can be partitioned into two independent structural components. The Law of Total Variance splits global variance into the average variance felt within localized clusters plus the macroscopic variance occurring between the cluster means.

> [!theorem] Law of Total Variance (Variance Decomposition Theorem)
> 
> The unconditional variance of a random variable $Y$ can be decomposed into the expectation of the conditional variance plus the variance of the conditional expectation:
> 
> $$\boxed{\mathrm{Var}(Y) = E\big(\mathrm{Var}(Y \mid X)\big) + \mathrm{Var}\big(E(Y \mid X)\big)}$$
> 
