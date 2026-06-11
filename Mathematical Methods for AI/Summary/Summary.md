## Recap Folder
### From Bernoulli Trials to Binomial and Multinomial Distributions
### Inference, Goodness-of-Fit and Sampling Models
> [!summary] Summary of Discrete Random Vectors
> 
> The three foundational discrete random vectors covered up to this point are:
> 
> - **$\text{Multinomial}(n, p_1, \dots, p_D)$:** Counts of $D$ categories over $n$ independent trials with replacement.
>     
> - **$\text{Multinoulli}(p_1, \dots, p_D)$:** A special case of the Multinomial vector for a single trial ($n = 1$).
>     
> - **$\text{Hypergeometric}(n, C_1, \dots, C_D)$:** Counts of categories when sampling _without_ replacement from a finite population with known category capacities $C_i$.
## Bivariate Categorical Analysis

## Linear Models Folder

> [!definition] Linear Model (Matrix Form)
> $$\underset{n \times 1}{Y} = \underset{n \times p}{X} \quad  \underset{p \times 1}{\beta} + \underset{n \times 1}{\varepsilon}$$
>
> - $\beta_{p \times 1}$ = p-vector of **unknown coefficients** containing the effects of the predictors
> - $Y_{n \times 1}$ = vector of responses, random variables
> - $X_{n \times p}$ = known matrix of input values of predictors, often under the control of the experimenter
> - $\varepsilon_{n \times 1}$ = vector of random variables representing **errors**

> [!definition] Normal Errors
>
> $$\underset{n \times 1}{\varepsilon} = \varepsilon_1, \ldots, \varepsilon_n$$ i.i.d. $\mathcal{N}(0, \sigma^2)$, so that:
>
> $$\varepsilon \sim \mathcal{N}_n\left(\begin{pmatrix} 0 \\ \vdots \\ 0 \end{pmatrix}, \begin{pmatrix} \sigma^2 & 0 & \cdots & 0 \\ 0 & \sigma^2 & & 0 \\ \vdots & & \ddots & \vdots \\ 0 & \cdots & 0 & \sigma^2 \end{pmatrix}\right) = \mathcal{N}_n\left(\mathbf{0}_{n \times 1}, \sigma^2 I_{n \times n}\right)$$
>
> where $I_{n \times n}$ is the identity matrix.
>
> Therefore, since linear transformations of normals are normal:
>
> $$Y \sim \mathcal{N}_n(X\beta, \sigma^2 I)$$
>
> $$E(Y) = E(X\beta + \varepsilon) = X\beta + E(\varepsilon) = X\beta$$
>
> $$\mathrm{VarCov}(Y) = \mathrm{VarCov}(X\beta + \varepsilon) = \mathrm{VarCov}(\varepsilon) = \sigma^2 I$$

