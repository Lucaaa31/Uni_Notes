## Recall

> [!summary] Key Results for Inference
> $$Y_{n \times 1} = X_{n \times p} \beta_{p \times 1} + \varepsilon_{n \times 1}$$
>
> $$\varepsilon \sim \mathcal{N}_n(0, \sigma^2 I)$$
>
> The MLE of $\beta$ are:
>
> $$\hat{\beta} \sim \mathcal{N}_p\left(\beta,\ \sigma^2 (X'X)^{-1}\right) \tag{1}$$
>
> Moreover:
>
> $$\hat{Y} = X\hat{\beta} \quad \text{(the fitted values)}$$
>
> $$E = Y - \hat{Y} \quad \text{(residuals)}$$
>
> $$\hat{\sigma}^2 = \frac{E'E}{n-p} = \frac{\sum(Y_i - \hat{Y}_i)^2}{n-p} \quad \text{preferred estimator of } \sigma^2, \text{ called RMS (Residual Mean Square)}$$
>
> $$\hat{\sigma} = \sqrt{\hat{\sigma}^2} = \underbrace{\text{Residual Std Error}}_{\text{in R}} = \sqrt{\text{RMS}}$$
>
> Then it can be proved that:
>
> $$\frac{(n-p),\hat{\sigma}^2}{\sigma^2} = \frac{E'E}{\sigma^2} \sim \chi^2(n-p) \quad \text{(a chi-square distribution with } n-p \text{ degrees of freedom)} \tag{2}$$
>
> independently of $\hat{\beta}$.

We will use these distributional results to make inference about $\beta$ and $\sigma^2$: confidence intervals (c.i.) and tests.

---

## Confidence Intervals for a Single $\beta_i$

Because of (1), each of them is:

$$\hat{\beta}_i \sim \mathcal{N}\left(\beta_i,\ \sigma^2 (X'X)^{-1}_{i+1,,i+1}\right) \tag{3}$$

So we can construct c.i. and tests in the way we learned before. E.g.:

$$P\left(\hat{\beta}_i - z_{\alpha/2}\sqrt{\sigma^2(X'X)^{-1}_{i+1,i+1}} \leq; \beta_i \leq \hat{\beta}_i + z_{\alpha/2}\sqrt{\sigma^2(X'X)^{-1}_{i+1,i+1}}\right) = 1-\alpha$$

> [!warning] Is this a confidence interval of level $1-\alpha$? **No**, — because we do not know $\sigma^2$.
> If we estimate it, we have to use $t_{\alpha/2}(n-p)$ instead of $z_{\alpha/2}$, and obtain:
> $$P\left(\hat{\beta}_i - t_{\alpha/2}(n-p)\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}} ;\leq; \beta_i ;\leq; \hat{\beta}_i + t_{\alpha/2}(n-p)\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}\right) = 1-\alpha$$
>This is a proper (computable from the data) $1-\alpha$ level c.i. for $\beta_i$. Written in shorthand notation:
$$\hat{\beta}_i ;\pm; t_{\alpha/2}(n-p),\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}$$
This is the c.i. we obtained in R (`confint()`) and Python yesterday.

---

## Standard Error and Hypothesis Testing

$$\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}$$

is the (estimated) **standard error** of $\hat{\beta}_i$, readable in the output row corresponding to $\hat{\beta}_i$.

Similarly, based on (3), we can construct a test of the null hypothesis:

$$H_0: \beta_i = 0$$

(If this hypothesis is true, we can forget the corresponding predictor, since its effect is 0.)

### Example: Insulate, Simple Additive Model
$$\begin{pmatrix} \text{cons}_1 \\ \vdots \\ \text{cons}_{56} \end{pmatrix}

\begin{pmatrix} 1 & 1 & \text{temp}_1 \\ 1 & \vdots & \vdots \\ \vdots & 0 & \vdots \\ 1 & 0 & \text{temp}_{56} \end{pmatrix} \begin{pmatrix} \beta_0 \\ \beta_1 \\ \beta_2 \end{pmatrix}

- \varepsilon$$

If $\beta_2 = 0$, then `temp` is a useless predictor, since:

$$E(\text{cons}_i) = \beta_0 + \beta_1 I_i + \cancel{\beta_2 \cdot \text{temp}_i}$$

where $I_i$ is a dummy variable (indicator function):

$$I_i = \begin{cases} 1 & \text{if prima} \\ 0 & \text{if dopo} \end{cases} \quad \text{for the } i\text{-th case}$$

### Rejection Rule

How do we test whether $\beta_i = 0$? 
We reject the null hypothesis if $\hat{\beta}_i$ is too large (very positive) or too small (very negative). 
In particular, we reject $H_0: \beta_i = 0$ if:

$$\left|\frac{\hat{\beta}_i}{\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}}\right| > t_{\alpha/2}(56 - 3)$$

at type-I error level $\alpha$.

Independently of $\alpha$, we can look at the **p-value** of the test, and if the p-value is "obviously" small (smaller than any reasonable $\alpha$), we reject $H_0$.

In the insulate example yesterday, all $\beta$'s were significantly different from 0.

In other cases:

