## 1. Generalization Risk & Test Loss

For a training set $\tau$, a prediction function class $\mathcal{G}$, and a learner $g_\tau^{\mathcal{G}}$, the **generalization risk** is the expected loss:
$$\ell(g_\tau^{\mathcal{G}}) = \mathbb{E},\mathrm{Loss}\big(Y, g_\tau^{\mathcal{G}}(X)\big)$$

The most straightforward way to quantify generalization risk is to estimate it via the **test loss**:
$$\ell_{\tau'}(g_\tau^{\mathcal{G}}) := \frac{1}{n'} \sum_{i=1}^{n'} \mathrm{Loss}\big(Y_i' , g_\tau^{\mathcal{G}}(X_i')\big)$$

where ${(x_1', y_1'), \dots, (x_{n'}', y_{n'}')} =: \tau'$ is a **test sample**.

> [!warning] Key caveats
> 
> - The generalization risk **depends inherently on the training set** — different training sets may yield significantly different estimates.
> - When data is limited, reserving a substantial proportion for testing (instead of training) may be **uneconomical**.

---

## 2. Cross-Validation

> [!info] Motivation
>  For complex function classes $\mathcal{G}$, it is very difficult to derive simple formulas for the approximation and statistical errors, let alone for the generalization risk or expected generalization risk.

When there is an abundance of data, the easiest way to assess the generalization risk for a given training set $\tau$ is to obtain a test set $\tau'$ and evaluate the test loss:
$$
\ell_{\tau'}(g_\tau^{\mathcal{G}}) := \frac{1}{n'} \sum_{i=1}^{n'} \mathrm{Loss}\big(y_i' , g_\tau^{\mathcal{G}}(x_i')\big)
$$
When a sufficiently large test set is not available but computational resources are cheap, one can instead gain direct knowledge of the expected generalization risk via a computationally intensive method called **cross-validation**.
### How it works 

- Make multiple identical copies of the data set, and partition each copy into different **training** sets (blue) and **test** sets (pink).
- For each partition, estimate the model parameters using **only training data**, then predict the responses for the test set.
- The **average loss** between predicted and observed responses is a measure of the model's predictive power.
![[2 - Estimating Risk-1780688576170.webp]]

---

## 3. $K$-Fold Cross-Validation (Formal)
Partition a data set $\tau$ of size $n$ into $K$ **folds** $C_1, \dots, C_K$ of sizes $n_1, \dots, n_K$. 
Typically:
$$n_k \approx n/K, \quad k = 1, \dots, K$$

Let $\ell_{C_k}$ be the test loss when using $C_k$ as **test data** and all remaining data, denoted $\tau_{-k}$, as **training data**.

> [!note] Unbiasedness
>  Each $\ell_{C_k}$ is an **unbiased estimator** of the generalization risk for training set $\tau_{-k}$ — that is, for $\ell(g_{\tau_{-k}})$.

The **$K$-fold cross-validation loss** is:
$$\mathrm{CV}_K := \sum_{k=1}^{K} \frac{n_k}{n}  \ell_{C_k}(g_{\tau_{-k}}) = \frac{1}{n} \sum_{i=1}^{n} \mathrm{Loss}\big(g_{\tau_{-\kappa(i)}}(x_i) , y_i\big)$$

where $\kappa(i)$ indicates to which of the $K$ folds observation $i$ belongs.

> [!important] What does CV actually estimate? 
> Because the average is taken over **varying** training sets $\{\tau_{-k}\}$, it estimates the **expected generalization risk** $\mathbb{E}\ell(g_\tau)$ — **not** the generalization risk $\ell(g_\tau)$ for one particular training set $\tau$.

---

## 4. Polynomial Regression

A $K$-fold CV loss can be computed with a **nonrandom** partitioning of the training set.

```python
# polyregCV.py
from polyreg3 import *

K_vals = [5, 10, 100]              # number of folds
cv = np.zeros((len(K_vals), max_p)) # cv loss
X = np.ones((n, 1))
# ... see GitHub code ...
```

The figure plots CV loss vs. the number of parameters $p$ for $K \in {5, 10, 100}$.

> [!tip] Leave-one-out CV
>  Here $n = 100$, so the case **$K = 100$ corresponds to leave-one-out cross-validation (LOOCV)** — each fold is a single observation.

![[2 - Estimating Risk-1780688776978.webp]]
**Observations from the plot:**
- All three $K$ values agree and drop sharply as $p$ increases from 1 → ~4.
- For moderate $p$ the curves are flat and low (good fit).
- For large $p$ (overfitting region), the curves become **erratic and spike upward**, with LOOCV ($K=100$) and $K=10$ showing the largest, noisiest jumps.

---

## 5. Example — Automobile Data

**Goal:** compare **linear vs. higher-order polynomial** terms in a linear regression.
![[2 - Estimating Risk-1780688855653.webp]]

About the plot:
- The **392 observations** were randomly split into two equal sets: a **training set of 196** points and a **validation set of the remaining 196**.
- **Left panel:** a single train/validation split → noisy, split-dependent MSE curve.
- **Right panel:** multiple splits → the **variability across splits** is clearly visible (the single-split estimate is unstable).

### Auto data revisited (LOOCV vs. 10-fold)
![[2 - Estimating Risk-1780688909860.webp]]

| Method         | Behaviour                                                              |
| -------------- | ---------------------------------------------------------------------- |
| **LOOCV**      | Smooth MSE-vs-degree curve; deterministic (no randomness in the split) |
| **10-fold CV** | Very similar shape, slightly cheaper, small variability between runs   |

Both flatten out after roughly degree 2, indicating little gain from higher-order terms.

---

## 6. ⚠️ Cross-Validation: Right and Wrong

> [!danger] The classic trap 
> A common, serious mistake — seen in **many high-profile genomics papers**.

**The setup — a simple two-class classifier:**
1. Starting with **5000 predictors** and **50 samples**, find the **100 predictors** having the largest correlation with the class labels.
2. Apply a classifier (e.g. logistic regression) using **only these 100 predictors**.

**The tempting question:** _Can we apply cross-validation in step 2, forgetting about step 1?_

### NO! ❌

> [!failure] Why this is wrong
> 
> - This ignores that **Step 1 has already seen the labels** of the (full) data and used them. Feature selection is **a form of training** and must be inside the validation loop.
> - You can simulate realistic data where class labels are **independent of the outcome**, so the **true test error = 50%**, yet the CV error that ignores Step 1 comes out as **zero**.

> [!example] Try it yourself Simulate independent labels/features, run the "wrong" procedure, and watch the CV error collapse to ~0 despite chance-level true performance.

### The Wrong Way vs. The Right Way ✅

- **Wrong** ❌: Select predictors on the full data first, then apply CV only to step 2.
- **Right** ✅: Apply cross-validation to **both steps 1 and 2** — re-do feature selection **inside each fold**, using only that fold's training data.|

**Wrong way (leakage):**
![[2 - Estimating Risk-1780689481794.webp]]

**Right way (leak-free):**
![[2 - Estimating Risk-1780689504821.webp]]

---

## 7. Summary

> [!summary]
> 
> - **Test loss** estimates generalization risk but is wasteful of data and varies with the training set.
> - **$K$-fold CV** reuses data efficiently and estimates the **expected** generalization risk $\mathbb{E},\ell(g_\tau)$.
> - **LOOCV** is the special case $K = n$.
> - More folds → less bias but more variance / compute.
> - **Golden rule:** _any_ step that uses the labels (including feature selection) must live **inside** the CV loop, or the error estimate is invalid.
