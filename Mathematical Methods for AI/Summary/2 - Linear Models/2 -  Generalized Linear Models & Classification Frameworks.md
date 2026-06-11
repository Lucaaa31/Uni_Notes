### Theoretical Shift to GLMs

> **Context:** Classical linear regression fails when the response variable $Y$ is bounded or discrete (such as binary choices or counts), as a straight line $X\beta$ would eventually predict probabilities outside the valid $[0,1]$ range. To handle non-normal distributions while preserving a linear predictor structure, we introduce a monotonic link function $g(\cdot)$ that maps the expected value of the response to the real line.

> [!definition] Generalized Linear Model (GLM)
> 
> Instead of modeling the mean response directly as $E(Y) = X\beta$, we apply a link function $g(\cdot)$:
> 
> $$\boxed{g(E(Y)) = X\beta}$$
> 
> - The linear combination of predictors ($X\beta$) is preserved.
>     
> - It allows the distribution of $Y$ to belong to the Exponential Family (Bernoulli, Poisson, Gamma, etc.).
>     

### The Binary Mapping Solution

> **Context:** In binary classification where $Y \sim \text{Bernoulli}(p)$, the mean response is the probability $p = E(Y)$. To map $p \in (0,1)$ to a continuous scale spanning from $-\infty$ to $+\infty$, we transform the probability into odds, and subsequently into log-odds (the logit transformation).

> [!definition] Odds and Log-Odds
> 
> The **odds** $o(p)$ represents the ratio of the probability of success to the probability of failure, ranging from $0$ to $+\infty$:
> 
> $$o(p) = \frac{p}{1-p}$$
> 
> Taking the natural logarithm yields the **log-odds** (or logit function), which ranges from $-\infty$ to $+\infty$:
> 
> $$\text{logodds}(p) = \text{logit}(p) = \log \frac{p}{1-p} = \log(p) - \log(1-p)$$

### Logistic Modeling and Estimation

> **Context:** Combining the logit link function with our linear predictor gives us the Logistic Regression model. Because the parameters $\beta$ enter the Bernoulli probability non-linearly, we cannot use analytical OLS equations; instead, we maximize the likelihood function using iterative numerical methods.

> [!definition] Logistic Regression & Maximum Likelihood Estimation
> 
> A GLM for binary data utilizing a logit link function is defined as:
> 
> $$\text{logit}(p) = X\beta$$
> 
> The inverse transformation yields the **Logistic (Sigmoid) Function**, mapping the linear predictor back to a probability:
> 
> $$\boxed{p = \frac{e^{X\beta}}{1 + e^{X\beta}}}$$
> 
> To estimate $\beta$, we maximize the **Likelihood Function** $L$:
> 
> $$L(\beta) = \prod_{i=1}^{n} \left(\frac{e^{x_i'\beta}}{1+e^{x_i'\beta}}\right)^{y_i} \left(1 - \frac{e^{x_i'\beta}}{1+e^{x_i'\beta}}\right)^{1-y_i}$$
> 
> - $L$ — the likelihood function, the quantity to be maximized over β
>- $n$ — the number of observations (sample size)
>- $yᵢ$ — the binary outcome (0 or 1) for observation i
>- $ℓᵢ$ — the linear predictor (log-odds) for observation i, typically ℓᵢ = xᵢᵀβ
>- $β$ — the vector of regression coefficients being estimated
>- $xᵢ$ — the vector of predictor/covariate values for observation i (implicit within ℓᵢ)
>- $\frac{e^{ℓᵢ}}{1+e^{ℓᵢ}}$ — the modeled probability that yᵢ = 1 (the logistic/sigmoid function)

### Large-Sample Inference

> **Context:** Unlike the exact finite-sample Student's t-distributions used in classical OLS regression, hypothesis testing and confidence intervals for GLM parameters rely on asymptotic (large-sample) maximum likelihood theory.
	
> [!definition] Asymptotic Sampling Distribution of $\hat{\beta}$
> 
> For a sufficiently large sample size $n$, the estimator $\hat{\beta}$ converges in distribution to a multivariate normal distribution:
> 
> $$\hat{\beta} \overset{a}{\sim} \mathcal{N}_p \big(\beta,\; \mathrm{VarCov}(\hat{\beta})\big)$$
> 
> - The estimators are asymptotically unbiased: $E(\hat{\beta}) \approx \beta$.
>     
> - $\mathrm{VarCov}(\hat{\beta})$ is estimated using the inverse of the observed Information Matrix based on the data.
>     

### Classifier Evaluation Metrics

> **Context:** Once a logistic model outputs probabilities, a threshold (typically 0.5) is applied to classify observations into discrete groups. We evaluate the performance of this binary mapping using a cross-tabulation of true vs. predicted states, known as a confusion matrix.

