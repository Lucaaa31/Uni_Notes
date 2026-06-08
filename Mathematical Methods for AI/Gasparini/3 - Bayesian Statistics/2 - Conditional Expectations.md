In this course, we only use random vectors with density.

$$(X_1, \ldots, X_n)' \sim f(x_1, \ldots, x_n)$$
"has distribution" → **Joint Density** _(description: verbal, a density, a cdf, ...)_

For $n = 2$:

$$(X, Y)' \sim f(x, y)$$

We have defined the conditional densities:
$$f_{Y|X}(y|x) = \frac{f(x,y)}{f_X(x)}$$

$$\boxed{f_{X|Y}(x|y)} = \frac{f(x,y)}{f_Y(y)} = \frac{\text{joint}}{\text{marginal density of } Y \text{ computed in } y}$$
The following conditional expectations follow (similarly for $E(X|Y=y)$):

$$\underbrace{E(Y|X=x)}_{\text{Conditional Expectation of } Y \text{ given } X = x \text{, a number}} = \begin{cases} \displaystyle\sum_k y_k, f_{Y|X}(y_k|x) & \text{if } Y \text{ discrete} \\ \displaystyle\int y, f_{Y|X}(y|x), dy & \text{if } Y \text{ continuous} \end{cases}$$

Consider now:
$$E(Y|X)$$ —> **Conditional Expectation of $Y$ given $X$ → a random variable** (similarly $E(X|Y)$).

For any possible value $x$ which can be taken by $X$, it is a function of $x$, therefore a random variable itself.

---

## Example: Tossing Two Fair Tetrahedrons (Independently)
![[2 - Conditional Expectations-1780521086585.webp]]

> [!example] Joint density and conditional table for $X$ and $S = X + Y$
> ![[2 - Conditional Expectations-1780521176468.webp]]
> 
> **CPT (Conditional Probability Table) for $f_{S|X}(\cdot|1)$:**
> $$f_{S|X}(s|1) = \begin{cases} \frac{1/16}{1/4} = \frac{1}{4} & s = 2 \\ \frac{1/16}{1/4} & s = 3 \\ \frac{1/16}{1/4} & s = 4 \\ \frac{1/16}{1/4} & s = 5 \\ 0 & \text{otherwise} \end{cases}$$
> 
> $$E(S|X=1) = 3.5 \qquad E(S|X=3) = 5.5$$ $$E(S|X=2) = 4.5 \qquad E(S|X=4) = 6.5$$
> Whereas $E(S|X)$ is a random variable:
> 
> $$E(S|X) = \begin{cases} 3.5 & \text{with prob. } \tfrac{1}{4} \\ 4.5 & \text{with prob. } \tfrac{1}{4} \\ 5.5 & \text{with prob. } \tfrac{1}{4} \\ 6.5 & \text{with prob. } \tfrac{1}{4} \end{cases}$$

---

## (Continuous) Example: Bivariate Normal

> [!example] Bivariate Normal conditional expectation
> $$ \binom{X}{Y} \sim \mathcal{N}_2 \left(\binom{\mu_X}{\mu_Y} , \begin{pmatrix} \sigma_X^2 & \sigma_{XY} \\ \sigma_{XY} & \sigma_Y^2 \end{pmatrix}\right)$$
> 
> The conditional density of $Y$ given $X = x$ is:
> 
> $$\mathcal{N}\left(\mu_Y + \rho\frac{\sigma_Y}{\sigma_X}(x - \mu_X),\ (1-\rho^2)\sigma_Y^2\right)$$
> 
> where $\rho = \dfrac{\sigma_{XY}}{\sigma_X \sigma_Y}$. 
> So:
> $$E(Y|X=x) = \mu_Y + \rho\frac{\sigma_Y}{\sigma_X}(x - \mu_X)$$
> So if I know $X = 3.7$, then $$E(Y|X=3.7) = \mu_Y + \rho\dfrac{\sigma_Y}{\sigma_X}(3.7 - \mu_X)$$
> Instead,
> $$E(Y|X) = \mu_Y + \rho\frac{\sigma_Y}{\sigma_X}(X - \mu_X)$$
> is a **random variable**. 

