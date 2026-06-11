### Mathematical Objective

> **Context:** The core objective of statistical learning is to analyze multivariate data structures to accurately predict future outcomes or uncover latent patterns, while formally quantifying the underlying structural uncertainty.

### Core Predictive Framework

> **Context:** Machine learning formalizes predictive tasks by constructing a mapping function that projects an input feature space onto a target response variable space, isolating the systematic relationship from environmental noise.

> [!definition] The Prediction Operator
> 
> Given an input feature vector $\mathbf{X} \in \mathbb{R}^d$ and an output response variable $Y$, we define a prediction function $g: \mathbb{R}^d \to \mathcal{Y}$ that maps inputs to a predicted estimate $\hat{y} = g(\mathbf{X})$.
>
>| **Task Type**      | **Response Domain Y**                                                 | **Core Operational Objective**                             |
| ------------------ | --------------------------------------------------------------------- | ---------------------------------------------------------- |
| **Regression**     | Continuous real values ($\mathcal{Y} \subseteq \mathbb{R}$)           | Predict a continuous mathematical quantity.                |
| **Classification** | Finite discrete categorical set ($\mathcal{Y} \in \{0, \dots, c-1\}$) | Assign the input vector to one of $c$ distinct categories. |
>

### Loss Function Mechanics

> **Context:** To optimize the **prediction function** $g$, we require a mathematical metric to evaluate the discrepancy between the true unobserved target $Y$ and the model's estimate $\hat{y}$.

> [!definition] Loss and Error Metrics
> 
> A loss function $\mathrm{Loss}(y, \widehat{y})$ quantifies the immediate penalty incurred by a discrepancy between the true value $y$ and the estimate $\hat{y}$.
> 
> - **Squared-Error Loss (Regression):**
>     
>     $$\mathrm{Loss}(y,\widehat{y}) = (y - \widehat{y})^2$$
>     
> - **Zero–One (0–1) Loss (Classification):**
>     
>     $$\mathrm{Loss}(y,\widehat{y}) = \mathbb{I}_{\{y \neq \widehat{y}\}} = \begin{cases} 1 & \text{if } \widehat{y} \neq y \\ 0 & \text{if } \widehat{y} = y \end{cases}$$
>     

### Statistical Risk Formulations

> **Context:** Because individual observations are subject to stochastic variations, a function cannot be evaluated on a single point. We treat the data pairs as random variables drawn from a joint probability density function $f(\mathbf{x}, y)$ and evaluate performance globally using expectations.

> [!definition] Expected Generalization Risk
> 
> The predictive performance of a stable, non-random target function $g$ across the entire population distribution is defined as the expected loss, or **Statistical Risk**:
> 
> $$\ell(g) = \mathbb{E}\left[ \mathrm{Loss}(Y, g(\mathbf{X})) \right] = \iint \mathrm{Loss}(y, g(\mathbf{x})) f(\mathbf{x}, y) \, d\mathbf{x} \, dy$$

### Optimal Decision Functions

> **Context:** The ideal goal of statistical learning is to identify the unique function $g^*$ that minimizes the population risk:$$ g^* = \underset{g}{\arg\min}\ \underbrace{\mathbb{E}\mathrm{Loss}(Y, g(\boldsymbol{X}))}_{\ell(g)}. $$The explicit form of this optimal function depends directly on the chosen loss metric.

> [!theorem] Optimal Regression Function under $L_2$ Loss
> 
> For squared-error loss $\mathrm{Loss}(y,\widehat{y}) = (y - \widehat{y})^2$, the optimal prediction function $g^*$ is the **conditional expectation** of $Y$ given $\boldsymbol{X} = \boldsymbol{x}$: $$ g^*(\boldsymbol{x}) = \mathbb{E}[Y \mid \boldsymbol{X} = \boldsymbol{x}]. $$

### Classification Optimal Boundaries

