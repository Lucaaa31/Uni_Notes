## Interaction

> [!important] A linear model is said to have **interactions** if the (true) **differential effects** of one predictor given a level of another predictor **depends on such level**.

![[7 - Confidence vs Prediction Intervals-1780320322620.webp]]

In the INSULATE example, there is only **one interaction term** $\beta_3$, so the p-value for:

$$H_0: \beta_3 = 0$$

(p-value $= 0.0007307$) can be read both:

- in the line relative to estimating $\beta_3$
- in the output of `anova(additivo, interattivo)`

---

### GOLF Example

#### Additive model (no interaction)
![[7 - Confidence vs Prediction Intervals-1780320369806.webp]]

#### Model with interaction — difference DRIVER–IRON changes depending on club
![[7 - Confidence vs Prediction Intervals-1780320426401.webp]]

The p-value ($0.001079$) for the **global no-interaction hypothesis**:

$$H_0: \text{all interaction parameters are null}$$

(in this case interaction parameters are $\beta_5, \beta_6, \beta_7$) can only be obtained by:

```r
anova(small,          large)
#       ↑              ↑
# no interaction    model with
#    model          interactions
```

> [!important] If we **don't reject** the no-interaction hypothesis, then we can think of the two factors as **adding their effects** — a simplification of scientific discovery.

---
## Confidence and Prediction Intervals
**Linear model:**
$$Y = X\beta + \varepsilon$$
The observations $y_1, \ldots, y_n$ together with the values in the $X$ matrix:

$$\begin{pmatrix} y_1 \\ \vdots \\ y_n \end{pmatrix} \quad \begin{bmatrix} 1 & & \\ \vdots & X & \\ 1 & & \end{bmatrix}$$

A **new configuration of predictors** $x_f$ (a new row, not in the training data) may represent two questions:
1. What is an estimate of $E(Y_{n+1})$, the **expectation** of the new $y$-value corresponding to $x_f$?
2. What is a reasonable **prediction** of $Y_{n+1}$, the new $y$-value corresponding to $x_f$? _(prediction problem — well known in Machine Learning)_

These are **two separate, but related, problems.**

---

## 1) Estimating $E(Y_{n+1})$ → Confidence Intervals

Assume $Y_{n+1}$ will be sampled independently of $Y_1, \ldots, Y_n$; then:

$$E(Y_{n+1}) = x_f \beta$$

An estimate of it is simply:

$$\hat{Y}_{n+1} = \widehat{E(Y_{n+1})} = x_f \hat{\beta} \qquad \text{("new" fitted value)}$$

Then:

$$E(\hat{Y}_{n+1}) = E(x_f \hat{\beta}) = x_f E(\hat{\beta}) = x_f \beta $$

$$\text{Var}(\hat{Y}_{n+1}) = \text{Var}(x_f \hat{\beta}) $$

Applying $\begin{cases}\text{VarCov}(AX) = A \\ \text{VarCov}(X)A' \end{cases}$:

$$= x_f \text{VarCov}(\hat{\beta}) x_f'$$

$$= x_f \sigma^2 (X'X)^{-1} x_f'$$

In turn, we can estimate this variance using an estimate of $\sigma^2$:

$$\widehat{\text{Var}}(\hat{Y}_{n+1}) = x_f \underbrace{\hat{\sigma}^2}_{\substack{\text{RMS} \ \text{ =} \ (\text{residual std error})^2 \ \text{readable from output}}} (X'X)^{-1} x_f'$$

$$\hat{\sigma}^2 = \frac{E'E}{n-p}$$

---

Now, as usual, we can derive mathematically:

$$\underbrace{\frac{x_f\hat{\beta} - x_f\beta}{\sqrt{x_f\cancel{\sigma^2}(X'X)^{-1}x_f'}}}_{\sim \mathcal{N}(0,1)} \cdot \underbrace{\frac{1}{\sqrt{\dfrac{E'E}{(n-p) \cancel{\sigma^2}}}}}_{\sim \sqrt{\dfrac{x^2}{n-p}}} \sim t(n-p)$$

And finally, a $t$-like $(1-\alpha)$-level **confidence interval** for $x_f\beta = E(Y_{n+1})$:

$$\boxed{x_f\hat{\beta} \pm; t_{\alpha/2}(n-p)\sqrt{ \underbrace{\frac{E'E}{n-p}}_{\sigma^2}x_f(X'X)^{-1}x_f'}}$$
> [!important] This formula represents the **confidence interval for the expectation of $Y_{n+1}$ corresponding to $x_f$


Example in simple linear regression: $x_f\hat{\beta} = \hat{\beta}_0 + \hat{\beta}_1 x_f$
![[7 - Confidence vs Prediction Intervals-1780321429441.webp|497]]

---

## 2) Predicting a New $Y_{n+1}$ → Prediction Intervals

$$Y_{n+1} = X\beta + \varepsilon_{n+1}$$

An obvious prediction for it is:

$$\hat{Y}_{n+1} = x_f\hat{\beta} \qquad \text{(same as estimate of its mean)}$$

$Y_{n+1}$ is a random variable:

$$Y_{n+1} \sim \mathcal{N}(x_f\beta, \sigma^2)$$

(we assume $Y_{n+1}$ independent of $Y_1, \ldots, Y_n$)

whereas $\hat{Y}_{n+1}$ is also a random variable — a function of $\hat{\beta}$:

$$\hat{Y}_{n+1} \sim \mathcal{N}(?,?) \quad \longrightarrow \quad \begin{cases} E(\hat{Y}_{n+1}) = E(x_f \hat \beta)= x_f E(\hat \beta) = x_f\beta \\ \text{Var}(\hat{Y}_{n+1}) = x_f\text{VarCov}(\hat{\beta}) x_f = x_f\sigma^2(X'X)^{-1}x_f' \end{cases}$$

Consider the **prediction error**:

$$Y_{n+1} - \hat{Y}_{n+1}$$

It is a new random variable, **normal**, since it is a difference between two independent normal variables:

$$Y_{n+1} - \hat{Y}_{n+1} \sim \mathcal{N}\Big(0,\text{Var}(Y_{n+1}) + \text{Var}(\hat{Y}_{n+1})\Big)$$

$$= \mathcal{N}\Big(0,\sigma^2 + x_f\sigma^2(X'X)^{-1}x_f'\Big)$$

$$= \mathcal{N}\Big(0,\sigma^2\big(1 + x_f(X'X)^{-1}x_f'\big)\Big)$$

Therefore, proceeding as before:

$$\frac{Y_{n+1} - \hat{Y}_{n+1}}{\sqrt{\underbrace{\frac{E'E}{n-p}}_{\sigma^2}\Big(1 + x_f(X'X)^{-1}x_f'\Big)}} \sim t(n-p)$$

And we finally obtain a $(1-\alpha)$-level **prediction interval**:

$$\boxed{\hat{Y}_{n+1} = x_f\hat{\beta} \pm t_{\alpha/2}(n-p)\sqrt{\frac{E'E}{n-p}\Big(1 + x_f(X'X)^{-1}x_f'\Big)}}$$
> [!important] The uncertainty quantification for a **single value** $Y_{n+1}$ is **higher** than the uncertainty quantification for its **mean** $E(Y_{n+1})$.

