### Linear Model Framework

> **Context:** Before estimating any parameters, we must formally define the structural layout of the multivariate linear regression model using matrix notation. This maps our observed data to a system of linear equations that includes a stochastic error component.

> [!definition] Linear Model (Matrix Form)
> 
> $$\underset{n \times 1}{Y} = \underset{n \times p}{X} \quad \underset{p \times 1}{\beta} + \underset{n \times 1}{\varepsilon}$$
> 
> - $\beta_{p \times 1}$ = p-vector of **unknown coefficients** containing the effects of the predictors
>     
> - $Y_{n \times 1}$ = vector of responses, random variables
>     
> - $X_{n \times p}$ = known matrix of input values of predictors, often under the control of the experimenter
>     
> - $\varepsilon_{n \times 1}$ = vector of random variables representing **errors**
>     

### Error Assumptions

> **Context:** To perform statistical inference (such as hypothesis testing and confidence intervals), we need to specify the probabilistic behavior of the unobserved errors. We assume they follow a multivariate normal distribution with a constant variance, which propagates directly to the response variable $Y$.

> [!definition] Normal Errors
> 
> $$\underset{n \times 1}{\varepsilon} = \varepsilon_1, \ldots, \varepsilon_n$$
> 
> i.i.d. $\mathcal{N}(0, \sigma^2)$, so that:
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

### Parameter Estimation

> **Context:** Since this is a parametric framework with unknown parameters $\beta$ and $\sigma^2$, we employ the Maximum Likelihood Estimation (MLE) principle. Under normality, maximizing the likelihood function is mathematically equivalent to minimizing the sum of squared distances, leading us to the Ordinary Least Squares (OLS) solutions.