---

## Properties
More generally, for any function $g(\cdot)$:
$$E(g(Y)|X=x) = \begin{cases} \displaystyle\sum g(y_k) f_{Y|X}(y_k|x) \\ \displaystyle\int g(y) f_{Y|X}(y|x) dy \end{cases}$$

as done for regular $E(g(Y))$.

Since $E(Y|X)$ is a r.v., I can compute its expectation:

> [!theorem] Tower Property (Law of Total Expectation)
> $$\boxed{E(E(Y|X)) = E(Y)} \tag{*}$$
> _(the first property of $E(Y|X)$)_

> [!proof]
> (if $Y$ continuous):
> 
> $$E(E(Y|X)) = E!\left(\int y, f_{Y|X}(y|x), dy\right) \quad \leftarrow \text{a function of } x$$
> 
> $$= \int \left(\int y, f_{Y|X}(y|x), dy\right) f_X(x), dx$$
> 
> $$= \iint y, \frac{f(x,y)}{f_X(x)}, dy, f_X(x), dx$$
> 
> $$= \int_y y \underbrace{\int_x f(x,y), dx}_{f_Y(y)}, dy$$
> 
> $$= \int y, f_Y(y), dy = E(Y) \qquad \square$$

---

## Example: Tetrahedron

> [!example] Verification of Tower Property
> $$E(E(S|X)) = \frac{1}{4}\cdot 3.5 + \frac{1}{4}\cdot 4.5 + \frac{1}{4}\cdot 5.5 + \frac{1}{4}\cdot 6.5 = \boxed{5}$$
> 
> At the same time:
> 
> $$E(S) = 2\cdot\frac{1}{16} + 3\cdot\frac{2}{16} + 4\cdot\frac{3}{16} + 5\cdot\frac{4}{16} + 6\cdot\frac{3}{16} + 7\cdot\frac{2}{16} + 8\cdot\frac{1}{16} = \boxed{5}$$
> 
> **SAME!** $\checkmark$

---

## Example: Bivariate Normal

> [!example] Verification for Bivariate Normal
> $$E(E(Y|X)) = E!\left(\mu_Y + \rho\frac{\sigma_Y}{\sigma_X}(X - \mu_X)\right)$$
> 
> $$= E(\mu_Y) + \rho\frac{\sigma_Y}{\sigma_X}\big(E(X) - \mu_X\big) = \mu_Y = E(Y) \qquad \checkmark$$

---

## Other Properties of $E(Y|X)$

$$E(a|X) = a \qquad \text{if } a \text{ constant}$$

