# Tree-Based Methods

> [!abstract] Overview
> Tree-based methods are used for **regression** and **classification**. They work by **stratifying** or **segmenting** the predictor space into a number of simple regions. Because the set of splitting rules can be summarized in a tree, these are called **decision-tree methods**.

## Pros and Cons at a Glance

- Simple and very useful for **interpretation**.
- Typically **not competitive** with the best supervised learning approaches in terms of prediction accuracy.
- Fix: **bagging**, **random forests**, and **boosting** grow multiple trees and combine them into a single consensus prediction.
- Combining many trees can give **dramatic accuracy improvements**, at the cost of some loss of interpretability.

---

# 1. The Basics of Decision Trees

Decision trees apply to both regression and classification. We start with **regression**, then move to **classification**.

## Motivating Example — Baseball Salary Data

- Predicting `log(Salary)` of a baseball player from:
    - `Years` — number of years in the major leagues
    - `Hits` — number of hits in the previous year
- Salary is color-coded from low (blue/green) to high (yellow/red).
![[2 - Tree-Based Methods-1780857583967.webp]]
## The Resulting Regression Tree
The tree has **two internal nodes** and **three terminal nodes (leaves)**.
![[2 - Tree-Based Methods-1780857606407.webp|356]]

> [!note] Reading a split
> At an internal node, a label of the form `Xj < tk` means the **left branch** is `Xj < tk` and the **right branch** is `Xj >= tk`. 
> Example: the top split sends `Years < 4.5` left and `Years >= 4.5` right.

The number in each leaf = the **mean response** of the training observations that fall there.

## Resulting Regions

The tree segments players into **three regions** of predictor space:
![[2 - Tree-Based Methods-1780857706327.webp|355]]

| Region | Definition                                                  |
| ------ | ----------------------------------------------------------- |
| $R_1$  | $\{X \mid \text{Years} < 4.5\}$                             |
| $R_2$  | $\{X \mid \text{Years} \geq 4.5,\ \text{Hits} < 117.5\}$    |
| $R_3$  | $\{X \mid \text{Years} \geq 4.5,\ \text{Hits} \geq 117.5\}$ |
## Terminology

- **Terminal nodes / leaves** — the regions $R_1, R_2, R_3$ (drawn at the _bottom_; trees are drawn upside down).
- **Internal nodes** — points where the predictor space is split (e.g. `Years<4.5`, `Hits<117.5`).
- **Branches** — segments connecting nodes.

## Interpretation

- **Years** is the most important factor for Salary; less experienced players earn less.
- For inexperienced players, `Hits` plays little role.
- Among players with ≥5 years, more `Hits` last year → higher salary.
- An over-simplification, but **easy to display, interpret, and explain** compared to a regression model.

---

# 2. The Tree-Building Process

> [!info] Two core steps
> 
> 1. Divide the predictor space ($X_1, \dots, X_p$) into $J$ distinct, **non-overlapping** regions $R_1, \dots, R_J$.
> 2. For every observation in $R_j$, predict the **mean** of the response values of the training observations in $R_j$.

## Choosing the Regions: Minimizing RSS
Regions are chosen as high-dimensional **rectangles ("boxes")** for simplicity and interpretability. The goal is to minimize:
$$\sum_{j=1}^{J} \sum_{i \in R_j} (y_i - \hat{y}_{R_j})^2$$

where $\hat{y}_{R_j}$ is the mean response in box $j$.
## Recursive Binary Splitting
Considering every possible partition is **computationally infeasible**, so we use a **top-down, greedy** approach called **recursive binary splitting**.
- **Top-down** — begins at the top, then successively splits the predictor space; each split adds two new branches.
- **Greedy** — at each step the _best split at that step_ is made, without looking ahead to future splits.

> [!example] The procedure
> 
> 1. Select predictor $X_j$ and cutpoint $s$ splitting into $\{X \mid X_j < s\}$ and $\{X \mid X_j \geq s\}$ to give the **greatest reduction in RSS**.
> 2. Repeat, but split **one of the two existing regions** (now three regions).
> 3. Continue splitting regions to minimize RSS until a **stopping criterion** is reached (e.g. no region has more than 5 observations).