> [!definition] Maximum Likelihood Estimate
> 
> The likelihood is the density of the observations $Y_1, \ldots, Y_n$ viewed as a function of the unknown parameters.
> 
> Under the assumption of normally distributed errors, maximizing this likelihood is equivalent to minimizing the sum of squared residuals.
> 
> We get the **Ordinary Least Squares (OLS)** normal equations:
> 
> $$\boxed{X'X\beta = X'y}$$
> 
> That is a classic Least Square Problem.
> 
> We can solve $\beta$ by assuming that $X'X$ is invertible (i.e. the $p$ columns of $X$ are linearly independent):
> 
> $$\boxed{\hat{\beta} = (X'X)^{-1}X'y} \quad \leftarrow \text{least square estimate of } \beta$$
> 
> Once we have maximized the likelihood in $\beta$, we have to find $\sigma^2$ and obtain:
> 
> $$\hat{\sigma}^2 = \frac{(y - X\hat{\beta})'(y - X\hat{\beta})}{n} = \frac{\sum\left(y_i - \sum_{p=0}^{j-1} x_{i,j}\hat{\beta}_p\right)^2}{n} = \frac{1}{n}$$
> 
> That is the sum of squared differences between observed and fitted values.

### Model Diagnoses

> **Context:** After computing our optimal beta coefficients, we can break down our observed vector $y$ into a systematic part (the predictions captured by the model) and a random part (the remaining unexplained variations).

> [!definition] Residuals and Fitted Values
> 
> - $\hat{\beta}$ — estimates of $\beta$
>     
> - $\hat{y} = X\hat{\beta}$ — fitted $y$ values
>     
> - $y$ — observed $y$ values
>     
> - $e = (y - X\hat{\beta})$ — vector of residuals
>     
> - $e'e = (y - X\hat{\beta})'(y - X\hat{\beta})$ — **residual sum of squares**
>     

### Baseline Reference

> **Context:** To understand the worst-case scenario where our predictors have absolutely no explanatory power, we look at the baseline "Null Model". This represents a benchmark model that only estimates a single common mean for all data points.

> [!definition] Null Model ($X$ = column of 1)
> 
> $$\hat{\beta}_0 = \bar{y}, \qquad \hat{\sigma}^2 = \frac{\sum(y_i - \bar{y})^2}{n}$$
> 
> Unbiased estimation:
> 
> $$s^2 = \dfrac{\sum(y_i - \bar{y})^2}{n-1}$$

### Simplest Case Analysis

> **Context:** Moving one step past the null model, we analyze the classic bivariate case. This simple linear regression setup helps visualize how adding just one continuous covariate affects the response variable and impacts the degrees of freedom for the variance estimator.

> [!definition] Linear Regression ($Y_i = \beta_0 + \beta_1 x_i + \varepsilon_i$)
> 
> $$\begin{cases}\hat{\beta}_1 = \frac{\sum(x_i - \bar{x})(y_i - \bar{y})}{\sum(x_i - \bar{x})^2}, \\ \hat{\beta}_0 = \bar{y} - \hat{\beta}_1\bar{x} \end{cases}$$The line goes through $(\bar{x}, \bar{y})$. Estimation of $\sigma^2$:$$\text{RMS} = \frac{\sum\big(y_i - (\hat{\beta}_0 + \hat{\beta}_1 x_i)\big)^2}{n-2}$$

### Frequentist Distinction

> **Context:** Before deriving the sampling distributions, it is essential to distinguish between the static numerical vectors obtained from a fixed dataset and the theoretical random variables governed by a sampling distribution.

> [!definition] Estimate vs Estimator
> 
> - **Estimate:** $\hat{\beta} = (X'X)^{-1}X'y$, it is a quantity calculated by the algorithm for a certain value of $y$
>     
> - **Estimator:** $\hat{\beta} = (X'X)^{-1}X'Y$, it is the corresponding Random Variable
>     

### Properties of Coefficients

> **Context:** Because the estimator is a linear function of the normally distributed variable $Y$, we can derive its exact finite-sample distribution to prove its unbiasedness and establish its standard errors.

> [!definition] Sampling Distribution of $\hat{\beta}$
> 
> $$\hat{\beta} \sim \mathcal{N}_p\big(\beta,\; \sigma^2 (X'X)^{-1}\big)$$
> 
> - **Unbiasedness:** $E(\hat{\beta}) = \beta$
>     
> - **Variance-covariance matrix:** $\text{VarCov}(\hat{\beta}) = \sigma^2 (X'X)^{-1}$
>     
> - **Variance of a single coefficient:** $\text{Var}(\hat{\beta}_i) = \sigma^2 (X'X)^{-1}_{ii}$
>     

### Properties of Error Variance

> **Context:** Evaluating the ML estimator for the error variance reveals a systematic downward bias because it fails to account for the degrees of freedom lost during the estimation of the $\beta$ vector.

> [!definition] Sampling Distribution of $\hat{\sigma}^2$
> 
> With $E = Y - X\hat{\beta}$:
> 
> $$\frac{E'E}{\sigma^2} \sim \chi^2(n - p), \qquad E\big[\chi^2(n-p)\big] = n - p$$
> 
> So $\hat{\sigma}^2 = \dfrac{E'E}{n}$ is **biased**:
> 
> $$E(\hat{\sigma}^2) = \frac{n-p}{n}\,\sigma^2$$

### Unbiased Variance Corrections

> **Context:** To fix this systematic bias, we scale the residual sum of squares by the proper degrees of freedom ($n-p$), giving us a statistically reliable estimator known as the Residual Mean Square (RMS).

> [!theorem] Unbiased Estimator of $\sigma^2$
> 
> $$\boxed{\text{RMS} = \frac{E'E}{n-p}}, \qquad E(\text{RMS}) = \sigma^2$$

### Quantifying Uncertainty

> **Context:** Since the population parameter $\sigma^2$ is unknown, we must plug in our unbiased sample estimate (RMS). This substitution changes our target distribution from a standard Normal to a Student's t-distribution, which we use to construct confidence intervals around individual beta coefficients.

> [!definition] Confidence Interval for a Single $\beta_i$
> 
> $$\hat{\beta}_i \sim \mathcal{N}\left(\beta_i,\ \sigma^2 (X'X)^{-1}_{i+1,i+1}\right)$$
> 
> $$\hat{\beta}_i \pm t_{\alpha/2}(n-p)\,\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}$$
> 
> _(uses $t$, not $z$, since $\sigma^2$ is estimated)_
> 
> **Standard error of $\hat{\beta}_i$:**
> 
> $$\text{SE}(\hat{\beta}_i) = \sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}$$

### Single Hypothesis Testing

> **Context:** We can leverage the Student's t-distribution to test whether an individual predictor holds any linear relationship with the response variable, allowing us to drop irrelevant variables based on standardized p-value significance thresholds.

> [!definition] Test of a Single Coefficient
> 
> $$H_0: \beta_i = 0$$
> 
> Reject $H_0$ at level $\alpha$ if:
> 
> $$\left|\frac{\hat{\beta}_i}{\sqrt{\hat{\sigma}^2(X'X)^{-1}_{i+1,i+1}}}\right| > t_{\alpha/2}(n-p)$$
> 
> Significance codes by p-value:
> 
> |**p-value**|**Code**|
> |---|---|
> |$< 0.05$|`*`|
> |$< 0.01$|`**`|
> |$< 0.001$|`***`|

### Geometric Projection

> **Context:** Fundamentally, Ordinary Least Squares can be interpreted through linear algebra as an orthogonal projection of our multi-dimensional response vector onto a lower-dimensional subspace spanned by the column vectors of our design matrix.

> [!definition] Geometric Picture
> 
> - $\text{span}(X)$ = subspace of $\mathbb{R}^n$ spanned by columns of $X$
>     
> - $\hat{Y}$ = projection of $Y$ onto $\text{span}(X)$
>     
> - A restriction $\beta_i = 0$ reduces to a subspace $\Omega_0 \subseteq \Omega = \text{span}(X)$
>     

### Comparing Subspaces

> **Context:** When evaluating multiple parameters simultaneously, we can compare the length of the projection vectors between a larger unrestricted subspace and a smaller restricted model via a partial F-test.

> [!definition] Nested Hypotheses (Subgroup of Predictors)
> 
> $H_0: \text{a fixed subgroup of } \beta\text{'s} = 0 \qquad \text{e.g. } \beta_4 = \beta_5 = \beta_6 = 0$
> 
> Small model $\subseteq$ large model: $\Omega_0 \subseteq \Omega$.
> 
> F-test for nested hypotheses:
> 
> $$\frac{\|\hat{Y} - \hat{Y}_{(0)}\|^2 / (p - q)}{\hat{\sigma}^2} > F_\alpha(p-q,\; n-p)$$
> 
> - $q$ = dimension of the small subspace
>     
> - $p$ = dimension of the large subspace
>     
> 
> In R: `anova(small, large)`

### Model Specification and Interaction

> **Context:** In practice, nested tests are heavily used to check for structural interaction terms, verifying whether the combined effect of two variables acts strictly as a sum of individual contributions or changes conditionally depending on their interaction.

> [!definition] Additive vs Interaction Model
> 
> **Additive:** `lm(cons ~ quando + temp)`
> 
> $$E(\text{cons}_i) = \beta_0 + \beta_1 I_i + \beta_2 \text{temp}_i$$
> 
> **Interaction:** `lm(cons ~ quando * temp)`
> 
> $$E(\text{cons}_i) = \beta_0 + \beta_1 I_i + \beta_2 \text{temp}_i + \beta_3 (I_i \times \text{temp}_i)$$
> 
> Testing interaction (`anova(small, large)`) $\equiv$ testing $H_0: \beta_3 = 0$.

### Model Classification

> **Context:** To structure our analytical approach, we classify linear frameworks into a taxonomy based on how the predictors map to the response variable $Y$. This highlights that even non-linear profiles (like polynomials) remain strictly linear in their parameters.

> [!definition] Model Taxonomy
>
>| **Graph Model**        | **Statistical Translation**                              |
| ---------------------- | -------------------------------------------------------- |
| $\mathbf{1} \to Y$     | Null model                                               |
| $x \to Y$              | Simple linear regression                                 |
| $x \to Y, \ x^2 \to Y$ | Polynomial linear regression                             |
| $b \to Y$              | Two-sample normal problem                                |
| $x_1, x_2, z \to Y$    | Multiple linear regression (all predictors quantitative) |
>
>
>
>
>
>
>
>
>
>

### Categorical Encoding

> **Context:** Linear algebra requires continuous numerical inputs. When dealing with qualitative features (factors), we must establish a mathematical encoding system—known as One-Hot Encoding or dummy coding—to integrate distinct categories safely into the design matrix $X$.

> [!definition] Qualitative Predictors (Factors)
> 
> A qualitative variable with a number of levels $\geq 2$, called a factor when used as a predictor.
> 
> - If 2 levels $\to$ binary predictors (also called categories or classes)
>     
> - Examples: nationality, brand, eye color, A/C/T/G in genomics, etc.
>     
> 
> A factor may have an alphanumeric representation, but to enter a linear model it is encoded via one-hot encoding (a binary vector with length = number of levels).
> 
> **One-Hot Encoding Rule for Factors:**
> 
> For one factor with $I$ levels, we need:
> 
> 1. the intercept column (1s)
>     
> 2. $I - 1$ binary columns (one-hot encoding)
>     
>     $$\text{a} \to Y \qquad \Longleftrightarrow \qquad \text{a} \to \text{[one-hot encoding]} \to Y$$
>     

### Multi-Factor Interactivity

> **Context:** When evaluating models containing multiple factors, we track the mean response structure ($\mu_{ij}$) across combinations of groups. This setup allows us to test whether group effects behave independently (parallel response curves) or interact conditionally.

> [!definition] Two-Factor Mean Structure
> 
> Let $\mu_{ij} = \text{mean } Y \text{ when } a = i \text{ and } b = j$.
> 
> - **Additive model** $\to$ parallel lines: the same vertical distance represents the difference across all brands.
>     
> - **Interaction model** $\to$ non-parallel lines: the effect of Club may change depending on the level of Brand, so lines are no longer parallel.
>     
> 
> **Interaction:** A linear model is said to have interactions if the (true) differential effects of one predictor given a level of another predictor depends on such level.

### Future Values Evaluation

> **Context:** Once the model is fitted, we can pass a completely new configuration of predictors, denoted as a row vector $x_f$. This leaves us with two distinct goals: assessing the average structural behavior at that point, or predicting where a single future observation will land.

> [!definition] Confidence Interval for the Expected Response $E(Y_{n+1})$
> 
> $$\frac{x_f\hat{\beta} - x_f\beta}{\sqrt{\hat{\sigma}^2\, x_f(X'X)^{-1}x_f'}} \sim t(n-p)$$
> 
> $(1-\alpha)$-level confidence interval:
> 
> $$x_f\hat{\beta} \;\pm\; t_{\alpha/2}(n-p)\sqrt{\frac{E'E}{n-p}\, x_f(X'X)^{-1}x_f'}$$
> 
> _Example (simple linear regression): $x_f\hat{\beta} = \hat{\beta}_0 + \hat{\beta}_1 x_f$._

### Individual Out-of-Sample Prediction

> **Context:** Predicting an actual individual observation $Y_{n+1}$ introduces an inescapable layer of real-world noise ($\varepsilon_{n+1}$). Because we must account for both our model's structural estimation error and this raw random variance, prediction intervals are inherently wider.

> [!definition] Prediction Interval for a New $Y_{n+1}$
> 
> $$Y_{n+1} - \hat{\beta} \sim \mathcal{N}\big(0,\ \sigma^2\big(1 + x_f(X'X)^{-1}x_f'\big)\big)$$
> 
> $$\frac{Y_{n+1} - \hat{Y}_{n+1}}{\sqrt{\hat{\sigma}^2\big(1 + x_f(X'X)^{-1}x_f'\big)}} \sim t(n-p)$$
> 
> $(1-\alpha)$-level prediction interval:
> 
> $$x_f\hat{\beta} \;\pm\; t_{\alpha/2}(n-p)\sqrt{\frac{E'E}{n-p}\big(1 + x_f(X'X)^{-1}x_f'\big)}$$
> 
> **Key Difference:** The prediction interval contains the extra $+1$ term inside the square root. Uncertainty for a single value $Y_{n+1}$ is higher than uncertainty for its mean $E(Y_{n+1})$.