> [!theorem] Linearity of Conditional Expectation
> $$\boxed{E(aX + bY|Z) = a E(X|Z) + b E(Y|Z)} \tag{**}$$ _(if $a, b$ constants and $X, Y, Z$ are r.v.'s)_

$$E(X \cdot Y | X) = X E(Y|X)$$

Or more generally, if $g(\cdot)$ is a function that only depends on $X$:

$$\boxed{E(g(X)\cdot Y|X) = g(X)E(Y|X)} \tag{***}$$

---

## Conditional Variance

> [!definition] Conditional Variance
> $$\boxed{\text{Var}(Y|X) = E!\left((Y - E(Y|X))^2 \big| X\right)}$$

Let's compute its expected value:
$$E(\text{Var}(Y|X)) \overset{\text{by def}}{=} E\left(E\left((Y - E(Y|X))^2\big|X\right)\right)$$

Expanding the square:
$$= E\left(E\left(Y^2 + E(Y|X)^2 - 2YE(Y|X)\big|X\right)\right)$$

By linearity $(**)$:
$$= E\left(E(Y^2|X)\right) + E\left(E\left(E(Y|X)^2\big|X\right)\right) - 2E\left(E\left(Y\cdot E(Y|X)\big|X\right)\right)$$

Using properties above:

$$= E(Y^2) + E\left(E(Y|X)^2\cdot E(1|X)\right) - 2E\left(E(Y|X)\cdot E(Y|X)\right)$$

$$= E(Y^2) + E(E(Y|X)^2) - 2E(E(Y|X)^2)$$

$$= E(Y^2) - E(E(Y|X)^2) \pm E(Y)^2$$

$$= \underbrace{E(Y^2) - E(Y)^2}_{\text{Var}(Y)} + E(Y)^2 - E(E(Y|X)^2)$$

$$= \text{Var}(Y) - \Big(E(E(Y|X)^2) - E(E(Y|X))^2\Big)$$

$$E(\text{Var}(Y|X)) = \text{Var}(Y) - \text{Var}(E(Y|X))$$

Or in other words:

> [!theorem] Law of Total Variance
> $$\boxed{\text{Var}(Y) = E(\text{Var}(Y|X)) + \text{Var}(E(Y|X))} \tag{**}$$

---

## Example: Bivariate Normal

> [!example] Verification of Law of Total Variance
> $$\text{Var}(Y) = \sigma_Y^2$$ $$\text{Var}(Y|X) = (1 - \rho^2)\sigma_Y^2 \qquad \text{(in this case, does not depend on } x\text{)}$$ $$E(\text{Var}(Y|X)) = (1-\rho^2)\sigma_Y^2 \qquad \text{(expectation of a constant)}$$ $$E(Y|X) = \mu_Y + \rho\frac{\sigma_Y}{\sigma_X}(X - \mu_X) \quad \text{, a random variable}$$ $$\text{Var}(E(Y|X)) = \rho^2\frac{\sigma_Y^2}{\sigma_X^2},\text{Var}(X) = \rho^2\frac{\sigma_Y^2}{\sigma_X^2}\cdot\sigma_X^2 = \rho^2\sigma_Y^2$$
> 
> So we can verify $(**)$:
> 
> $$\sigma_Y^2 = (1-\rho^2)\sigma_Y^2 + \rho^2\sigma_Y^2 = \sigma_Y^2 - \rho^2\sigma_Y^2 + \rho^2\sigma_Y^2 \qquad \checkmark \qquad \square$$

All of these properties of conditional expectations can be used together with **DAG factorizations**.

---

## Example: DAG Factorization

> [!example] Trivariate Normal via DAG
> $$\begin{pmatrix} X \\ Y \\ Z \end{pmatrix} \sim \mathcal{N}_3(\mu, \Sigma)$$
> such that:
> - $X \sim \mathcal{N}(0,1)$ — the marginal of $X$
> - $Y|X = x \sim \mathcal{N}(x, 1)$ — the conditional of $Y$ given $X=x$ _(or $Y|X \sim \mathcal{N}(X,1)$)_
> - $Z|X, Y \sim \mathcal{N}(X+Y, 1)$ — the conditional of $Z$ given $X$ and $Y$
> 
> This is equivalent to representing $(X, Y, Z)'$ using a DAG:
> 
> ![[2 - Conditional Expectations-1780576729414.webp]]
> 
> $$f(x,y,z) = \underbrace{f_X(x)}_{\text{given}} \cdot \underbrace{f_{Y|X}(y|x)}_{\text{given}} \cdot \underbrace{f_{Z|X,Y}(z|x,y)}_{\text{given}}$$
> 
> Now, using the properties above, you can compute $\mu$ and $\Sigma$:
> 
> $$\mu_X = E(X) = 0 \qquad \text{since } X \sim \mathcal{N}(0,1)$$
> 
> $$\mu_Y \overset{*}{=} E(E(Y|X)) = E(X) = 0$$
> 
> $$\therefore \quad \text{some calculus}$$
> 
> $$\text{Cov}(X,Y) \overset{\text{def of Cov}}{=} E(XY) - E(X)E(Y) = E!\left(E(XY|X)\right) = E!\left(XE(Y|X)\right) \ldots$$
