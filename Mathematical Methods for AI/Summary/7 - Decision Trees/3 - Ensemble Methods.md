### Bootstrap Aggregation (Bagging)

> **Context:** The core idea of bootstrap aggregation is to combine prediction functions learned from multiple data sets to improve overall prediction accuracy. It is especially beneficial for predictors that tend to overfit, such as decision trees, whose structure is very sensitive to small changes in the training set.

> [!definition] Averaging over iid Training Sets
> 
> Given $B$ iid copies $\mathcal{T}_1, \dots, \mathcal{T}_B$ of a training set, train $B$ separate models to obtain learners $g_{\mathcal{T}_1}, \dots, g_{\mathcal{T}_B}$ and take their average:
> 
> $$ g_{\text{avg}}(\boldsymbol{x}) = \frac{1}{B}\sum_{b=1}^{B} g_{\mathcal{T}_b}(\boldsymbol{x}) $$
> 
> By the law of large numbers, as $B \to \infty$ the average converges to the **expected prediction function** $g^{\dagger} := \mathbb{E}\, g_{\mathcal{T}}$.

> [!theorem] Expected Squared-Error Generalization Risk
> 
> Let $\mathcal{T}$ be a random training set and $\boldsymbol{X}, Y$ a feature vector and response independent of $\mathcal{T}$. Then:
> 
> $$ \mathbb{E}\left(Y - g_{\mathcal{T}}(\boldsymbol{X})\right)^2 \geq \mathbb{E}\left(Y - g^{\dagger}(\boldsymbol{X})\right)^2 $$
> 
> Using the expected trainer $g^{\dagger}$ (if known) yields an expected squared-error risk less than or equal to that of a general predictor $g_{\mathcal{T}}$.

### The Bagged Estimator

> **Context:** Multiple independent data sets are rarely available in practice. Instead, bootstrapped data sets are substituted: random training sets are obtained by resampling with replacement from a single fixed training set, and used to train $B$ models that are then averaged.

> [!definition] Bagged Estimator
> 
> Obtaining random training sets $\mathcal{T}^*_1, \dots, \mathcal{T}^*_B$ by resampling from a single training set $\tau$, the bootstrapped aggregated (bagged) estimator is:
> 
> $$ g_{\text{bag}}(\boldsymbol{x}) = \frac{1}{B}\sum_{b=1}^{B} g_{\mathcal{T}^*_b}(\boldsymbol{x}) $$
> 
> For **classification**, $g_{\text{bag}}$ takes the **majority vote** among $\{g_{\mathcal{T}^*_b}\}_{b=1}^{B}$.

> [!algorithm] Bootstrap Aggregation Sampling
> 
> **Input:** Training set $\tau = \{(\boldsymbol{x}_i, y_i)\}_{i=1}^{n}$ and resample size $B$. **Output:** Bootstrapped data sets.
> 
> ```
> for b = 1 to B do
>     T*_b ← ∅
>     for i = 1 to n do
>         Draw U ~ Uniform(0, 1)
>         I ← ⌈nU⌉              // select random index
>         T*_b ← T*_b ∪ {(x_I, y_I)}
> return T*_b,  b = 1, ..., B
> ```

### The Bootstrap

> **Context:** The bootstrap is a flexible, powerful statistical tool to quantify the uncertainty associated with an estimator or learning method — for example estimating the standard error of a coefficient or a confidence interval. For real data we cannot generate new samples from the original population, so the bootstrap mimics this process on a computer.

> [!definition] The Bootstrap Procedure
> 
> - Rather than drawing independent data sets from the population, obtain distinct data sets by repeatedly sampling observations from the original data set **with replacement**.
>     
> - Each **bootstrap data set** is the same size as the original; some observations appear more than once and some not at all.
>     
> - Each bootstrap set $Z^{*b}$ yields an estimate $\hat\alpha^{*b}$; repeating $B$ times gives estimates $\hat\alpha^{*1}, \dots, \hat\alpha^{*B}$.
>     

