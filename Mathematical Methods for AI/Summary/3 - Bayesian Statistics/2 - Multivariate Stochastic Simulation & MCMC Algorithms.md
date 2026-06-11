### Direct Multivariate Transformations

> **Context:** Simulating a vector of independent standard normal variables is straightforward. However, when variables are correlated with a specific mean vector $\mu$ and covariance matrix $\Sigma$, we can leverage linear algebra transformations—specifically the Cholesky decomposition—to inject the target correlation structure into independent white noise.

> [!definition] Multivariate Normal Generation (Cholesky Method)
> 
> To simulate a multivariate normal vector $X \sim \mathcal{N}_n(\mu, \Sigma)$ from a vector of independent standard normals $Z \sim \mathcal{N}_k(\mathbf{0}, I)$, we apply the linear transformation:
> 
> $$ \underset{n \times 1}{X} = \underset{n \times k}{A} \underset{k \times 1}{Z} + \underset{n \times 1}{\mu} $$
> 
> where the transformation matrix $A$ must satisfy the relation $AA' = \Sigma$.
> 
> - **Cholesky Factorization:** A highly efficient choice is to set $A$ as the Cholesky decomposition of $\Sigma$, which yields a lower triangular matrix.
>     

### Sequential Conditioning

> **Context:** When dealing with non-normal multivariate distributions, we cannot rely on linear transformations. Instead, we apply the probability chain rule to factorize the joint density into a product of sequential conditional densities, reducing the problem to a sequence of univariate simulations.

> [!definition] Sequential Conditional Simulation
> 
> Any joint probability density function $f(x_1, \ldots, x_n)$ can be sequentially factorized without loss of generality:
> 
> $$f(x_1, \ldots, x_n) = f_{X_1}(x_1) \cdot f_{X_2 \mid X_1}(x_2 \mid x_1) \cdots f_{X_n \mid X_1 \ldots X_{n-1}}(x_n \mid x_1, \ldots, x_{n-1})$$
> 
> **The General Sequential Algorithm:**
> 
> 1. Simulate $x_1$ from the marginal distribution $f_{X_1}(\cdot)$.
>     
> 2. Simulate $x_2$ from the conditional distribution $f_{X_2 \mid X_1}(\cdot \mid x_1)$.
>     
> 3. Continue sequentially until drawing $x_n$ from $f_{X_n \mid X_1 \ldots X_{n-1}}(\cdot \mid x_1, \ldots, x_{n-1})$.
>     

### Graphical Simplifications

> **Context:** For large systems of variables, complete sequential factorization becomes computationally intractable because each step requires conditioning on every single previously sampled variable. If conditional independence properties exist, we can simplify the factorization using a Directed Acyclic Graph (DAG).

> [!definition] Directed Acyclic Graphs (DAGs) in Simulation
> 
> A DAG visualizes conditional dependencies where vertices represent random variables and directed edges represent direct structural influences.
> 
> - If a joint density can be represented as a factorization along a simplified sparse DAG, variables only need to be conditioned on their immediate **parents** ($\pi_i$) rather than the entire historical sequence:
>     
>     $$\boxed{f(x_1, \ldots, x_n) = \prod_{i=1}^{n} f(x_i \mid \pi_i)}$$
>     

### Non-Unique Structural Mappings

> **Context:** It is critical to recognize that a joint probability distribution does not possess a single unique graphical structure; the same underlying multivariate distribution can be validly factorized using different directional ordering choices.

> [!definition] DAG Factorization Equivalence
> 
> A single joint density can yield multiple equivalent network structures depending on the chosen conditioning order. For any bivariate system, two directional paths are equally valid:
> 
> - **Forward Path:** $f(x,y) = f_X(x) \cdot f_{Y \mid X}(y \mid x)$
>     
> - **Reverse Path:** $f(x,y) = f_Y(y) \cdot f_{X \mid Y}(x \mid y)$
>     

### Common Graphical Architectures

> **Context:** Specific dependency structures appear frequently across statistics and machine learning, leading to named graph architectures that capture distinct memory patterns.

> [!definition] Named Graphical Topologies
>
>| **Network Topology**       | **Graph Architecture**                          | **Factorization Rule**                       |
| -------------------------- | ----------------------------------------------- | -------------------------------------------- |
| **Markov Chain**           | $X_1 \to X_2 \to X_3 \to \dots \to X_n$         | $f(x_1) \prod_{i=2}^{n} f(x_i \mid x_{i-1})$ |
| **Independent Components** | $X_1 \quad X_2 \quad X_3 \quad \dots \quad X_n$ | $\prod_{i=1}^{n} f(x_i)$                     |
>
### Iterative Sampling Workarounds

> **Context:** Direct sequential conditional simulation completely breaks down if the marginal distributions are mathematically intractable or impossible to normalize. When global shapes are unknown but localized full conditional distributions are available, we switch to an iterative Markov Chain Monte Carlo (MCMC) strategy known as the Gibbs Sampler.

> [!definition] The Gibbs Sampler
> 
> An MCMC algorithm that bypasses intractable global marginals by updating one coordinate at a time, cycling through the full conditional distribution of each variable given the current states of all other components.
> 
> **Symmetrized Algorithmic Loop (Transitioning from Iteration $J \to J+1$):**
> 
> 1. Draw $x_1^{J+1}$ from $f(x_1 \mid x_2^J, x_3^J, \ldots, x_n^J)$
>     
> 2. Draw $x_2^{J+1}$ from $f(x_2 \mid x_1^{J+1}, x_3^J, \ldots, x_n^J)$
>     
> 3. Draw $x_3^{J+1}$ from $f(x_3 \mid x_1^{J+1}, x_2^{J+1}, x_4^J, \ldots, x_n^J)$
>     
>     $$\vdots$$
>     
>     $n.$ Draw $x_n^{J+1}$ from $f(x_n \mid x_1^{J+1}, x_2^{J+1}, \ldots, x_{n-1}^{J+1})$
>     
> 
> **The Notation of Full Conditionals:**
> 
> Each element is drawn from its conditional density given all remaining companion elements, compactly denoted as $X_{-i}$ to symmetrize the algorithm:
> 
> $$\boxed{f(x_i \mid x_{-i}) = f(x_i \mid x_1, \dots, x_{i-1}, x_{i+1}, \dots, x_n)}$$

### Managing Markovian Dependency

> **Context:** Because the Gibbs sampler generates a sequence where each step depends directly on the previous state, the resulting samples form a dependent Markov chain. To secure independent draws for downstream Monte Carlo analysis, we must manage this chain correlation.

> [!definition] Strategies for Independent MCMC Realizations
> 
> Because the simulation sequence $(x_1, y_1), \ldots, (x_B, y_B)$ is a dependent chain, we extract uncorrelated realizations using one of two common approaches:
> 
> - **Multi-Chain Initialization:** Run a large number of entirely separate Markov chains, each starting from a different randomized starting point, and harvest the final state of each chain after a long burn-in period.
>     
> - **Thinning and Sub-sampling:** Run a single continuous long chain but extract states separated by massive iteration gaps (e.g., keeping only realizations at distant checkpoints $B_1 = 10^6, B_2 = 10^{12}$), allowing the chain sufficient steps to lose its memory of previous states.
>