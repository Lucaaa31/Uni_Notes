## Example — Ad-hoc Method for $X \sim N_n(\mu, \Sigma)$

> [!note] Specifically aimed at the normal distribution

For the **standard normal** $(Z_1, \dots, Z_n) \sim N_n(\underline{0}, \underset{k \times k}{I})$, just repeat univariate simulation for $Z_i$, $n$ times independently.

For **general** $X$, use the linear transformation: $$ \underset{n \times 1}{X} = \underset{n \times k}{A} \underset{k \times 1}{Z} + \underset{n \times 1}{\mu} $$

where $AA' = \Sigma$.

> [!tip] Choice of $A$ 
> For example, take $A = \text{Cholesky}(\Sigma)$ — pretty because it is **lower triangular**.

---

## General Case (Not Necessarily Normal)

We **reduce** simulating $n$ variables to simulating one variable $n$ times.

If the **joint density** is available, we write (using the definition of $f_{X_n \mid X_1 \dots X_{n-1}}(\cdot \mid \cdot)$):

$$ f(x_1, \dots, x_n) = f_{X_1 \dots X_{n-1}}(x_1, \dots, x_{n-1}) \cdot f_{X_n \mid X_1 \dots X_{n-1}}(x_n \mid x_1, \dots, x_{n-1}) $$

Doing it again for $x_{n-1}$:

$$ = f_{X_1 \dots X_{n-2}}(x_1, \dots, x_{n-2}) \cdot f_{X_{n-1} \mid X_1 \dots X_{n-2}}(x_{n-1} \mid x_1, \dots, x_{n-2}) \cdot f_{X_n \mid X_1 \dots X_{n-1}}(x_n \mid x_1, \dots, x_{n-1}) $$

Doing it again ($\vdots$), we obtain the full factorization:

$$ = f_{X_1}(x_1), f_{X_2 \mid X_1}(x_2 \mid x_1), f_{X_3 \mid X_1, X_2}(x_3 \mid x_1, x_2) \cdots f_{X_n \mid X_1 \dots X_{n-1}}(x_n \mid x_1, \dots, x_{n-1}) $$

### A General Algorithm

> [!abstract] Sequential conditional simulation
> 
> 1. Simulate $X_1$ from its **marginal** $f_{X_1}(\cdot)$; obtain $x_1$
> 2. Simulate $X_2$ from the **conditional** $f_{X_2 \mid X_1}(\cdot \mid x_1)$; get $x_2$
> 3. Simulate $X_n$ from $f_{X_n \mid X_1 \dots X_{n-1}}(\cdot \mid x_1, \dots, x_{n-1})$

### Example — Bivariate Normal

$$ \begin{pmatrix} X \ Y \end{pmatrix} \sim N_2\left( \begin{pmatrix} \mu_X \ \mu_Y \end{pmatrix}, \begin{pmatrix} \sigma_X^2 & \sigma_{XY} \ \sigma_{XY} & \sigma_Y^2 \end{pmatrix} \right) $$

1. Simulate $X$ from $N(\mu_X, \sigma_X^2)$, get $X = x$
2. Simulate $Y$ from $$ N\left( \mu_Y + \rho,\frac{\sigma_Y}{\sigma_X}(x - \mu_X),; (1-\rho^2)\sigma^2 \right) $$

**Graphically:** Step 1 draws $x$ from the marginal $f_X(x)$ along the horizontal axis; Step 2 draws $y$ from the conditional slice, and $(x, y)$ is the result lying on the contour lines of $f(x,y)$.

---

## Conditional Independence & DAGs

Sometimes the **complete graph**

$$ X_1 \rightarrow X_2 \rightarrow X_3 \rightarrow X_4 \cdots \rightarrow X_n $$

(with all back-edges) can be **simplified** if certain conditional independence relationships apply (to be verified).

In other words, sometimes we get a **Directed Acyclic Graph (DAG)** simpler than the complete DAG above.

### Example — Trivariate with a Zero in $\Sigma$