> [!definition] Confusion Matrix & Performance Metrics
> 
> The cross-tabulation of observed values ($Y$) and predicted classes ($\hat{Y}$) yields:
>
>|                  | **$\hat{Y} = 0$** | **$\hat{Y} = 1$** |
| :--------------- | :---------------: | :---------------: |
| **$Y = 0$** <br> |      **TN**       |     *FP* <br>     |
| **$Y = 1$** <br> |     *FN* <br>     |      **TP**       |
>
>
>
> - **Sensitivity** (True Positive Rate / Recall): The probability of correctly identifying a true positive.
>     
>     $$\text{sensitivity} = P(\hat{Y}=1 \mid Y=1) \approx \frac{TP}{TP + FN}$$
>     
> - **Specificity** (True Negative Rate): The probability of correctly identifying a true negative.
>     
>     $$\text{specificity} = P(\hat{Y}=0 \mid Y=0) \approx \frac{TN}{TN + FP}$$
>     

### Threshold Calibration Analysis

> **Context:** Altering the classification cutoff away from 0.5 creates a trade-off between sensitivity and specificity. Mapping this full dynamic range allows us to visualize the discriminatory power of a model regardless of the chosen threshold.

> [!definition] Receiver Operating Characteristic (ROC) Curve
> 
> A graphical plot displaying **sensitivity** on the y-axis versus **$1 - \text{specificity}$** (False Positive Rate) on the x-axis across all possible classification thresholds.
> 
> - **Ideal Classifier:** Approaches the top-left coordinate $(0,1)$ instantly, yields an Area Under the Curve $\text{AUC} = 1$.
>     
> - **Random Classifier:** Follows the 45-degree diagonal baseline, yielding an $\text{AUC} = 0.5$.
>     

### Generative Parametric Classification

> **Context:** Rather than directly modeling the conditional probability $P(Y \mid X)$ like logistic regression (a discriminative approach), generative classification models the distribution of features within each class $f(x \mid Y)$ using multivariate normal distributions, assigning observations using likelihood ratios.

> [!definition] Quadratic Discriminant Analysis (QDA)
> 
> Assuming features within the positive ($+$) and negative ($-$) classes follow distinct multivariate normal distributions:
> 
> $$X_+ \sim \mathcal{N}(\mu_+, \Sigma_+) \qquad X_- \sim \mathcal{N}(\mu_-, \Sigma_-)$$
> 
> By evaluating the likelihood ratio $\frac{f_+(x)}{f_-(x)}$, an observation is assigned to the positive class if the quadratic inequality holds true:
> 
> $$\boxed{(x-\mu_+)'\Sigma_+^{-1}(x-\mu_+) - (x-\mu_-)'\Sigma_-^{-1}(x-\mu_-) < \text{threshold}}$$

### Simplifying Classification Boundarie

> **Context:** If we introduce a homoscedasticity assumption—forcing all classes to share an identical covariance matrix—the quadratic terms in the QDA decision rule cancel out, yielding a simpler, linear classification boundary.

> [!definition] Fisher's Linear Discriminant Analysis (LDA)
> 
> Under the assumption of equal covariance matrices across groups ($\Sigma_+ = \Sigma_- = \Sigma$), the quadratic classification boundary simplifies into a linear function of $x$:
> 
> $$\boxed{-2(\mu_+' - \mu_-')\Sigma^{-1}x < t}$$

### Non-Parametric Classifiers

> **Context:** When the underlying probability distributions of the features cannot be safely assumed, we shift to non-parametric memory-based algorithms that classify points based on local spatial proximity rather than global equations.

> [!definition] K-Nearest Neighbors (KNN)
> 
> A non-parametric instance-based learning algorithm that bypasses distributional assumptions entirely.
> 
> To classify a new feature vector $x$:
> 
> 1. Compute the distance between $x$ and all training samples.
>     
> 2. Identify the $K$ closest samples (neighbors).
>     
> 3. Assign $x$ to the majority class among those $K$ neighbors via a plurality vote.
>     

### Polytomous Target Extensions

> **Context:** When the target variable contains more than two unordered categories ($K > 2$), we expand binary logistic regression into Multinomial Logistic Regression by electing a reference class and fitting simultaneous log-odds equations against it.

> [!definition] Multinomial Logistic Regression
> 
> For a target variable with $K$ nominal levels, choosing class $K$ as the baseline reference yields $K-1$ simultaneous equations:
> 
> - **For any non-reference class $k \in \{1, \ldots, K-1\}$:**
>     
>     $$P(Y_i = k) = \frac{e^{x_i'\beta_k}}{1 + \sum_{\ell=1}^{K-1} e^{x_i'\beta_\ell}}$$
>     
> - **For the baseline reference class $K$:**
>     
>     $$P(Y_i = K) = \frac{1}{1 + \sum_{\ell=1}^{K-1} e^{x_i'\beta_\ell}}$$
>