## Prediction

Predict a test observation using the **mean** of the training observations in the region it belongs to.

> [!warning] Not every partition is reachable
> A general partition of the feature space **cannot always result from recursive binary splitting** — the method only produces box-shaped (rectangular) regions.
![[2 - Tree-Based Methods-1780857867753.webp]]

---

# 3. Pruning a Tree

> [!question] Why does a large tree overfit? 
> The full-grown tree may give great training predictions but is **likely to overfit**, leading to poor test performance. A smaller tree (fewer regions) can lower **variance** and improve interpretation at the cost of a little **bias**.

## Why not just stop early?

Growing only while each split's RSS decrease exceeds a high threshold produces smaller trees, but is **too short-sighted**: a seemingly worthless split early may enable a very valuable split later.

## Cost Complexity Pruning (Weakest Link Pruning)
**Better strategy:** grow a large tree $T_0$, then **prune** it back to a subtree.

For a tuning parameter $\alpha \geq 0$, find the subtree $T \subset T_0$ minimizing:

$$\sum_{m=1}^{|T|} \sum_{i:, x_i \in R_m} (y_i - \hat{y}_{R_m})^2 + \alpha |T|$$

- $|T|$ = number of terminal nodes
- $R_m$ = region of the $m$-th terminal node
- $\hat{y}_{R_m}$ = mean of training observations in $R_m$
- $\alpha$ = controls the trade-off between **complexity** and **fit** to the training data.

## Choosing $\alpha$

- Select optimal $\hat{\alpha}$ via **cross-validation**.
- Return to the full dataset and take the subtree corresponding to $\hat{\alpha}$.

## Summary: Full Tree Algorithm

> [!algorithm] Algorithm
> 
> 1. Use **recursive binary splitting** to grow a large tree, stopping when each terminal node has fewer than some minimum number of observations.
> 2. Apply **cost complexity pruning** to get a sequence of best subtrees as a function of $\alpha$.
> 3. Use **K-fold cross-validation** to choose $\alpha$. For each fold $k = 1, \dots, K$:
> 
> 	a. Repeat steps 1–2 on the other $\frac{K-1}{K}$ of the data.
> 	b. Evaluate MSE on the held-out fold $k$ as a function of $\alpha$.
> 	c. Average results and pick $\alpha$ to minimize the average error.
> 
> 4. Return the subtree from step 2 corresponding to the chosen $\alpha$.

## Baseball Example (Pruning)

- Split the data: **132** training observations, **131** test observations.
- Grew a large tree, varied $\alpha$ to create subtrees of different sizes.
- Used **six-fold cross-validation** to estimate CV MSE as a function of $\alpha$.
- Plot of Tree Size vs MSE shows **Training**, **Cross-Validation**, and **Test** error curves — training error keeps dropping while CV/test error bottoms out at an intermediate tree size.
![[2 - Tree-Based Methods-1780858092623.webp|697]]
---

# 4. Classification Trees
Very similar to regression trees, but predict a **qualitative** response. Each observation is assigned to the **most commonly occurring class** of training observations in its region.

## Splitting Criteria
RSS cannot be used. Alternatives measure **node purity**.
### Classification Error Rate

$$E = 1 - \max_k (\hat{p}_{mk})$$

where $\hat{p}_{mk}$ = proportion of training observations in region $m$ from class $k$.

> [!warning] Not sensitive enough
> Classification error is **not sufficiently sensitive** for tree-growing; two other measures are preferred.

### Gini Index
$$G = \sum_{k=1}^{K} \hat{p}_{mk}(1 - \hat{p}_{mk})$$

- A measure of **total variance** across the $K$ classes.
- Small when all $\hat{p}_{mk}$ are near 0 or 1 → a measure of **node purity**.

