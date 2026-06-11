### 1. Generalization Risk & Test Loss Boundaries

### Mathematical Foundations

> **Context:** Quantifying the performance of an empirically learned function requires estimating its behavior on unobserved data. While population expectations are analytically inaccessible, independent test partitions provide an unbiased proxy for this risk, subject to sample constraints.

> [!definition] Generalization Risk and Sample Constraints
> 
> Given a deterministic training sample outcome $\tau$, a restricted function space $\mathcal{G}$, and an empirically optimized learner $g_\tau^{\mathcal{G}}$, the true generalization risk is the population-level expected loss:
> 
> $$\ell(g_\tau^{\mathcal{G}}) = \mathbb{E}\left[ \mathrm{Loss}\big(Y, g_\tau^{\mathcal{G}}(X)\big) \right]$$
> 
> Let $\tau' = \{(x_1', y_1'), \dots, (x_{n'}', y_{n'}')\}$ define an independent test sample drawn from the identical joint distribution. The empirical test loss is defined as:
> 
> $$\ell_{\tau'}(g_\tau^{\mathcal{G}}) := \frac{1}{n'} \sum_{i=1}^{n'} \mathrm{Loss}\big(Y_i' , g_\tau^{\mathcal{G}}(X_i')\big)$$
> 
> **Theoretical Caveats:**
> 
> - **Sample Dependency:** The generalization risk $\ell(g_\tau^{\mathcal{G}})$ is a random variable conditional on the specific training sample $\tau$; different training configurations yield distinct risk surfaces.
>     
> - **Data Inefficiency:** Allocating a fixed subset of size $n'$ strictly for validation diminishes the sample size available for empirical training, which can lead to a higher estimation error for the learner.
>     

### 2. The Cross-Validation Framework

### Structural Partitioning

> **Context:** When data limits prevent the use of a separate test set, resampling methods reuse the available sample to estimate the expected performance of a learning algorithm. This shifts the target from estimating the risk of a specific model to estimating the expected risk of the training process itself.

> [!definition] Expected Risk Approximation
> 
> For complex function classes $\mathcal{G}$, it is very difficult to derive simple formulas for the approximation and statistical errors, cross-validation approximates the **Expected Generalization Risk** $\mathbb{E}\left[\ell(g_\mathcal{T})\right]$. It computes this approximation by evaluating the **average error** across multiple complementary data partitions:
> 
> $$\mathrm{CV} \approx \mathbb{E}_{\mathcal{T}}\left[ \mathbb{E}_{(X,Y)}\left[ \mathrm{Loss}(Y, g_\mathcal{T}(X)) \right] \right]$$
>![[2 - Estimating Risk-1780688576170.webp|356x232]]
>

****### 3. K-Fold Cross-Validation Formal Mechanics

### Algorithmic Formulation

> **Context:** To ensure every observation is used for both training and validation, the dataset is divided into equally sized subsets. The model is then iteratively trained on all subsets except one, which is held out for testing.

> [!theorem] K-Fold Cross-Validation Loss Formulation
> 
> Let a dataset $\tau$ of size $n$ be partitioned into $K$ disjoint subsets (folds) $C_1, \dots, C_K$ of corresponding sizes $n_1, \dots, n_K$, such that $\sum_{k=1}^K n_k = n$ and $n_k \approx \frac{n}{K}$.
> 
> Let $\tau_{-k} = \tau \setminus C_k$ denote the training sample remaining after omitting fold $C_k$, and let $\ell_{C_k}(g_{\tau_{-k}})$ define the empirical test loss evaluated on the held-out partition $C_k$.
> 
> Each individual partition loss acts as an **unbiased estimator** of the generalization risk for that specific training slice:
> 
> $$\mathbb{E}\left[ \ell_{C_k}(g_{\tau_{-k}}) \right] = \ell(g_{\tau_{-k}})$$
> 
> The total $K$-fold cross-validation risk estimator $\mathrm{CV}_K$ is the weighted sum of these partition losses:
> 
> $$\boxed{\mathrm{CV}_K := \sum_{k=1}^{K} \frac{n_k}{n} \ell_{C_k}(g_{\tau_{-k}}) = \frac{1}{n} \sum_{i=1}^{n} \mathrm{Loss}\big(g_{\tau_{-\kappa(i)}}(x_i) , y_i\big)}$$
> 
> where $\kappa(i) \in \{1, \dots, K\}$ maps observation index $i$ to its assigned validation fold.

### Estimator Properties and Asymptotic Limits

> **Context:** Choosing the number of folds $K$ introduces a statistical tradeoff between bias and variance. The extreme case where each observation forms its own fold eliminates a key source of training sample distortion.

> [!definition] Leave-One-Out Cross-Validation (LOOCV)
> 
> When the number of folds matches the sample size ($K = n$), the partitioning limits each validation fold $C_k$ to a single observation. The LOOCV estimator is given by:
> 
> $$\mathrm{CV}_n = \frac{1}{n} \sum_{i=1}^{n} \mathrm{Loss}\big(g_{\tau_{-i}}(x_i) , y_i\big)$$
> 
> **Statistical Properties:**
> 
> - **Bias:** LOOCV has minimal bias for the expected generalization risk $\mathbb{E}\left[\ell(g_\tau)\right]$ because each sub-model is trained on $n-1$ observations, making the training sample size nearly identical to the full dataset.
>     
> - **Variance:** $\mathrm{CV}_n$ can exhibit high variance because the $n$ distinct training sets $\tau_{-i}$ are highly correlated with one another. This high correlation can cause the average of the individual fold losses to be more volatile than when using smaller values of $K$.
>     
> - **Determinism:** Unlike smaller $K$-fold partitions that depend on a random split, LOOCV is entirely deterministic.
>     

### 4. Methodological Validity and Information Leakage

### The Information Leakage Trap

> **Context:** A common failure mode in validation loops occurs when supervised operations, such as feature selection, are applied to the full dataset before partitioning. This leaks information from the validation sets into the training process, leading to overly optimistic risk estimates.

> [!definition] Supervised Feature Selection Leakage
> 
> Let a pipeline consist of a feature selection operator $\mathcal{S}(X, Y)$ followed by a supervised learning algorithm $\mathcal{A}$. 
> If $\mathcal{S}$ utilizes target labels $Y$ across the full dataset to isolate a lower-dimensional feature space before the cross-validation split is applied, the resulting risk estimate is invalid:
> 
> $$\mathrm{CV}_{\text{invalid}} = f\big(\mathcal{A}(\mathcal{S}(X, Y)_{\text{all}})\big) \implies \mathrm{CV} \to 0 \quad \text{even if } X \perp Y$$
> 
> _Supervised preprocessing changes the distribution of the data, meaning the held-out folds are no longer independent of the training sample._

### Correct Validation Routing

> **Context:** To ensure validation remains valid, any preprocessing step that uses the target labels must be contained entirely within the cross-validation loop. This ensures that feature selection is performed using only the data within the active training fold.

```
WRONG PIPELINE (Information Leakage)
┌────────────────────────────────────────────────────────┐
│ Full Dataset (Features X + Labels Y)                   │
└───────────────────────────┬────────────────────────────┘
                            ▼
           [ Supervised Feature Selection ]  <── Target labels leaked here!
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ Split into Cross-Validation Folds                      │
└────────────────────────────────────────────────────────┘

──────────────────────────────────────────────────────────────────────────

RIGHT PIPELINE (Strict Separation)
┌────────────────────────────────────────────────────────┐
│ Split Full Dataset into K Folds                        │
└───────────────────────────┬────────────────────────────┘
                            ▼
            Iterate for each fold k = 1,...,K:
┌──────────────────────────────────┐  ┌──────────────────────────────────┐
│ Training Fold (τ_{-k})           │  │ Held-out Validation Fold (C_k)   │
└─────────────────┬────────────────┘  └─────────────────┬────────────────┘
                  ▼                                     │
    [ Run Feature Selection ]                           │
                  │                                     │
                  ▼                                     ▼
      [ Train Model Algorithm ] ──────────────> [ Evaluate Test Loss ]
```

> [!theorem] Non-Leaking Validation Protocol
> 
> To compute an unbiased estimate of the expected generalization risk, all supervised transformations must be isolated within the individual fold operations:
> 
> $$\boxed{\mathrm{CV}_{K} = \sum_{k=1}^{K} \frac{n_k}{n} \cdot \ell_{C_k}\left( \mathcal{A}\left[ \mathcal{S}\left(\tau_{-k}\right) \right] \right)}$$
> 
> This protocol guarantees that the feature selector $\mathcal{S}$ never processes information from the validation partition $C_k$.

### Architectural Resampling Comparison

|**Method Architecture**|**Sample Size per Fold**|**Computational Cost**|**Bias / Variance Tradeoff**|
|---|---|---|---|
|**Single Validation Split**|$\frac{n}{2}$|Low ($\mathcal{O}(1)$ updates)|High Bias (underestimates training size); High variance across random splits.|
|**$K$-Fold Cross-Validation**|$\frac{n}{K}$|Moderate ($\mathcal{O}(K)$ updates)|Low Bias; stable variance balances performance and efficiency.|
|**Leave-One-Out (LOOCV)**|$1$|High ($\mathcal{O}(n)$ updates)|Lowest Bias for the expected risk; higher variance due to highly correlated training subsets.|