This is a **parametric models**, so we have to find $\beta$ and $\sigma^2$ ($X$'s are known). 
A classic method used is the **Maximum Likelihood**.

> [!definition] Maximum Likelihood Estimate
> The likelihood is the density of the observations $Y_1, \ldots, Y_n$ viewed as a function of the unknown parameters.
> Under the assumption of normally distributed errors, maximizing this likelihood is equivalent to minimizing the sum of squared residuals.
>We get the **Ordinary Least Squares (OLS)** normal equations::$$\boxed{X'X\beta = X'y}$$
>That is a classic Least Square Problem.
>We can solve $\beta$ by assuming that $X'X$ is invertible (i.e. the $p$ columns of $X$ are linearly independent):
>$$\boxed{\hat{\beta} = (X'X)^{-1}X'y} \quad \leftarrow \text{least square estimate of } \beta$$
>
>Once we have maximized the likelihood in $\beta$, we have to find $\sigma^2$ and obtain:
>$$\hat{\sigma}^2 = \frac{(y - X\hat{\beta})'(y - X\hat{\beta})}{n} = \frac{\sum\left(y_i - \sum_{p=0}^{j-1} x_{i,j}\hat{\beta}_p\right)^2}{n} = \frac{1}{n}$$
>That is the sum of squared differences between observed and fitted values.

> [!definition] Residuals and Fitted Values
> - $\hat{\beta}$ — estimates of $\beta$
> - $\hat{y} = X\hat{\beta}$ — fitted $y$ values
> - $y$ — observed $y$ values
> - $e = (y - X\hat{\beta})$ — vector of residuals
> - $e'e = (y - X\hat{\beta})'(y - X\hat{\beta})$ — **residual sum of squares

> [!definition] Null Model   ($X$ = column of 1)
$$\hat{\beta}_0 = \bar{y}, \qquad \hat{\sigma}^2 = \frac{\sum(y_i - \bar{y})^2}{n}$$ 
>Unbiased estimation: $$s^2 = \dfrac{\sum(y_i - \bar{y})^2}{n-1}$$

> [!definition] Linear Regression ($Y_i = \beta_0 + \beta_1 x_i + \varepsilon_i$)
>  $$\begin{cases}\hat{\beta}_1 = \frac{\sum(x_i - \bar{x})(y_i - \bar{y})}{\sum(x_i - \bar{x})^2}, \\ \hat{\beta}_0 = \bar{y} - \hat{\beta}_1\bar{x} \end{cases}$$ The line goes through $(\bar{x}, \bar{y})$.  Estimation of $\sigma^2$: $$\text{RMS} = \frac{\sum\big(y_i - (\hat{\beta}_0 + \hat{\beta}_1 x_i)\big)^2}{n-2}$$

> [!definition] Estimate vs Estimator
> - **Estimate:** $\hat{\beta} = (X'X)^{-1}X'y$, it is a quantity calculated by the algorithm for a certain value of $y$
> - **Estimator:** $\hat{\beta} = (X'X)^{-1}X'Y$, it is the corresponding Random Variable

> [!definition] Sampling Distribution of $\hat{\beta}$
>$$\hat{\beta} \sim \mathcal{N}_p\big(\beta,\; \sigma^2 (X'X)^{-1}\big)$$
>
>- **Unbiasedness:** $E(\hat{\beta}) = \beta$
>- **Variance-covariance matrix:** $\text{VarCov}(\hat{\beta}) = \sigma^2 (X'X)^{-1}$
>- **Variance of a single coefficient:** $\text{Var}(\hat{\beta}_i) = \sigma^2 (X'X)^{-1}_{ii}$

> [!definition] Sampling Distribution of $\hat{\sigma}^2$
With $E = Y - X\hat{\beta}$:
>$$\frac{E'E}{\sigma^2} \sim \chi^2(n - p), \qquad E\big[\chi^2(n-p)\big] = n - p$$
So $\hat{\sigma}^2 = \dfrac{E'E}{n}$ is **biased**:
>$$E(\hat{\sigma}^2) = \frac{n-p}{n}\,\sigma^2$$

> [!theorem] Unbiased Estimator of $\sigma^2$
> $$\boxed{\text{RMS} = \frac{E'E}{n-p}}, \qquad E(\text{RMS}) = \sigma^2$$


> [!definition] Confidence Interval for a Single $\beta_i$
$$\hat{\beta}_i \sim \mathcal{N}\left(\beta_i,\ \sigma^2 (X'X)^{-1}_{i+1,i+1}\right)$$
> $$\hat{\beta}_i \pm t_{\alpha/2}(n-p)\,\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}$$
>(uses $t$, not $z$, since $\sigma^2$ is estimated)
>
**Standard error of $\hat{\beta}_i$:**
$$\text{SE}(\hat{\beta}_i) = \sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}$$


> [!definition] Test of a Single Coefficient
$$H_0: \beta_i = 0$$
>Reject $H_0$ at level $\alpha$ if:
$$\left|\frac{\hat{\beta}_i}{\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}}\right| > t_{\alpha/2}(n-p)$$
Significance codes by p-value:
>
>| p-value   | Code  |
| --------- | ----- |
| $< 0.05$  | `*`   |
| $< 0.01$  | `**`  |
| $< 0.001$ | `***` |
>
>
>
>
>



> [!definition] Geometric Picture
>- $\text{span}(X)$ = subspace of $\mathbb{R}^n$ spanned by columns of $X$
>- $\hat{Y}$ = projection of $Y$ onto $\text{span}(X)$
>- A restriction $\beta_i = 0$ reduces to a subspace $\Omega_0 \subseteq \Omega = \text{span}(X)$


> [!definition] Null Model
$$Y = \mathbf{1}\,\beta_0 + \varepsilon, \qquad \Omega_0 = \{\text{vectors with all components equal}\}$$
Global hypothesis:
$$H_0: \beta_1 = \beta_2 = \cdots = \beta_{p-1} = 0$$
(tested via the **global p-value** in standard output)

---

>[!definition] Nested Hypotheses (Subgroup of Predictors)
$$H_0: \text{a fixed subgroup of } \beta\text{'s} = 0 \qquad \text{e.g. } \beta_4 = \beta_5 = \beta_6 = 0$$
>
>Small model $\subseteq$ large model: $\Omega_0 \subseteq \Omega$.
>
**F-test for nested hypotheses:**
$$\frac{|\hat{Y} - \hat{Y}_{(0)}|^2}{(p - q)\,\hat{\sigma}^2} > F_\alpha(p-q,\; n-p)$$
>
>- $q$ = dimension of the small subspace
>- $p$ = dimension of the large subspace
>- In R: `anova(small, large)`

>[!definition] Additive vs Interaction Model
> - **Additive:** `lm(cons ~ quando + temp)`
$$E(\text{cons}_i) = \beta_0 + \beta_1 I_i + \beta_2 \text{temp}_i$$
>- **Interaction:** `lm(cons ~ quando * temp)`
$$E(\text{cons}_i) = \beta_0 + \beta_1 I_i + \beta_2 \text{temp}_i + \beta_3 (I_i \times \text{temp}_i)$$
>
>Testing interaction (`anova(small, large)`) $\equiv$ testing $H_0: \beta_3 = 0$.

### Various Linear Models

> [!definition] Model Taxonomy
> | Graph | Model |
> |---|---|
> | $\mathbf{1} \to Y$ | **Null model** |
> | $x \to Y$ | **Simple linear regression** |
> | $x \to Y,\ x^2 \to Y$ | **Polynomial linear regression** |
> | $b \to Y$ | **Two-sample normal problem** |
> | $x_1, x_2, z \to Y$ | **Multiple linear regression** (all predictors quantitative) |

> [!definition] Additive vs Interaction Model (Insulate)
> - **Additive model:** a linear model with no interaction term.
> - **Interaction model:** includes the interaction $bx$.
>
> The interaction $bx$ is a **nonlinear function of $x$**, but the model is **linear in the coefficients**.
>
> Matrix form (interaction):
> $$\begin{pmatrix} Y_1 \\ \vdots \\ Y_n \end{pmatrix} = \begin{pmatrix} 1 & 0 & x_1 & 0 \\ 1 & 0 & x_2 & 0 \\ \vdots & 1 & \vdots & 0 \\ 1 & 1 & x_n & x_n \end{pmatrix} \begin{pmatrix} \beta_0 \\ \beta_1 \\ \beta_2 \\ \beta_3 \end{pmatrix} + \begin{pmatrix} \varepsilon_1 \\ \vdots \\ \varepsilon_n \end{pmatrix}$$

> [!definition] Polynomial Regression & Transformations
> In polynomial regression the predictor $x$ is transformed before entering the model. Other transformations may be used:
> $$\log(x), \quad \sqrt{x}, \quad \ldots$$
> **Neural networks** are a development of these ideas.

> [!definition] Advantages of the Linear Model
> You pay the price of introducing probability distributions for $Y$, but you gain:
> - Interpretability
> - Uncertainty quantification
> - Confidence intervals
> - Tests
> - Prediction intervals
> - All other **Bayesian tools** (Part 2 of the course)

> [!definition] Qualitative Predictors (Factors)
> A qualitative variable with a number of levels $\geq 2$, called a **factor** when used as a predictor.
> - If 2 levels → **binary predictors** (also called _categories_ or _classes_)
>
> Examples: nationality, brand, eye color, A/C/T/G in genomics, etc.
>
> A factor may have an alphanumeric representation, but to enter a linear model it is encoded via **one-hot encoding** (a binary vector with length = number of levels).

> [!definition] One-Hot Encoding Example
> Observations: It, It, Fr, It, Fr, Ru, Ru, …
> $$X = \begin{pmatrix} \text{IT} & \text{FR} & \text{RU} & \cdots & \text{SS} \\ 1 & 0 & 0 & \cdots & 0 \\ 1 & 0 & 0 & \cdots & 0 \\ 0 & 1 & 0 & \cdots & 0 \\ \vdots & & & \ddots & \\ 0 & 0 & 1 & \cdots & 0 \\ \vdots & \vdots & \vdots & \vdots & \vdots \end{pmatrix}$$
> Use one-hot encoding to build the $X$ matrix → $Y = X\beta + \varepsilon$.

> [!definition] Special Case: 2 Levels
> If the number of levels $= 2$, one-hot encoding is already implied. E.g. Italian / Not Italian:
> $$\begin{pmatrix} Y_1 \\ \vdots \\ Y_n \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 1 & 0 \\ \vdots & \vdots \\ 1 & 1 \end{pmatrix} \begin{pmatrix} \beta_0 \\ \beta_1 \end{pmatrix} + \varepsilon$$
> _(top rows = "Not Italian", bottom rows = "Italian")_

> [!definition] One-Hot Encoding Rule for Factors
> For **one factor** with $I$ levels, we need:
> - the **intercept column** (1s)
> - **$I - 1$ binary columns** (one-hot encoding)
>
> $$a \to Y \qquad \Longleftrightarrow \qquad a \to \text{[one-hot encoding]} \to Y$$

> [!definition] Two-Factor Models
> Two factors can combine to give:
> - A **simple additive model** (_small model_) — no interaction
> - A **model with interactions** (_large model_)
>```r
anova(small,          large)
>#       ↑              ↑
># no interaction    model with
>#    model          interactions
>```
>
> If we knew the means $\mu_{ij}$, the mean $Y$ response corresponds to:
> - $i$-th level of $a$
> - $j$-th level of $b$

> [!definition] Two-Factor Mean Structure
> Let $\mu_{ij}$ = mean $Y$ when $a = i$ and $b = j$.
> - **Additive model → parallel lines:** the same vertical distance represents the difference across all brands.
> - **Interaction model → non-parallel lines:** the effect of Club may change depending on the level of Brand, so lines are no longer parallel.

> [!definition] Interaction
> A linear model is said to have **interactions** if the (true) **differential effects** of one predictor given a level of another predictor **depends on such level**.
> ![[7 - Confidence vs Prediction Intervals-1780320322620.webp]]

### Confidence and Prediction
> [!definition] Setup: Confidence vs Prediction
> Linear model: $$Y = X\beta + \varepsilon$$
> A new configuration of predictors $x_f$ (a new row, not in the training data) raises two related but separate questions:
> 1. **Estimate** $E(Y_{n+1})$, the expected $y$-value at $x_f$ → **Confidence Interval**
> 2. **Predict** $Y_{n+1}$, the actual new $y$-value at $x_f$ → **Prediction Interval**
>
> Assume $Y_{n+1}$ is sampled independently of $Y_1, \ldots, Y_n$.

> [!definition] New Fitted Value
> $$E(Y_{n+1}) = x_f \beta, \qquad \hat{Y}_{n+1} = x_f \hat{\beta} \quad \text{(the "new" fitted value)}$$
> $$E(\hat{Y}_{n+1}) = x_f \beta$$
> $$\text{Var}(\hat{Y}_{n+1}) = x_f \,\sigma^2 (X'X)^{-1} x_f'$$
> Estimated variance (using $\hat{\sigma}^2 = \dfrac{E'E}{n-p}$, the RMS):
> $$\widehat{\text{Var}}(\hat{Y}_{n+1}) = x_f \,\hat{\sigma}^2 (X'X)^{-1} x_f'$$

> [!definition] Confidence Interval for $E(Y_{n+1})$
> $$\frac{x_f\hat{\beta} - x_f\beta}{\sqrt{\hat{\sigma}^2\, x_f(X'X)^{-1}x_f'}} \sim t(n-p)$$
> $(1-\alpha)$-level confidence interval:
> $$x_f\hat{\beta} \;\pm\; t_{\alpha/2}(n-p)\sqrt{\frac{E'E}{n-p}\, x_f(X'X)^{-1}x_f'}$$
> Example (simple linear regression): $x_f\hat{\beta} = \hat{\beta}_0 + \hat{\beta}_1 x_f$.

> [!definition] Prediction of a New $Y_{n+1}$
> $$Y_{n+1} = x_f\beta + \varepsilon_{n+1}, \qquad Y_{n+1} \sim \mathcal{N}(x_f\beta,\ \sigma^2)$$
> Prediction (same point estimate as the mean):
> $$\hat{Y}_{n+1} = x_f\hat{\beta}, \qquad \hat{Y}_{n+1} \sim \mathcal{N}\big(x_f\beta,\ x_f\sigma^2(X'X)^{-1}x_f'\big)$$

> [!definition] Prediction Error
> Difference of two independent normals:
> $$Y_{n+1} - \hat{Y}_{n+1} \sim \mathcal{N}\big(0,\ \text{Var}(Y_{n+1}) + \text{Var}(\hat{Y}_{n+1})\big)$$
> $$= \mathcal{N}\big(0,\ \sigma^2\big(1 + x_f(X'X)^{-1}x_f'\big)\big)$$
> $$\frac{Y_{n+1} - \hat{Y}_{n+1}}{\sqrt{\hat{\sigma}^2\big(1 + x_f(X'X)^{-1}x_f'\big)}} \sim t(n-p)$$

> [!definition] Prediction Interval for $Y_{n+1}$
> $(1-\alpha)$-level prediction interval:
> $$x_f\hat{\beta} \;\pm\; t_{\alpha/2}(n-p)\sqrt{\frac{E'E}{n-p}\big(1 + x_f(X'X)^{-1}x_f'\big)}$$

> [!important] Key Difference
> The prediction interval contains the extra $+1$ term inside the root. Uncertainty for a single value $Y_{n+1}$ is **higher** than uncertainty for its mean $E(Y_{n+1})$.

### Logistic Regression
> [!definition] Motivation for GLMs
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

If we try $E(Y) = X\beta$ on Bernoulli it does not work:
$$E(Y) = p = X\beta \quad \text{(e.g. } \beta_0 + \beta_1 x \text{ with a single quantitative predictor } x\text{)}$$
![[8 - Logistic Regression-1780325171230.webp]]
So the idea is to use a **link function** $g(\cdot)$ and write:
> [!definition] Generalized Linear Model
> $$g(E(Y)) = X\beta$$
>
> The linear combination ($X$) of predictors is **kept**.

> [!definition] Logit Link Function
> In the binary case: $$g(p) = X\beta$$

> [!definition] Odds and Log-Odds
> Formally, the **odds** is a function $o(p)$:
> $$o(p) = \frac{p}{1-p}$$
![[8 - Logistic Regression-1780325307850.webp]]
>
$o(p)$ ranges from $0$ to $+\infty$.
To obtain a $g(\cdot)$ ranging from $-\infty$ to $+\infty$, we take the logarithm:
$$\log o(p) = \log \frac{p}{1-p} = \log p - \log(1-p) = \text{logodds}(p)$$
![[8 - Logistic Regression-1780325388338.webp]]

> [!definition] Logistic Regression
> Now we can write:
$$\text{logodds}(p) = X\beta$$
and see how predictors affect the mean response $p$ through the link function:
$$g(p) = \text{logodds}(p) = \text{logit}(p) = \log \frac{p}{1-p}$$
> A **Generalized Linear Model** for binary (Bernoulli) $Y$ and link function **logit** is called **logistic regression**.

> [!definition] Inverse Transformation: The Logistic Function
> $$\boxed{p = \frac{e^\ell}{1 + e^\ell}} \qquad \text{the logistic function}$$
>
![[8 - Logistic Regression-1780325497174.webp]]

> [!definition] Estimation of $\beta$ via Maximum Likelihood
> $$L = \prod_{i=1}^{n} \left(\frac{e^{\ell_i}}{1+e^{\ell_i}}\right)^{y_i} \left(1 - \frac{e^{\ell_i}}{1+e^{\ell_i}}\right)^{1-y_i}$$
> - $L$ — the likelihood function, the quantity to be maximized over β
>- $n$ — the number of observations (sample size)
>- $yᵢ$ — the binary outcome (0 or 1) for observation i
>- $ℓᵢ$ — the linear predictor (log-odds) for observation i, typically ℓᵢ = xᵢᵀβ
>- $β$ — the vector of regression coefficients being estimated
>- $xᵢ$ — the vector of predictor/covariate values for observation i (implicit within ℓᵢ)
>- $\frac{e^{ℓᵢ}}{1+e^{ℓᵢ}}$ — the modeled probability that yᵢ = 1 (the logistic/sigmoid function)

Maximizing the likelihood does **not** have an explicit analytical solution, but we can use **numerical methods** to obtain $\hat{\beta}$.
> [!theorem] Asymptotic Sampling Distribution of $\hat{\beta}$ (GLM)
> $$\hat{\beta} \sim \mathcal{N}_p (\beta, VarCov(\hat \beta))$$
> where:
> - $\mathcal{N}_p$ is the **normal approximation** to the sampling distribution of $\hat{\beta}$
> - $\hat \beta$ are approximately unbiased
> - $VarCov(\hat \beta)$ is estimated based on data

> [!definition] Confusion Matrix & Performance Metrics
The classification gives rise to the usual **confusion matrix**:
> 
>
>
>
>
| Y \ $\hat Y$ | 0   | 1   |
| ------------ | --- | --- |
| 0            | TN  | FP  |
| 1            | FN  | TP  |
**Sensitivity** (true positive rate):
$$\text{sensitivity} = P(\hat{Y}=1 \mid Y=1) \approx \frac{TP}{TP + FN}$$
**Specificity** (true negative rate):
$$\text{specificity} = P(\hat{Y}=0 \mid Y=0) \approx \frac{TN}{TN + FP}$$
We can **vary the threshold** (instead of 0.5, choose any number $\in (0,1)$) and obtain different sensitivities and specificities.

> [!definition] ROC Curve
> A plot of **sensitivity vs (1 − specificity)** is called the **ROC curve**.
> ![[8 - Logistic Regression-1780326617356.webp]]
> The ideal/perfect ROC curve goes straight up to $(0,1)$ then across — the real curve lies between the diagonal (random classifier) and this ideal.

### Classification

> [!definition] Fisher's Discriminant Analysis Setup
> Two normal distributions, one for the $+$ population and one for the $-$ population:
>
> $$X_+ \sim \mathcal{N}(\mu_+, \Sigma_+) \quad \text{features from the + population}$$
>
> $$X_- \sim \mathcal{N}(\mu_-, \Sigma_-) \quad \text{features from the - population}$$
> - $x$: Feature vector of the sample to be classified.
>- $\mu$: Mean vector (average feature values) of a class.
>- $\Sigma$: Covariance matrix describing the variance and correlation of the features within a class.
>
> **Fisher's idea:** look at the likelihood ratio $\dfrac{f_+(x)}{f_-(x)}$ and assign to $+$ population if the likelihood ratio is greater than a threshold (e.g. $\frac{1}{2}$).


> [!definition] General Classification Rule - Quadratic Discriminant Analysis
> $$\boxed{(x-\mu_+)'\Sigma_+^{-1}(x-\mu_+) - (x-\mu_-)'\Sigma_-^{-1}(x-\mu_-) < \text{threshold}} \tag{†}$$
> Expanded:
>  $$x'\Sigma_+^{-1}x - 2\mu_+'\Sigma_+^{-1}x + \mu_+'\Sigma_+^{-1}\mu_+ - x'\Sigma_-^{-1}x + 2\mu_-'\Sigma_-^{-1}x - \mu_-'\Sigma_-^{-1}\mu_- < t$$


> [!theorem] Fisher's Linear Discrimination Rule (LDA)
> **Homoscedasticity**: the two groups have the same covariance matrix ($\Sigma_+ = \Sigma_- = \Sigma$), we can simplify the formula:
> $$\boxed{-2(\mu_+' - \mu_-')\Sigma^{-1}x < t}$$

> [!definition] K-Nearest Neighbors
> KNN is a totally different approach that does **not** rely on distributional assumptions.
>
> - $K$ = a fixed number
> - $N$ = nearest
> - $N$ = neighbors
![[9 - Classification - LDA, QDA, KNN-1780519086527.webp]]

> [!definition] Generalizing Logistic Regression to More Than Two Classes
> 
$$P(k\text{-class}),\ k = 1, 2, \ldots, K-1 \quad = \quad \frac{e^{\beta_{k0} + \beta_{k1}x_1 + \cdots + \beta_{kp}x_p}}{1 + \displaystyle\sum_{\ell=1}^{K-1} e^{\beta_{\ell 0} + \beta_{\ell 1}x_1 + \cdots + \beta_{\ell p}x_p}}$$
$$P(\text{last class } K,\ \text{taken as reference}) \quad = \quad \frac{1}{1 + \displaystyle\sum_{\ell=1}^{K-1} e^{\beta_{\ell 0} + \beta_{\ell 1}x_1 + \cdots + \beta_{\ell p}x_p}}$$