### Cross-Entropy (Deviance)
$$D = -\sum_{k=1}^{K} \hat{p}_{mk} \log \hat{p}_{mk}$$

> [!note] Gini and cross-entropy are **numerically very similar**.

## Example — Heart Data

- Binary outcome `HD` for **303 patients** presenting with chest pain (`Yes`/`No` for heart disease via angiographic test).
- **13 predictors**: `Age`, `Sex`, `Chol`, plus other heart/lung function measures.
- Cross-validation yields a pruned tree with **six terminal nodes**.
- Trees can split on **qualitative predictors** directly (e.g. `Thal:a`, `ChestPain:bc`) — no dummy variables needed.
![[2 - Tree-Based Methods-1780858205038.webp|425]]
![[2 - Tree-Based Methods-1780858240095.webp|413]]

---

# 5. Trees vs. Linear Models

|Situation|Better Model|
|---|---|
|True boundary is **linear**|Linear model|
|True boundary is **non-linear / box-like**|Tree-based model|

Neither dominates; the best choice depends on the true decision boundary.

## Advantages and Disadvantages of Trees

> [!success] Advantages
> 
> - Very **easy to explain** — even easier than linear regression.
> - May **mirror human decision-making** more closely.
> - Can be **displayed graphically** and interpreted by non-experts (especially small trees).
> - Handle **qualitative predictors** without dummy variables.

> [!failure] Disadvantage
> 
> - Generally **lower predictive accuracy** than other approaches.
> - Fix: **aggregate many trees** (bagging, random forests, boosting).

---

# 6. Bagging (Bootstrap Aggregation)

> [!abstract] Idea 
> A general-purpose procedure for **reducing variance**. Averaging $n$ independent observations each with variance $\sigma^2$ gives a mean with variance $\sigma^2/n$ — averaging reduces variance.

Since we usually have only one training set, we **bootstrap**: take repeated samples from the single training set.
- Generate $B$ bootstrapped training sets.
- Train on the $b$-th set to get $\hat{f}^{*b}(x)$.
- Average predictions:

$$\hat{f}_{\text{bag}}(x) = \frac{1}{B} \sum_{b=1}^{B} \hat{f}^{*b}(x)$$

## Bagging Classification Trees

For each test observation, record the class predicted by each of the $B$ trees and take a **majority vote**.

## Out-of-Bag (OOB) Error Estimation

> [!info] Free test error estimate
> 
> - Each bagged tree uses, on average, about **two-thirds** of the observations.
> - The remaining **one-third** are the **out-of-bag (OOB)** observations.
> - Predict observation $i$ using only trees where it was OOB (~$B/3$ predictions), then average.
> - This is essentially **LOO cross-validation** error for bagging when $B$ is large.

---

# 7. Random Forests

> [!abstract] Key tweak 
> Like bagging, but **decorrelates** the trees, further reducing variance when averaging.

- Build trees on bootstrapped samples (as in bagging).
- **At each split**, only a **random selection of $m$ predictors** (out of $p$) is allowed as split candidates. A fresh selection is made at each split.
- Typically $m \approx \sqrt{p}$.
    - Heart data: $\sqrt{13} \approx 4$ of the 13 predictors.

> [!tip] Why it works 
> If one predictor is very strong, in bagging most trees split on it first → highly **correlated** trees. Restricting candidates forces variety, decorrelating the trees so averaging reduces variance more.

## Heart Data Results

- Test error shown vs $B$ (number of bootstrapped sets); random forests used $m = \sqrt{p}$.
- Dashed line = test error of a **single classification tree**.
- **OOB error** traces are considerably lower.

## Gene Expression Data

- Expression of **4,718 genes** on tissue from **349 patients** (~20,000 genes exist in humans).
- Each sample labelled with one of **15 levels**: normal or one of 14 cancer types.
- Used the **500 genes with the largest variance**; tried three values of $m$.
- Random forests ($m < p$) slightly improve on bagging ($m = p$).
- A single classification tree → **45.7%** error rate.