> [!definition] Bootstrap Standard Error
> 
> $$ \mathrm{SE}_B(\hat\alpha) = \sqrt{\frac{1}{B-1}\sum_{r=1}^{B}\left(\hat\alpha^{*r} - \bar{\hat\alpha}^{*}\right)^2} $$
> 
> This estimates the standard error of $\hat\alpha$ from the original data set.
>  ![[3 - Ensemble Methods-1780860636066.webp]] 

> [!note] Other Uses and Caveats
> 
> - **Primary use:** obtaining standard errors of an estimate.
>     
> - **Confidence intervals:** the 5% and 95% quantiles of the bootstrap values give an approximate 90% **Bootstrap Percentile** confidence interval.
>     
> - **Time series caveat:** sampling observations with replacement destroys temporal dependence. Fix: create **blocks** of consecutive observations, sample blocks with replacement, then paste them together (block bootstrap).
>     

### Bootstrap and Prediction Error

> **Context:** The bootstrap cannot directly estimate prediction error. In cross-validation each validation fold is distinct from the training folds, which is crucial. Using a bootstrap dataset for training and the original sample for validation causes significant overlap, biasing the estimate.

> [!definition] Why the Bootstrap Underestimates Prediction Error
> 
> On average about **two-thirds** of the original points appear in each bootstrap sample, so using the original sample as validation creates heavy train/validation overlap. This causes the bootstrap to **seriously underestimate** the true prediction error.
> 
> - **Partial fix:** only use predictions for observations *not* in the current bootstrap sample → OOB estimation.
>     
> - Since this gets complicated, **cross-validation** is usually the simpler, more attractive approach.
>     

### Bias–Variance Decomposition for Bagging

> **Context:** Although bagging applies to any model, it is most effective for predictors sensitive to small changes in the training set. Decomposing the expected generalization risk shows why: bagging leaves the bias unchanged, so any improvement must come from the variance term.

> [!definition] Bias–Variance Decomposition
> 
> $$ \mathbb{E}\,\ell(g_{\mathcal{T}}) = \ell^{*} + \underbrace{\mathbb{E}\left(\mathbb{E}[g_{\mathcal{T}}(\boldsymbol{X})\mid\boldsymbol{X}] - g^{*}(\boldsymbol{X})\right)^2}_{\text{expected squared bias}} + \underbrace{\mathbb{E}\big[\mathrm{Var}[g_{\mathcal{T}}(\boldsymbol{X})\mid\boldsymbol{X}]\big]}_{\text{expected variance}} $$
> 
> Since $\mathbb{E}\,g_{\text{bag}}(\boldsymbol{x}) = \mathbb{E}\,g_{\mathcal{T}}(\boldsymbol{x})$, bagging leaves the **bias unchanged**; improvement comes only from the **expected variance** term.
> 
> - **Unstable** (high variance, benefit from bagging): decision trees, neural networks, subset selection in linear regression.
>     
> - **Stable** (insensitive to data changes, little benefit): $K$-nearest neighbors.
>     
> - For independent training sets, variance is reduced by a factor $B$: $\mathrm{Var}\,g_{\text{bag}}(\boldsymbol{x}) = B^{-1}\mathrm{Var}\,g_{\mathcal{T}}(\boldsymbol{x})$.
>     

### Out-of-Bag (OOB) Observations

> **Context:** Bagging allows estimating the generalization risk without a separate test set. For large samples, on average a fraction $e^{-1} \approx 0.37$ of the original points are not included in a given bootstrapped set; these left-out points can be used for loss estimation.

> [!definition] OOB Estimation
> 
> For large sample sizes, on average about a third ($e^{-1} \approx 0.37$) of the original points are **not** included in a given bootstrapped set $\mathcal{T}^*_b$. These **out-of-bag (OOB)** observations can be used for loss estimation: each point is predicted using only the trees for which it was OOB.