> **Context:** When shifting from continuous estimation to discrete classification under a 0-1 loss framework, the risk minimization objective yields a probabilistic decision rule known as the Bayes Classifier.

> [!theorem] Optimal Bayes Classifier
> 
> Under a zero-one loss function, the risk matches the misclassification probability $\ell(g) = \mathbb{P}[Y \neq g(\mathbf{X})]$. The risk-minimizing optimal function $g^*(\mathbf{x})$ assigns each input vector to the category with the highest posterior probability:
> 
> $$g^*(\mathbf{x}) = \underset{y \in \{0,\dots,c-1\}}{\arg\max} \, f(y \mid \mathbf{x})$$
> 
> where $f(y \mid \mathbf{x}) = \mathbb{P}[Y = y \mid \mathbf{X} = \mathbf{x}]$.

### Sample Estimation Frameworks

> **Context:** The true optimal function $g^*$ cannot be computed directly because the joint distribution $f(\mathbf{x}, y)$ is unknown. In practice, we approximate these population properties using a finite training sample.

> [!definition] Empirical Training Datasets
> 
> - **Random Training Set ($\mathcal{T}$):** A sequence of $n$ independent and identically distributed random variable pairs drawn from the joint distribution:
>     
>     $$\mathcal{T} := \{(\mathbf{X}_1, Y_1), \dots, (\mathbf{X}_n, Y_n)\} \sim \mathrm{i.i.d.} \; f(\mathbf{x},y)$$
>     
> - **Deterministic Realization ($\tau$):** The observed numeric matrix and response vector sampled during an experiment:
>     
>     $$\tau := \{(\mathbf{x}_1, y_1), \dots, (\mathbf{x}_n, y_n)\}$$
>     

### Empirical Risk Minimization

> **Context:** To find a usable alternative to $g^*$, we substitute the analytical population expectation with an empirical sample average calculated over the observed training data.

> [!definition] Training Loss & Empirical Estimation
> 
> The **Training Loss** $\ell_\mathcal{T}(g)$ is the sample-average approximation of the population risk evaluated over the training dataset:
> 
> $$\ell_\mathcal{T}(g) = \frac{1}{n} \sum_{i=1}^{n} \mathrm{Loss}(Y_i, g(\mathbf{X}_i))$$
>
>This is an **unbiased estimator** of the risk for a fixed prediction function $g$, based on the training data.
> 
> To prevent overfitting—where a model memorizes data noise and fits the training samples perfectly ($\ell_\tau(g) = 0$) while generalizing poorly—we restrict the optimization search to a constrained function space $\mathcal{G}$:
> 
> $$g_\mathcal{T}^{\mathcal{G}} = \underset{g \in \mathcal{G}}{\arg\min} \; \ell_\mathcal{T}(g)$$

### Validation Dynamics

> **Context:** Minimizing the empirical training loss can bias our performance estimates. To measure generalization performance accurately, we evaluate the learned model on an independent test dataset that was not used during training.
> - $\tau = \text{fixed dataset}$
> - $\mathcal{T} = \text{random dataset}$