---

# 8. Boosting

> [!abstract] Idea 
>Like bagging, a general approach (here for decision trees), but trees are grown **sequentially** — each tree uses information from previously grown trees. No bootstrap.

## Boosting Algorithm (Regression Trees)

> [!algorithm] Algorithm
> 
> 1. Set $\hat{f}(x) = 0$ and $r_i = y_i$ for all $i$ (residuals = response).
> 2. For $b = 1, 2, \dots, B$:
> a. Fit a tree $\hat{f}^b$ with $d$ splits ($d+1$ terminal nodes) to the data $(X, r)$.
> b. Update $\hat{f}$ with a shrunken version of the new tree: $$\hat{f}(x) \leftarrow \hat{f}(x) + \lambda \hat{f}^b(x)$$
> c. Update residuals: $$r_i \leftarrow r_i - \lambda \hat{f}^b(x_i)$$
> 3. Output the boosted model: $$\hat{f}(x) = \sum_{b=1}^{B} \lambda \hat{f}^b(x)$$

## The Intuition — "Learn Slowly"
- Unlike fitting one large tree (which fits hard and may overfit), boosting **learns slowly**.
- Each step fits a tree to the **current residuals**, then adds it in to improve $\hat{f}$ where it does poorly.
- Trees are small (few terminal nodes, set by $d$).
- The **shrinkage parameter $\lambda$** slows learning further, letting many differently-shaped trees attack the residuals.


## Tuning Parameters

|Parameter|Role|Notes|
|---|---|---|
|**$B$** — number of trees|More trees|Boosting **can overfit** if $B$ is too large (slowly). Choose via cross-validation.|
|**$\lambda$** — shrinkage|Learning rate|Small positive number; typical values **0.01** or **0.001**. Small $\lambda$ needs large $B$.|
|**$d$** — splits per tree|Complexity / interaction depth|Often $d = 1$ (a **stump**) works well → additive model. $d$ splits involve at most $d$ variables.|
![[2 - Tree-Based Methods-1780859554330.webp]]
## Results

- **Gene expression data**: $\lambda = 0.01$. Depth-1 trees slightly outperform depth-2; both edge out random forest, though differences are **not significant** (SE ≈ 0.02). Single tree → **24%** error.
- **California Housing** & **Spam** examples (from _Elements of Statistical Learning_, Ch. 15) compare RF and GBM at various depths/$m$.

---

# 9. Variable Importance

A way to recover some interpretability from ensembles.

- **Regression (bagged/RF)**: record total RSS decrease due to splits over a given predictor, **averaged over all $B$ trees**. Large value → important predictor.
- **Classification (bagged/RF)**: add up total **Gini index** decrease from splits over a predictor, averaged over all $B$ trees.

> For the **Heart data**, the variable importance plot ranks (high → low): `Thal`, `Ca`, `ChestPain`, `Oldpeak`, `MaxHR`, `RestBP`, `Age`, `Chol`, `Slope`, `Sex`, `ExAng`, `RestECG`, `Fbs`.
> ![[2 - Tree-Based Methods-1780859585040.webp]]

---

# Summary

> [!summary] Chapter Takeaways
> 
> - Decision trees are **simple and interpretable** for regression and classification.
> - But they are **often not competitive** in prediction accuracy.
> - **Bagging, random forests, and boosting** improve accuracy by growing many trees and combining their predictions.
> - **Random forests** and **boosting** are among the **state-of-the-art** methods for supervised learning — but results are **harder to interpret**.

## Quick Comparison of Ensemble Methods

|Method|How trees are built|Key feature|Main benefit|
|---|---|---|---|
|**Bagging**|Independent, on bootstrap samples|Average / majority vote|Reduces variance|
|**Random Forest**|Independent, bootstrap + random $m$ predictors per split|**Decorrelates** trees|Reduces variance more|
|**Boosting**|**Sequential**, fit to residuals|Slow learning via $\lambda$|Reduces bias & variance|
