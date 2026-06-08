## Bootstrap Aggregation (Bagging)

The core idea of **bootstrap aggregation** (a.k.a. _bagging_) is to combine prediction functions learned from multiple data sets, to improve overall prediction accuracy.

> [!tip] When is bagging useful?
> Bagging is especially beneficial for predictors that tend to **overfit**, such as decision trees, where the (unpruned) tree structure is very sensitive to small changes in the training set.

### Averaging over iid training sets

Suppose we have $B$ iid copies $\mathcal{T}_1, \dots, \mathcal{T}_B$ of a training set $\mathcal{T}$. We train $B$ separate regression models (e.g. $B$ different decision trees), giving learners $g_{\mathcal{T}_1}, \dots, g_{\mathcal{T}_B}$, and take their average:

$$ g_{\text{avg}}(\boldsymbol{x}) = \frac{1}{B}\sum_{b=1}^{B} g_{\mathcal{T}_b}(\boldsymbol{x}) \tag{1} $$

> [!note] Law of large numbers 
> As $B \to \infty$, the average prediction function converges to the **expected prediction function**: $$g^{\dagger} := \mathbb{E} g_{\mathcal{T}}$$

---

### Theorem — Expected Squared-Error Generalization Risk

> [!theorem] Expected Squared-Error Generalization Risk
> Let $\mathcal{T}$ be a random training set and let $\boldsymbol{X}, Y$ be a random feature vector and response that are **independent of** $\mathcal{T}$. Then: $$ \mathbb{E}\left(Y - g_{\mathcal{T}}(\boldsymbol{X})\right)^2 \geq \mathbb{E}\left(Y - g^{\dagger}(\boldsymbol{X})\right)^2 $$

This shows that using the expected trainer $g^{\dagger}$ as a prediction function (if it were known) yields an expected squared-error risk **less than or equal to** that of a general predictor $g_{\mathcal{T}}$.

> [!example] Proof
> We have $\boldsymbol{X}, Y$: $$ \mathbb{E}\left[\left(Y - g_{\mathcal{T}}(\boldsymbol{X})\right)^2 ,\middle| \boldsymbol{X}, Y\right] \geq \left(\mathbb{E}[Y \mid \boldsymbol{X}, Y] - \mathbb{E}[g_{\mathcal{T}}(\boldsymbol{X}) \mid \boldsymbol{X}, Y]\right)^2 = \left(Y - g^{\dagger}(\boldsymbol{X})\right)^2 $$ where the inequality follows from $\mathbb{E} U^2 \geq (\mathbb{E} U)^2$ for any (conditional) expectation.
> By the **tower property**: $$ \mathbb{E}\left(Y - g_{\mathcal{T}}(\boldsymbol{X})\right)^2 = \mathbb{E} \left[\mathbb{E} \left[\left(Y - g_{\mathcal{T}}(\boldsymbol{X})\right)^2 \mid \boldsymbol{X}, Y\right]\right] \geq \mathbb{E}\left(Y - g^{\dagger}(\boldsymbol{X})\right)^2 $$

---

### The Bagged Estimator

> [!warning] The catch Multiple **independent** data sets are rarely available in practice.

Instead, we substitute **bootstrapped** data sets. Rather than independent $\mathcal{T}_1, \dots, \mathcal{T}_B$, we obtain random training sets $\mathcal{T}^*_1, \dots, \mathcal{T}^*_B$ by **resampling** from a **single** fixed training set $\tau$, and use them to train $B$ models.

By model averaging as in $(1)$, we get the **bootstrapped aggregated** (bagged) estimator:

$$ g_{\text{bag}}(\boldsymbol{x}) = \frac{1}{B}\sum_{b=1}^{B} g_{\mathcal{T}^*_b}(\boldsymbol{x}) \tag{2} $$

> [!info] Algorithm 1 — Bootstrap Aggregation Sampling
>  **Input:** Training set $\tau = {(\boldsymbol{x}_i, y_i)}_{i=1}^{n}$ and resample size $B$. **Output:** Bootstrapped data sets.
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

---

## The Bootstrap (Statistical Background)

> [!quote] What is the bootstrap? The bootstrap is a flexible, powerful statistical tool to quantify the **uncertainty** associated with an estimator or learning method — e.g. estimating the standard error of a coefficient, or a confidence interval.