> [!definition] Generalization and Test Loss Metrics
> 
> - **Generalization Risk:** The expected performance of a fixed, trained model $g_\tau^{\mathcal{G}}$ on a new independent observation $(\mathbf{X}, Y)$:
>     
>     $$\ell(g_\tau^{\mathcal{G}}) = \mathbb{E}\left[ \mathrm{Loss}(Y, g_\tau^{\mathcal{G}}(\mathbf{X})) \right]$$
>     
> - **Expected Generalization Risk:** The average performance of the learning algorithm across all possible random training dataset realizations:
>     
>     $$\mathbb{E}\left[\ell(g_\mathcal{T}^{\mathcal{G}})\right] = \mathbb{E}_{\mathcal{T}} \left[ \mathbb{E}_{(\mathbf{X},Y)} \left[ \mathrm{Loss}(Y, g_\mathcal{T}^{\mathcal{G}}(\mathbf{X})) \right] \right]$$
>     
> - **Empirical Test Loss:** An unbiased estimate of the generalization risk computed using a separate, independent dataset $\mathcal{T}'$:
>     
>     $$\ell_{\tau'}(g_\tau^{\mathcal{G}}) := \frac{1}{n'} \sum_{i=1}^{n'} \mathrm{Loss}(Y_i', g_\tau^{\mathcal{G}}(\mathbf{X}_i'))$$
>     

### Linear Model Geometry

> **Context:** The linear function class is a fundamental model space in statistics. Transforming non-linear features into a higher-dimensional space allows us to apply linear estimation techniques, which can then be solved analytically using orthogonal projections.

> [!theorem] Ordinary Least Squares (OLS) Subspace Projection
> 
> Let $\mathbf{X} \in \mathbb{R}^{n \times p}$ denote the design model matrix containing $n$ samples and $p$ features, and let $\boldsymbol{y} \in \mathbb{R}^n$ represent the response vector. The quadratic training loss is given by:
> 
> $$\ell_\tau(\boldsymbol{\beta}) = \frac{1}{n} \|\boldsymbol{y} - \mathbf{X}\boldsymbol{\beta}\|^2$$
> 
> The vector of coefficients $\widehat{\boldsymbol{\beta}}$ that minimizes this sample loss is:
> 
> $$\widehat{\boldsymbol{\beta}} = \underset{\boldsymbol{\beta}}{\arg\min} \; \|\boldsymbol{y} - \mathbf{X}\boldsymbol{\beta}\|^2 = \mathbf{X}^{+}\boldsymbol{y}$$
> 
> where $\mathbf{X}^{+}$ represents the Moore-Penrose pseudo-inverse.
> 
> - **Full Rank Direct Matrix Form:** If $\mathbf{X}$ has full column rank, the solution simplifies to:
>     
>     $$\boxed{\widehat{\boldsymbol{\beta}} = (\mathbf{X}^\top \mathbf{X})^{-1} \mathbf{X}^\top \boldsymbol{y}}$$
>     
> - **Geometric Interpretation:** The fitted value vector $\hat{\boldsymbol{y}} = \mathbf{X}\widehat{\boldsymbol{\beta}}$ represents an orthogonal projection of the observed data vector $\boldsymbol{y}$ onto the subspace spanned by the columns of the design matrix:
>     
>     $$\mathbf{X}\widehat{\boldsymbol{\beta}} = \mathbf{P}\boldsymbol{y} \qquad \mathbf{P} = \mathbf{X}(\mathbf{X}^\top \mathbf{X})^{-1}\mathbf{X}^\top$$
>     

### Core Notation Reference Table

> **Context:** This table standardizes the mathematical notation used throughout the statistical learning framework.

|**Notation Symbol**|**Structural Meaning**|
|---|---|
|$\mathbf{x}$ and $\mathbf{X}$|Fixed realization and random explanatory feature vector.|
|$y$ and $Y$|Fixed realization and random response variable.|
|$f(\mathbf{x}, y)$|Joint probability density function of $(\mathbf{X}, Y)$.|
|$\tau$ and $\mathcal{T}$|Fixed deterministic outcome and random training dataset sample.|
|$\mathbf{X}$|Design model matrix containing row vectors of features ($\mathbf{x}_i^\top$).|
|$g(\mathbf{x})$|Prediction function mapping features to targets.|
|$\ell(g)$|True population risk: $\mathbb{E}\left[\mathrm{Loss}(Y, g(\mathbf{X}))\right]$.|
|$g^*$|Optimal decision function minimizing population risk: $\arg\min_g \ell(g)$.|
|$\ell_\tau(g)$|Empirical training loss computed over the dataset $\tau$.|
|$g_\tau^{\mathcal{G}}$|Empirically optimized model selected from the function class $\mathcal{G}$.|