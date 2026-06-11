## Generalized Linear Models (GLMs)

> [!info] Motivation for GLMs
> **So far:**
>
> $$Y = X\beta + \varepsilon \qquad = \text{"signal"} + \text{"error"}$$
>
> where:
> - $\varepsilon$ is **normally distributed**
> - **predictors** enter via $X$.
>
> We want to **generalize** to $Y$ not necessarily normally distributed — e.g. Bernoulli, Poisson, Weibull, or other distributions.
>
> To link all these cases together, look at $E(Y) = X\beta$.

---
### What if $Y \sim \text{Bernoulli}(p)$?

$$Y = \begin{cases} 0 & \text{with prob } 1-p \\ 1 & \text{with prob } p \end{cases}$$

If we try $E(Y) = X\beta$, **it does not work**:
$$E(Y) = p = X\beta \quad \text{(e.g. } \beta_0 + \beta_1 x \text{ with a single quantitative predictor } x\text{)}$$
![[8 - Logistic Regression-1780325171230.webp]]

So the idea is to use a **link function** $g(\cdot)$ (≈ '70) and write:

> [!definition] Generalized Linear Model
> $$g(E(Y)) = X\beta$$
>
> The linear combination ($X$) of predictors is **kept**.

---

## The Logit Link Function

In the binary case: $$g(p) = X\beta$$What is a good choice for $g$?

If $p = 0.7$, i.e. $p = P(Y=1) = P(\text{"success"})$:

> [!example] "Brasil will win this year's World Cup with probability 0.7, or 70%"
> In betting language: _"Brasil is given to win 7 to 3"_ — this is talking in **odds**.

Formally, the **odds** is a function $o(p)$:

> [!definition] Odds and Log-Odds
> $$o(p) = \frac{p}{1-p}$$
![[8 - Logistic Regression-1780325307850.webp]]
>
$o(p)$ ranges from $0$ to $+\infty$.
To obtain a $g(\cdot)$ ranging from $-\infty$ to $+\infty$, we take the logarithm:
$$\log o(p) = \log \frac{p}{1-p} = \log p - \log(1-p) = \text{logodds}(p)$$
![[8 - Logistic Regression-1780325388338.webp]]

---

## Logistic Regression

Now we can write:
$$\text{logodds}(p) = X\beta$$
and see how predictors affect the mean response $p$ through the link function:
$$g(p) = \text{logodds}(p) = \text{logit}(p) = \log \frac{p}{1-p}$$
> [!important] A **Generalized Linear Model** for binary (Bernoulli) $Y$ and link function **logit** is called **logistic regression**.
### Inverse Transformation: The Logistic Function

Starting from $$\ell = \log \dfrac{p}{1-p}$$
$$e^\ell = e^{\log p/(1-p)} = \frac{p}{1-p}$$

$$e^\ell - p e^\ell = p$$

$$e^\ell = p(1 + e^\ell)$$

> [!theorem] The Logistic Function (Inverse of Logit)
> $$\boxed{p = \frac{e^\ell}{1 + e^\ell}} \qquad \text{the logistic function}$$
>
![[8 - Logistic Regression-1780325497174.webp]]

> [!note] Other choices of link function are possible, e.g. the CDF of $\mathcal{N}(0,1)$ (**probit**).

---
## Data Structure in Practice

In practice, the database will look like:

| $y$ (response, 0-1 variable) | $x$      | $z$      | $w$      | (predictors — may be continuous or discrete) |
| ---------------------------- | -------- | -------- | -------- | -------------------------------------------- |
| 0                            | $x_1$    | $z_1$    | $w_1$    |                                              |
| 1                            | $\vdots$ | $\vdots$ | $\vdots$ |                                              |
| 0                            | $\vdots$ | $\vdots$ | $\vdots$ |                                              |
| 0                            | $x_n$    | $z_n$    | $w_n$    |                                              |

**Example:**
- $y = \text{political vote} = \begin{cases}1 & \text{right} \\ 0 & \text{left}\end{cases}$  
- $x = \text{income (quantitative)}$ 
- $z = \text{age (quantitative)}$ 
- $w = \text{sex (binary, M/F)}$ 

---

## Estimation via Maximum Likelihood
As for linear models, we estimate $\beta$'s (the effects of the predictors on the binary response) using **maximum likelihood**.

**Likelihood** = density of $y_1, \ldots, y_n$:
$$L = \prod_{y_i = 1} \frac{e^{\ell_i}}{1+e^{\ell_i}} \cdot \prod_{y_i = 0} \left(1 - \frac{e^{\ell_i}}{1+e^{\ell_i}}\right)$$
where $\dfrac{e^{\ell_i}}{1+e^{\ell_i}}$ is $p$ for the $i$-th observation.

This can be written compactly as:

$$L = \prod_{i=1}^{n} \left(\frac{e^{\ell_i}}{1+e^{\ell_i}}\right)^{y_i} \left(1 - \frac{e^{\ell_i}}{1+e^{\ell_i}}\right)^{1-y_i}$$
since $y_1, \ldots, y_n$ are independent $\text{Bernoulli}\left(\dfrac{e^{\ell_i}}{1+e^{\ell_i}}\right)$.

### Numerical Optimization
Maximizing the likelihood does **not** have an explicit analytical solution, but we can use **numerical methods** to obtain $\hat{\beta}$.
Also, in mathematical statistics we prove that, approximately:

> [!theorem] Asymptotic Sampling Distribution of $\hat{\beta}$ (GLM)
> $$\hat{\beta} \sim \mathcal{N}_p (\beta, VarCov(\hat \beta))$$
> where:
> - $\mathcal{N}_p$ is the **normal approximation** to the sampling distribution of $\hat{\beta}$
> - $\hat \beta$ are approximately unbiased
> - $VarCov(\hat \beta)$ is estimated based on data


---

## R Implementation

To do so in R, we extend `lm()` methods to `glm()`.

|Function|Description|
|---|---|
|`summary()`|Displays detailed results for the fitted model|
|`coefficients()`, `coef()`|Lists the model parameters (intercept and slopes)|
|`confint()`|Provides confidence intervals for the model parameters (95% by default)|
|`residuals()`|Lists the residual values for a fitted model|
|`anova()`|Generates an ANOVA table comparing two fitted models|
|`plot()`|Generates diagnostic plots for evaluating the fit|
|`predict()`|Uses a fitted model to predict response values for a new dataset|

All these functions apply to objects of class `glm`, obtained by the R function:

```r
glm(response ~ predictors, <optimal arguments>)
```
- ~ is a formula linking response and predictors
For a **binary response**, we must specify `family = "binomial"` as an optional argument.

---

## Heart Disease Example

From the dataset found at `statandr.com`, we have approximately:
$$\hat{\beta}_0 \approx -3.05, \qquad \hat{\beta}_1 \approx 0.05$$
So that:

$$\widehat{\text{logit}}(p) \approx -3.05 + 0.05 \cdot \text{age}$$

$$\log \frac{\hat{p}}{1-\hat{p}} \approx -3.05 + 0.05 \cdot \text{age}$$

$$\hat{p} \approx \frac{e^{-3.05 + 0.05 \cdot \text{age}}}{1 + e^{-3.05 + 0.05 \cdot \text{age}}}$$

**Interpretation:**

- As age increases, $\hat{p}$ also increases.
- For each additional year of age, $\text{logit}(p)$ is estimated to increase by $0.05$.
- How much the estimate of $p$ changes with age **depends on age** (non-linear relationship).

---

## Logistic Regression as a Classifier

Logistic regression can be seen as a **classifier** into two classes (0/1, left/right, presence/absence, …).
Depending on $\hat{p}$, we can predict $Y$ to be 0 or 1:
![[8 - Logistic Regression-1780326400013.webp]]

---
## Confusion Matrix & Performance Metrics

The classification gives rise to the usual **confusion matrix**:

| Y \ $\hat Y$ | 0   | 1   |
| ------------ | --- | --- |
| 0            | TN  | FP  |
| 1            | FN  | TP  |

**Sensitivity** (true positive rate):

$$\text{sensitivity} = P(\hat{Y}=1 \mid Y=1) \approx \frac{TP}{TP + FN}$$

**Specificity** (true negative rate):

$$\text{specificity} = P(\hat{Y}=0 \mid Y=0) \approx \frac{TN}{TN + FP}$$

We can **vary the threshold** (instead of 0.5, choose any number $\in (0,1)$) and obtain different sensitivities and specificities.

---

## ROC Curve

A plot of **sensitivity vs (1 − specificity)** is called the **ROC curve**.

![[8 - Logistic Regression-1780326617356.webp]]

It is a curve in $\mathbb{R}^2$ given in **parametric terms** — both $x$ and $y$ values (sensitivity and 1−specificity) are functions of the **threshold** $\in (0,1)$.

> [!important] The ideal/perfect ROC curve goes straight up to $(0,1)$ then across — the real curve lies between the diagonal (random classifier) and this ideal.