> [!algorithm] Out-of-Bag Loss Estimation
> 
> **Input:** Original data $\tau$, bootstrapped sets, trained predictors. **Output:** OOB loss for the averaged model.
> 
> ```
> for i = 1 to n do
>     C_i ← ∅                    // predictors NOT depending on (x_i, y_i)
>     for b = 1 to B do
>         if (x_i, y_i) ∉ T*_b then C_i ← C_i ∪ {b}
>     Y'_i ← |C_i|⁻¹ Σ_{b ∈ C_i} g_{T*_b}(x_i)
>     L_i ← Loss(y_i, Y'_i)
> L_OOB ← (1/n) Σ_{i=1}^{n} L_i
> return L_OOB
> ```

### Random Forests

> **Context:** With independent sets the variance of the average is $\sigma^2/B$, but bootstrapped predictions are identically distributed and positively correlated. The correlation places a floor on variance reduction that bagging alone cannot overcome.

> [!definition] The Correlation Problem
> 
> For bootstrapped sets, the predictions $Z_b = g_{\mathcal{T}^*_b}(\boldsymbol{x})$ are identically distributed but correlated, with positive pairwise correlation $\varrho$:
> 
> $$ \mathrm{Var}\,\bar Z_B = \varrho\sigma^2 + \frac{\sigma^2(1-\varrho)}{B} $$
> 
> The second term vanishes as $B \to \infty$, but the first term $\varrho\sigma^2$ **stays constant**. If one feature gives a very good split, it is selected at the root of every tree, producing highly correlated predictions.

> [!definition] The Random Forest Idea
> 
> Perform bagging **plus decorrelation**: at each split, consider only a randomly selected subset of $m \leq p$ features. This breaks the dominance of any single strong feature and reduces $\varrho$.

> [!algorithm] Random Forest Construction
> 
> **Input:** Training set $\tau$, number of trees $B$, number of features $m \leq p$. **Output:** Ensemble of trees.
> 
> ```
> Generate bootstrapped sets {T*_1, ..., T*_B} via Algorithm 1
> for b = 1 to B do
>     Randomly select m of p features, without replacement
>     Using only these features, train a decision tree g_{T*_b}
> return {g_{T*_b}}, b = 1, ..., B
> ```
> 
> - **Regression:** $g_{\text{RF}}(\boldsymbol{x}) = \dfrac{1}{B}\sum_{b=1}^{B} g_{\mathcal{T}^*_b}(\boldsymbol{x})$
>     
> - **Classification:** majority vote among $\{g_{\mathcal{T}^*_b}\}$
>     

> [!definition] Choosing the Number of Subset Features $m$
> 
> Default values:
> 
> - **Regression:** $\lfloor p/3 \rfloor$
>     
> - **Classification:** $\lfloor \sqrt{p} \rfloor$
>     
> 
> Standard practice treats $m$ as a **hyperparameter** to tune per problem. Bagging decision trees is a special case of random forests ($m = p$), so the OOB loss is also readily available for random forests. The accuracy gain comes at the cost of **interpretability**.

### Feature Importance

> **Context:** Ensembles lose the interpretability of a single tree. Feature importance addresses this by measuring how much each feature contributes to reducing the training loss across the splits where it is used, averaged over all trees.

> [!definition] Feature Importance
> 
> Each internal node $v$ induces a decrease $\Delta\text{Loss}(v)$ in training loss, associated with the feature $x_j$ that determines the split.
> 
> **Per-tree feature importance:**
> 
> $$ I_{\mathcal{T}}(x_j) = \sum_{v\ \text{internal} \in \mathcal{T}} \Delta\text{Loss}(v)\, \mathbb{I}\{x_j \text{ is associated with } v\}, \quad 1 \leq j \leq p $$
> 
> **Random-forest feature importance** (average over trees):
> 
> $$ I_{\text{RF}}(x_j) = \frac{1}{B}\sum_{b=1}^{B} I_{\mathcal{T}_b}(x_j), \quad 1 \leq j \leq p $$

### Pre-validation

> **Context:** In microarray and genomic studies, comparing a predictor derived from many biomarkers against standard clinical predictors on the same dataset used to derive it strongly biases results in favor of the biomarker predictor.

> [!definition] Pre-validation
> 
> Pre-validation makes a fairer comparison by forming a version of the adaptive predictor that has not "seen" the response $y$, so the comparison against standard predictors is unbiased.