$$ \begin{pmatrix} X \\ Y \\ Z \end{pmatrix} \sim N_3\left( \begin{pmatrix} \mu_X \\ \mu_Y \\ \mu_Z \end{pmatrix}, \begin{pmatrix} \sigma_X^2 & 0 & \ \sigma_{XZ} \\ 0 & \sigma_Y^2 & \sigma_{YZ} \\ \sigma_{XZ} & \sigma_{YZ} & \sigma_Z^2 \end{pmatrix} \right) $$

Always true (general factorization): $$ f(x, y, z) = f_X(x), f_{Y \mid X}(y \mid x), f_{Z \mid X, Y}(z \mid x, y) $$

Since $X$ and $Y$ are **independent** ($\sigma_{XY} = 0$): $$ f(x, y, z) = f_X(x), f_Y(y), f_{Z \mid X, Y}(z \mid x, y) $$

This corresponds to the DAG: $$ X \rightarrow Z \leftarrow Y $$ (the edge $X \rightarrow Y$ is dropped).

> [!summary] General principle 
> If a joint density can be represented as a **factorization along a DAG**, then following the same DAG we can simulate $(X_1, \dots, X_n)$ sequentially with some efficiency gain.

### Example — Binary $(X, Y, Z, W)$

DAG structure: $X \rightarrow Z$, $Y \rightarrow Z$, $Z \rightarrow W$.

> [!note] If this is true, all I need is $p_X,; p_{Z00},; p_{Z10},; p_{Z01},; p_{Z11},; p_{W0},; p_{W1}$

$$ X \sim \text{Bernoulli}(p_X) \qquad Y \sim \text{Bernoulli}(p_Y) $$

For $Z$:

|Condition|Distribution|
|---|---|
|$X=0,\ Y=0$|$Z \sim \text{Bernoulli}(p_{Z00})$|
|$X=1,\ Y=0$|$Z \sim \text{Bernoulli}(p_{Z10})$|
|$X=0,\ Y=1$|$Z \sim \text{Bernoulli}(p_{Z01})$|
|$X=1,\ Y=1$|$Z \sim \text{Bernoulli}(p_{Z11})$|

Then for $W$:

|Condition|Distribution|
|---|---|
|$Z=0$|$W \sim \text{Bernoulli}(p_{W0})$|
|$Z=1$|$W \sim \text{Bernoulli}(p_{W1})$|

**To simulate** $(x, y, z, w)$ from $(X, Y, Z, W)$, I simulate:

1. $X \sim \text{Bernoulli}(p_X)$, get $x$
2. $Y \sim \text{Bernoulli}(p_Y)$, get $y$
3. $Z \sim \text{Bernoulli}$ depending (as above) on $x$ and $y$, get $z$
4. $W \sim \text{Bernoulli}$ depending on $z$, as above

---

## Notes

### 1) DAGs are not unique

**Example — bivariate normal:**

$$ \begin{pmatrix} X \ Y \end{pmatrix} \sim N_2\left( \begin{pmatrix} \mu_X \ \mu_Y \end{pmatrix}, \begin{pmatrix} \sigma_X^2 & \sigma_{XY} \ \sigma_{XY} & \sigma_Y^2 \end{pmatrix} \right) $$

Two equally valid factorizations:

$$ X \rightarrow Y : \quad f(x, y) = f_X(x), f_{Y \mid X}(y \mid x) $$ $$ X \leftarrow Y : \quad f(x, y) = f_Y(y), f_{X \mid Y}(x \mid y) $$

### 2) Some DAGs have names

**Example 1 — Markov Chain:** 
![[2 - Multivariate Stochastic Simulation-1781102877609.webp]]

**Example 2 — Independent Components:** $
![[2 - Multivariate Stochastic Simulation-1781102903393.webp]]
$$ f(x_1, \dots, x_n) = f_{X_1}(x_1), f_{X_2}(x_2) \cdots f_{X_n}(x_n) $$

---

## When the General Algorithm Fails — The Gibbs Sampler