### Origin of the name

The term derives from the phrase _"to pull oneself up by one's bootstraps,"_ widely linked to _The Surprising Adventures of Baron Munchausen_ (Rudolph Erich Raspe, 18th c.), where the Baron, fallen to the bottom of a lake, picks himself up by his own bootstraps. It is distinct from the computer-science sense of "booting" a machine, though the derivation is similar.

---

### A Simple Example — Investment Allocation

Suppose we invest a fixed sum in two assets with random returns $X$ and $Y$. We invest a fraction $\alpha$ in $X$ and $1-\alpha$ in $Y$. We choose $\alpha$ to **minimize total risk**:

$$ \min_{\alpha}  \mathrm{Var}(\alpha X + (1-\alpha)Y) $$

The risk-minimizing value is:

$$ \alpha = \frac{\sigma_Y^2 - \sigma_{XY}}{\sigma_X^2 + \sigma_Y^2 - 2\sigma_{XY}} $$

where $\sigma_X^2 = \mathrm{Var}(X)$, $\sigma_Y^2 = \mathrm{Var}(Y)$, and $\sigma_{XY} = \mathrm{Cov}(X, Y)$.

Since these quantities are **unknown**, we estimate them ($\hat\sigma_X^2, \hat\sigma_Y^2, \hat\sigma_{XY}$) from data:

$$ \hat\alpha = \frac{\hat\sigma_Y^2 - \hat\sigma_{XY}}{\hat\sigma_X^2 + \hat\sigma_Y^2 - 2\hat\sigma_{XY}} $$

> [!example] Simulation study 
> Simulating 100 paired observations of $X, Y$ and estimating $\alpha$, repeated **1,000 times**, yields estimates $\hat\alpha_1, \dots, \hat\alpha_{1000}$. With true parameters $\sigma_X^2 = 1$, $\sigma_Y^2 = 1.25$, $\sigma_{XY} = 0.5$, the true value is $\alpha = 0.6$.
> ![[3 - Ensemble Methods-1780860508784.webp|316x296]]
> - **Mean:** $\bar\alpha = \frac{1}{1000}\sum_{r=1}^{1000}\hat\alpha_r = 0.5996$ (very close to $0.6$)
> - **Standard deviation:** $$\sqrt{\frac{1}{1000-1}\sum_{r=1}^{1000}(\hat\alpha_r - \bar\alpha)^2} = 0.083$$ So $\mathrm{SE}(\hat\alpha) \approx 0.083$ — we'd expect $\hat\alpha$ to differ from $\alpha$ by about $0.08$ on average.

---

### Back to the Real World

> [!warning] The fundamental problem 
> For real data we **cannot generate new samples** from the original population.

The bootstrap mimics this process on a computer:
- Rather than drawing independent data sets from the population, we obtain distinct data sets by **repeatedly sampling observations from the original data set _with replacement_**.
- Each **bootstrap data set** is the same size as the original; some observations appear more than once, and some not at all.

> [!example] Bootstrap with $n = 3$ observations 
> ![[3 - Ensemble Methods-1780860560147.webp|238x223]]
> From original data $Z$, each bootstrap set $Z^{*b}$ contains $n$ observations sampled with replacement, and each gives an estimate $\hat\alpha^{*b}$. For the first bootstrap set $Z^{*1}$ we get estimate $\hat\alpha^{*1}$; repeating $B$ times (say 100 or 1000) gives $Z^{*1}, \dots, Z^{*B}$ with estimates $\hat\alpha^{*1}, \dots, \hat\alpha^{*B}$.

### Bootstrap standard error

$$ \mathrm{SE}_B(\hat\alpha) = \sqrt{\frac{1}{B-1}\sum_{r=1}^{B}\left(\hat\alpha^{*r} - \bar{\hat\alpha}^{*}\right)^2} $$

This estimates the standard error of $\hat\alpha$ from the original data set. _(In the example, $\mathrm{SE}_B(\hat\alpha) = 0.087$, vs. the true-simulation value $0.083$ — very close.)_

> [!note] General picture
> ![[3 - Ensemble Methods-1780860636066.webp]] 

---

### The Bootstrap in General

> [!question] Time series caveat
>  If the data is a **time series**, we _can't_ simply sample observations with replacement — doing so destroys the temporal dependence structure. **Fix:** create **blocks** of consecutive observations, sample blocks with replacement, then paste sampled blocks together (block bootstrap).