|p-value|Significance|
|---|---|
|$< 0.05$|`*`|
|$< 0.01$|`**`|
|$< 0.001$|`***`|

---
## Graph Interpretation (Directed Acyclic Graph)
# METTI FOTO
An intuitive **Directed Acyclic Graph (DAG)**:

- Node $Y$ ← first column in $X$ matrix (intercept, node `1`)
- $x_1, x_2, \ldots, x_i, \ldots, x_{p-1}$ ← second to $p$-th columns in $X$ matrix

Testing each $\beta_i$ (asking whether $\beta_i = 0$) is equivalent to discussing the **relevance of the arrow** from $x_i$ to $Y$.
We may also test a **whole subgroup of predictors** (more than one arrow). The extreme example is the **null model**, when all $x$'s disappear:

$$H_0: \beta_1 = \beta_2 = \cdots = \beta_{p-1}$$

is testing whether the null model is appropriate.

---
## Geometric Picture

Consider $Y = \begin{pmatrix} Y_1 \\ \vdots \\ Y_n \end{pmatrix}$ and each column of $X$ as a point in $\mathbb{R}^n$.

$$\text{span}(X) = \text{linear subspace of } \mathbb{R}^n \text{ spanned by the columns of } X$$
![[4 - Inference in Linear Models-1780266487147.webp]]
$\hat{Y} = X\hat{\beta}$ is the **projection of $Y$ into span$(X)$**.

Now, if $\beta_i = 0$, that means span$(X)$ is **reduced** since the $(i+1)$-st column disappears. We are therefore in a subspace of span$(X)$:

$$\Omega_0 \subseteq \Omega$$
---
## The Null Model
The null model:
$$\begin{pmatrix} Y_1 \\ \vdots \\ Y_n \end{pmatrix} = \begin{pmatrix} 1 \\ \vdots \\ 1 \end{pmatrix} \beta_0 + \varepsilon$$

$$\Omega_0 = \text{subspace of } \mathbb{R}^n \text{ of vectors with all components equal}$$
Software usually gives a p-value for the null model hypothesis as a standard output (**global p-value**).

We now learn how to test **intermediate hypotheses** for subgroups of predictors (not necessarily one or all of them).

Testing a subgroup of predictors is testing a **nested hypothesis**:

$$H_0: \text{a fixed subgroup of } \beta\text{'s} = 0$$
E.g.:
$$H_0: \beta_4 = \beta_5 = \beta_6 = 0$$
---
## Testing Nested Hypotheses with ANOVA

In R, this is done with:

```r
anova(small, large)
```

where `large` is the whole model, whereas `small` is a model subject to the hypothesized restriction.

### Example
**Large model** (6 columns of $X$ in addition to intercept):
$$E(Y) = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 x_3 + \beta_4 x_4 + \beta_5 x_5 + \beta_6 x_6$$
**Small model**:
$$E(Y) = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 x_3$$
A small model is a subspace of $\Omega$: $\Omega_0 \subseteq \Omega = \text{span}(X)$.
![[4 - Inference in Linear Models-1780266885359.webp]]
We also have $\hat{Y}_{(0)} = X_{(0)}\hat{\beta}_{(0)}$, the fitted values of the small model.

### The F-Test
The following rule is intuitive:
> [!tip] Reject the small model if $|\hat{Y} - \hat{Y}_0|^2$ is too large.
Sparing the mathematics, this is an **F-test**, since we reject the small model if:

> [!theorem] F-Test for Nested Hypotheses
> $$\frac{|\hat{Y} - \hat{Y}_{(0)}|^2}{(p - q),\hat{\sigma}^2} > F_\alpha(p-q,; n-p)$$
>
> where:
> - $q$ = dimension of the small subspace
> - $F_\alpha(p-q, n-p)$ is a cut-off point of the F distribution.

In practice, the p-value is given by `anova(small, large)`.

---

## Example: Insulate — Additive vs Interaction Model

### Simple Additive Model

```r
lm(cons ~ quando + temp)
```

$$E(\text{cons}_i) = \beta_0 + \beta_1 I_i + \beta_2 \text{temp}_i = \begin{cases} \beta_0 + \beta_1 + \beta_2 \text{temp}_i & \text{if } I_i = 1 \\ \beta_0 + \beta_2 \text{temp}_i & \text{if } I_i = 0 \end{cases}$$
### Model with Interaction

```r
lm(cons ~ quando * temp)
```
$$E(\text{cons}_i) = \beta_0 + \beta_1 I_i + \beta_2 \text{temp}_i + \beta_3 (I_i \times \text{temp}_i)$$
In this case, the interaction term is the product of $I_i \times \text{temp}_i$.
$$
\begin{cases}
\beta_0 + \beta_1 + \beta_2\text{tempi} + \beta_3 + \beta_3\text{tempi} = (\beta_0 + \beta_1) + (\beta_2 + \beta_3) \text{tempi} & \text{if } I_i = 1 \\ 
\beta_0 + \beta_2\text{tempi} & \text{if } I_i = 0
\end{cases}
$$

Here:
- `large` = model with interaction
- `small` = additive model (no interaction)
Therefore, `anova(small, large)` is equivalent to testing $H_0: \beta_3 = 0$.