The general algorithm above may **not work** if some of the sequential conditional densities are not known. For example, the marginal density of $X$, $f_X(x)$, may not be available.

> [!warning] Example 
> A joint $f(x, y)$ for which $f_X(x)$ is **not known**, whereas the full conditionals $f_{Y \mid X}(y \mid x)$ and $f_{X \mid Y}(x \mid y)$ **are** known.

### Idea

Iterate by alternating the two known conditionals, starting from some $x_0$:
![[2 - Multivariate Stochastic Simulation-1781102978446.webp]]

> [!definition] Convergence intuition 
> Hopefully, as I iterate the procedure, the system **forgets** about the starting point and will visit the whole distribution progressively.

Therefore I take as the result of the simulation: $$ (x_B, y_B) \quad \text{for large } B $$
> [!definition] Gibbs Sampler 
> This is called the **Gibbs sampler**. It is a particular **MCMC** (Markov Chain Monte Carlo), since the sequence $$ (x_1, y_1), (x_2, y_2), \dots, (x_B, y_B) $$ is a Markov Chain.

### General Form (symmetrized, iterate over $J$)

At iteration $J \to J+1$:

1. Simulate $X_1$ from $f_{X_1 \mid X_2 \dots X_n}(\cdot \mid x_2^J, \dots, x_n^J)$; obtain $x_1^{J+1}$
2. Simulate $X_2$ from $f_{X_2 \mid X_1 X_3 \dots X_n}(\cdot \mid x_1^{J+1}, x_3^J, \dots, x_n^J)$; get $x_2^{J+1}$
3. Simulate $X_3$ from $f_{X_3 \mid X_1 X_2 X_4 \dots X_n}(\cdot \mid x_1^{J+1}, x_2^{J+1}, x_4^J, \dots, x_n^J)$; get $x_3^{J+1}$ $\quad \vdots$ $n$. Simulate $X_n$ from $f_{X_n \mid X_1 \dots X_{n-1}}(\cdot \mid x_1^{J+1}, \dots, x_{n-1}^{J+1})$; get $x_n^{J+1}$

> [!note] Full conditional density 
> This way, every $i$-th component is treated **equally**, since I simulate from the **full conditional density of $X_i$ given all other components**: $$ f_{X_i \mid \underbrace{X_1 \dots X_{i-1} X_{i+1} \dots X_n}_{X_{-i}}}(\cdot \mid x_{-i}) $$ This **symmetrizes** the algorithm. Of course, it works when these single full conditional densities are available.

### Example — Bivariate Normal (Gibbs)

Iterate $J = 1, \dots, B$ for large $B$ and get $(x_B, y_B)$.

At each step, use either $f_{X \mid Y}(x \mid y)$ or $f_{Y \mid X}(y \mid x)$.

- $x_0$ = any starting point, **not** simulated from $f_X(x)$.
- The chain walks toward and around the high-density region (contour lines), eventually exploring the whole distribution.

---

## Getting Independent Iterations

To get **independent** iterations $(x_1, \dots, x_n)^k$, $; k = 1, 2, \dots$, either:

- **Use many different starting points** and their many different Markov Chains, **or**
- **Use a single chain** and select realizations at **large gaps**, e.g. $$ B_1 = 10^6 \rightarrow k = 1 $$ $$ B_2 = 10^{12} \rightarrow k = 2 $$ $$ \vdots $$

---

> [!abstract] Key takeaways
> 
> - **Normal vectors:** simulate via $X = AZ + \mu$ with $AA' = \Sigma$ (Cholesky).
> - **General vectors:** factorize the joint density and simulate **sequentially** along a chain rule / DAG.
> - **Conditional independence** simplifies the complete DAG and gives efficiency gains; DAG factorizations are **not unique**.
> - **Gibbs sampler:** when marginals are unavailable but **full conditionals** are, iterate the conditionals as an MCMC and read off $(x_B, y_B)$ for large $B$.
> - **Independence between draws:** use multiple chains or large thinning gaps.