### Other uses of the bootstrap

- **Primary use:** obtaining standard errors of an estimate.
- **Confidence intervals:** e.g. the 5% and 95% quantiles of the 1000 bootstrap values give $(0.43, 0.72)$ — an approximate **90% confidence interval** for true $\alpha$.
- This is the **Bootstrap Percentile** confidence interval — the simplest of many approaches.

---

## Bootstrap and Prediction Error

> [!danger] Can the bootstrap estimate prediction error? 
> Not directly. In **cross-validation**, each of the $K$ validation folds is _distinct_ (no overlap with the training folds) — crucial for success.
> 
> If we used each bootstrap dataset as the training sample and the original sample as validation, there's **significant overlap**: on average about **two-thirds** of the original points appear in each bootstrap sample. This causes the bootstrap to **seriously underestimate** the true prediction error. (The reverse arrangement is even worse.)

> [!tip] Partial fix 
> Only use predictions for observations _not_ in the current bootstrap sample → leads to [[#Out-of-Bag (OOB) Observations|OOB estimation]]. But this gets complicated; **cross-validation** is usually the simpler, more attractive approach.

---

## Pre-validation

> [!info] Motivation 
> In microarray/genomic studies, comparing a predictor derived from many _biomarkers_ against standard clinical predictors **on the same dataset used to derive it** biases results strongly in favor of the biomarker predictor. **Pre-validation** makes a fairer comparison by forming a version of the adaptive predictor that hasn't "seen" the response $y$.

---

## Bagging for Classification

For **classification**, $g_{\text{bag}}$ takes the **majority vote** among $\{g_{\mathcal{T}^*_b}\}_{b=1}^{B}$ — i.e. accept the most frequent predicted class.

### Bias–Variance decomposition

While bagging applies to any model, it is most effective for predictors **sensitive to small changes** in the training set. To see why, decompose the expected generalization risk:

$$ \mathbb{E},\ell(g_{\mathcal{T}}) = \ell^{*} + \underbrace{\mathbb{E}\left(\mathbb{E}[g_{\mathcal{T}}(\boldsymbol{X})\mid\boldsymbol{X}] - g^{*}(\boldsymbol{X})\right)^2}_{\text{expected squared bias}} + \underbrace{\mathbb{E}\big[\mathrm{Var}[g_{\mathcal{T}}(\boldsymbol{X})\mid\boldsymbol{X}]\big]}_{\text{expected variance}} \tag{3} $$

> [!important] (Un)stable predictors 
>Since $\mathbb{E}g_{\text{bag}}(\boldsymbol{x}) = \mathbb{E}g_{\mathcal{T}}(\boldsymbol{x})$, bagging **leaves the bias unchanged** — any improvement must come from the **expected variance** term.
> 
> - **Unstable** (high variance, benefit from bagging): decision trees, neural networks, subset selection in linear regression.
> - **Stable** (insensitive to data changes, little benefit): $K$-nearest neighbors.
> 
> For **independent** training sets, variance is reduced by a factor $B$: $$\mathrm{Var}g_{\text{bag}}(\boldsymbol{x}) = B^{-1}\mathrm{Var}g_{\mathcal{T}}(\boldsymbol{x})$$

---

## Out-of-Bag (OOB) Observations

Bagging lets us estimate the generalization risk **without a separate test set**.

> [!important] The $e^{-1}$ fact 
> For large sample sizes, on average a fraction $e^{-1} \approx 0.37$ (about a third) of the original points are **not** included in a given bootstrapped set $\mathcal{T}^*_b$. _(Exercise!)_ These left-out points — **out-of-bag (OOB) observations** — can be used for loss estimation.

> [!algorithm] Algorithm 2 — Out-of-Bag Loss Estimation 
> **Input:** Original data $\tau$, bootstrapped sets $\{\mathcal{T}^*1, \dots, \mathcal{T}^*B\}$, trained predictors $\{g_{\mathcal{T}^*1}, \dots, g_{\mathcal{T}^*B}\}$. **Output:** OOB loss for the averaged model.
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

---

## Example — Bagging a Regression Tree

Compares a single decision tree against the bagged estimator using $R^2$ (coefficient of determination).

```python
import numpy as np
from sklearn.datasets import make_friedman1
from sklearn.tree import DecisionTreeRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import r2_score

np.random.seed(100)

# create regression problem
n_points = 1000  # points
x, y = make_friedman1(n_samples=n_points, n_features=15,
                      noise=1.0, random_state=100)

# split to train/test set
x_train, x_test, y_train, y_test = \
    train_test_split(x, y, test_size=0.33, random_state=100)

# training a single tree
regTree = DecisionTreeRegressor(random_state=100)
regTree.fit(x_train, y_train)
yhat = regTree.predict(x_test)

# Bagging construction
n_estimators = 500
bag = np.empty((n_estimators), dtype=object)
bootstrap_ds_arr = np.empty((n_estimators), dtype=object)

for i in range(n_estimators):
    # sample bootstrapped data set
    ids = np.random.choice(range(0, len(x_test)), size=len(x_test),
                           replace=True)
    x_boot = x_train[ids]
    y_boot = y_train[ids]
    bootstrap_ds_arr[i] = np.unique(ids)
    bag[i] = DecisionTreeRegressor()
    bag[i].fit(x_boot, y_boot)

# bagging prediction
yhatbag = np.zeros(len(y_test))
for i in range(n_estimators):
    yhatbag = yhatbag + bag[i].predict(x_test)
yhatbag = yhatbag / n_estimators

# out-of-bag loss estimation
oob_pred_arr = np.zeros(len(x_train))
for i in range(len(x_train)):
    x = x_train[i].reshape(1, -1)
    C = []
    for b in range(n_estimators):
        if np.isin(i, bootstrap_ds_arr[b]) == False:
            C.append(b)
    for pred in bag[C]:
        oob_pred_arr[i] = oob_pred_arr[i] + (pred.predict(x) / len(C))

L_oob = r2_score(y_train, oob_pred_arr)

print("DecisionTreeRegressor R^2 score = ", r2_score(y_test, yhat),
      "\nBagging R^2 score = ", r2_score(y_test, yhatbag),
      "\nBagging OOB R^2 score = ", L_oob)
```

> [!success] Output
> 
> ```
> DecisionTreeRegressor R^2 score =  0.5754
> Bagging R^2 score          =  0.7612
> Bagging OOB R^2 score       =  0.7758
> ```
> 
> Bagging dramatically improves $R^2$ over a single tree, and the OOB score closely tracks the test score.

---

## Random Forests

### The correlation problem

For iid predictions $Z_b = g_{\mathcal{T}_b}(\boldsymbol{x})$ from **independent** sets with $\mathrm{Var},Z_b = \sigma^2$, the variance of the average $\bar Z_B$ is $\sigma^2/B$.

But with **bootstrapped** sets, the $Z_b = g_{\mathcal{T}^*_b}(\boldsymbol{x})$ are identically distributed but **correlated**, with positive pairwise correlation $\varrho$:

$$ \mathrm{Var}\bar Z_B = \varrho\sigma^2 + \frac{\sigma^2(1-\varrho)}{B} \tag{4} $$

> [!warning] The second term $\to 0$ as $B \to \infty$, but the first term $\varrho\sigma^2$ **stays constant**. Correlation places a floor on variance reduction.

### Why trees correlate

If one feature gives a very good split, it will be selected at the **root** of every tree → highly correlated predictions → averaging gives little improvement.

> [!important] The Random Forest idea 
> Perform bagging **plus decorrelation**: at each split, consider only a randomly selected subset of $m \leq p$ features. This breaks the dominance of any single strong feature and reduces $\varrho$.

> [!info] Algorithm 3 — Random Forest Construction 
> **Input:** Training set $\tau = {(\boldsymbol{x}_i, y_i)}_{i=1}^{n}$, number of trees $B$, number of features $m \leq p$ ($p$ = total features). 
> **Output:** Ensemble of trees.
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
> - **Classification:** majority vote among $\{g_{\mathcal{T}^*_b}\}$

---

### Example — Random Forest Regressor

```python
from sklearn.datasets import make_friedman1
from sklearn.model_selection import train_test_split
from sklearn.metrics import r2_score
from sklearn.ensemble import RandomForestRegressor

# create regression problem
n_points = 1000  # points
x, y = make_friedman1(n_samples=n_points, n_features=15,
                      noise=1.0, random_state=100)

# split to train/test set
x_train, x_test, y_train, y_test = \
    train_test_split(x, y, test_size=0.33, random_state=100)

rf = RandomForestRegressor(n_estimators=500, oob_score=True,
                           max_features=8, random_state=100)
rf.fit(x_train, y_train)
yhatrf = rf.predict(x_test)

print("RF R^2 score = ", r2_score(y_test, yhatrf),
      "\nRF OOB R^2 score = ", rf.oob_score_)
```

> [!success] Output
> 
> ```
> RF R^2 score      =  0.8107
> RF OOB R^2 score   =  0.8261
> ```
> 
> Better than plain bagging ($0.7612$) thanks to decorrelation.

---

### Choosing the number of subset features $m$

Default values for $m$

 - **Regression:** $\lfloor p/3 \rfloor$
 - **Classification:** $\lfloor \sqrt{p} \rfloor$
  Standard practice: treat $m$ as a **hyperparameter** to tune per problem.

Bagging decision trees is a **special case** of random forests (take $m = p$). Consequently, the OOB loss is readily available for random forests too.

> [!warning] Trade-off
>  Bagging/RF improve accuracy but cost **interpretability** — you can no longer read off a single tree.

---

## Feature Importance

Addresses the interpretability loss. Each internal node $v$ of a tree induces a decrease $\Delta\text{Loss}(v)$ in training loss. For splits of the form $\mathbb{1}{x_j \leq \xi}$, each node is associated with the feature $x_j$ that determines the split.

**Per-tree feature importance:**

$$ I_{\mathcal{T}}(x_j) = \sum_{v\ \text{internal} \in \mathcal{T}} \Delta\text{Loss}(v) \mathbb{I}\{x_j \text{ is associated with } v\}, \quad 1 \leq j \leq p \tag{5} $$

**Random-forest feature importance** (average over trees):
$$ I_{\text{RF}}(x_j) = \frac{1}{B}\sum_{b=1}^{B} I_{\mathcal{T}_b}(x_j), \quad 1 \leq j \leq p \tag{6} $$

### Example — Feature Importance

Classification with 15 features, only **5 informative**.

```python
import numpy as np
from sklearn.datasets import make_classification
from sklearn.ensemble import RandomForestClassifier
import matplotlib.pyplot as plt, pylab

n_points = 1000
x, y = make_classification(n_samples=n_points, n_features=15,
                           n_informative=5, n_redundant=0, n_repeated=0,
                           random_state=100, shuffle=False)

rf = RandomForestClassifier(n_estimators=200, max_features="log2")
rf.fit(x, y)

importances = rf.feature_importances_
indices = np.argsort(importances)[::-1]
for f in range(15):
    print("Feature %d (%f)" % (indices[f] + 1, importances[indices[f]]))

std = np.std([rf.feature_importances_ for tree in rf.estimators_], axis=0)

f = plt.figure()
plt.bar(range(x.shape[1]), importances[indices],
        color="b", yerr=std[indices], align="center")
plt.xticks(range(x.shape[1]), indices + 1)
plt.xlim([-1, x.shape[1]])
pylab.xlabel("feature index")
pylab.ylabel("importance")
plt.show()
```

> [!success] Result The procedure correctly identifies features $x_1, x_2, x_3, x_4, x_5$ as the important ones (their importance bars dominate the other 10 noise features). While 200 trees are impossible to visualize directly, the importance plot recovers interpretable structure.

---

## Summary / Key Takeaways

> [!summary]
> 
> - **Bagging** averages models trained on bootstrap resamples → reduces **variance**, leaves **bias** unchanged.
> - The expected trainer $g^{\dagger}$ has risk $\leq$ any single $g_{\mathcal{T}}$.
> - Bagging helps **unstable** predictors (trees, NNs) most; useless for **stable** ones (kNN).
> - **OOB** observations ($\approx e^{-1} \approx 37%$ left out) give a free risk estimate.
> - The **bootstrap** quantifies estimator uncertainty (SEs, percentile CIs) but **underestimates prediction error** due to train/validation overlap → prefer cross-validation.
> - **Random Forests** = bagging + feature subsetting ($m \leq p$) to **decorrelate** trees; the constant term $\varrho\sigma^2$ in $(4)$ is what RF attacks.
> - **Feature importance** restores interpretability by averaging per-node loss decreases.
