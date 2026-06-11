### Tree-Based Methods Overview

> **Context:** Tree-based methods are used for both regression and classification. They work by stratifying or segmenting the predictor space into a number of simple regions. Because the set of splitting rules can be summarized in a tree, these are called decision-tree methods.

> [!definition] Nature of Tree-Based Methods
> 
> - Simple and very useful for **interpretation**.
>     
> - Typically **not competitive** with the best supervised learning approaches in terms of prediction accuracy.
>     
> - **Bagging**, **random forests**, and **boosting** grow multiple trees and combine them into a single consensus prediction, giving dramatic accuracy improvements at the cost of some interpretability.
>     

### The Tree-Building Process

> **Context:** Building a regression tree consists of two core steps: partitioning the predictor space into non-overlapping regions, and predicting a constant (the mean) within each region. The regions are chosen to minimize the residual sum of squares.

> [!definition] Two Core Steps
> 
> 1. Divide the predictor space $(X_1, \dots, X_p)$ into $J$ distinct, **non-overlapping** regions $R_1, \dots, R_J$.
>     
> 2. For every observation in $R_j$, predict the **mean** of the response values of the training observations in $R_j$.
>     
> 
> Regions are chosen as high-dimensional **rectangles ("boxes")** for simplicity and interpretability, minimizing:
> 
> $$\sum_{j=1}^{J} \sum_{i \in R_j} (y_i - \hat{y}_{R_j})^2$$
> 
> where $\hat{y}_{R_j}$ is the mean response in box $j$.

### Recursive Binary Splitting

> **Context:** Considering every possible partition of the feature space is computationally infeasible, so a top-down, greedy approach called recursive binary splitting is used. The method only produces box-shaped regions, so not every general partition is reachable.

> [!definition] Recursive Binary Splitting
> 
> - **Top-down** — begins at the top, then successively splits the predictor space; each split adds two new branches.
>     
> - **Greedy** — at each step the *best split at that step* is made, without looking ahead to future splits.
>     
> 
> **Procedure:**
> 
> 1. Select predictor $X_j$ and cutpoint $s$ splitting into $\{X \mid X_j < s\}$ and $\{X \mid X_j \geq s\}$ to give the **greatest reduction in RSS**.
>     
> 2. Repeat, splitting **one of the existing regions** to minimize RSS.
>     
> 3. Continue until a **stopping criterion** is reached (e.g. no region has more than 5 observations).
>     
> 
> Prediction for a test observation uses the **mean** of the training observations in the region it belongs to.

### Pruning a Tree

> **Context:** A full-grown tree gives great training predictions but is likely to overfit, leading to poor test performance. A smaller tree can lower variance and improve interpretation at the cost of a little bias. Stopping early is too short-sighted, since a seemingly worthless split may enable a valuable split later; instead, a large tree is grown and then pruned.

> [!definition] Cost Complexity Pruning (Weakest Link Pruning)
> 
> Grow a large tree $T_0$, then prune it back to a subtree. For a tuning parameter $\alpha \geq 0$, find the subtree $T \subset T_0$ minimizing:
> 
> $$\sum_{m=1}^{|T|} \sum_{i: x_i \in R_m} (y_i - \hat{y}_{R_m})^2 + \alpha |T|$$
> 
> - $|T|$ = number of terminal nodes.
>     
> - $R_m$ = region of the $m$-th terminal node.
>     
> - $\hat{y}_{R_m}$ = mean of training observations in $R_m$.
>     
> - $\alpha$ = controls the trade-off between **complexity** and **fit** to the training data.
>     
> 
> The optimal $\hat{\alpha}$ is selected via cross-validation, then the subtree corresponding to $\hat{\alpha}$ is taken on the full dataset.

> [!algorithm] Full Tree Algorithm
> 
> 1. Use **recursive binary splitting** to grow a large tree, stopping when each terminal node has fewer than some minimum number of observations.
>     
> 2. Apply **cost complexity pruning** to get a sequence of best subtrees as a function of $\alpha$.
>     
> 3. Use **K-fold cross-validation** to choose $\alpha$. For each fold $k = 1, \dots, K$:
>     
>     a. Repeat steps 1–2 on the other $\frac{K-1}{K}$ of the data.
>     
>     b. Evaluate MSE on the held-out fold $k$ as a function of $\alpha$.
>     
>     c. Average results and pick $\alpha$ to minimize the average error.
>     
> 4. Return the subtree from step 2 corresponding to the chosen $\alpha$.

### Classification Trees

> **Context:** Classification trees are very similar to regression trees but predict a qualitative response. Each observation is assigned to the most commonly occurring class of training observations in its region. Since RSS cannot be used, alternative measures of node purity are needed.

> [!definition] Splitting Criteria for Classification
> 
> Let $\hat{p}_{mk}$ be the proportion of training observations in region $m$ from class $k$.
> 
> - **Classification Error Rate:**
>     
>     $$E = 1 - \max_k (\hat{p}_{mk})$$
>     
>     Not sufficiently sensitive for tree-growing; two other measures are preferred.
>     
> - **Gini Index:**
>     
>     $$G = \sum_{k=1}^{K} \hat{p}_{mk}(1 - \hat{p}_{mk})$$
>     
>     A measure of total variance across the $K$ classes; small when all $\hat{p}_{mk}$ are near 0 or 1 → a measure of **node purity**.
>     
> - **Cross-Entropy (Deviance):**
>     
>     $$D = -\sum_{k=1}^{K} \hat{p}_{mk} \log \hat{p}_{mk}$$
>     
>     Numerically very similar to the Gini index.
>     
> 
> Trees can split on **qualitative predictors** directly, without creating dummy variables.

### Trees vs. Linear Models

> **Context:** Neither trees nor linear models dominate universally; the best choice depends on the true decision boundary. Trees also carry distinctive interpretability advantages but lower predictive accuracy.

> [!definition] Comparison and Trade-offs
> 
> | Situation | Better Model |
> | --- | --- |
> | True boundary is **linear** | Linear model |
> | True boundary is **non-linear / box-like** | Tree-based model |
> 
> **Advantages of trees:** very easy to explain (even more than linear regression); may mirror human decision-making; can be displayed graphically and interpreted by non-experts; handle qualitative predictors without dummy variables.
> 
> **Disadvantage:** generally lower predictive accuracy, addressed by aggregating many trees (bagging, random forests, boosting).

### Bagging (Bootstrap Aggregation)

> **Context:** Bagging is a general-purpose procedure for reducing variance. Averaging many independent observations reduces variance; since we usually have only one training set, we bootstrap by taking repeated samples from it, then average the resulting predictions.

> [!definition] Bagging
> 
> - Generate $B$ bootstrapped training sets.
>     
> - Train on the $b$-th set to get $\hat{f}^{*b}(x)$.
>     
> - Average predictions:
>     
>     $$\hat{f}_{\text{bag}}(x) = \frac{1}{B} \sum_{b=1}^{B} \hat{f}^{*b}(x)$$
>     
> - **Classification:** record the class predicted by each of the $B$ trees and take a **majority vote**.
>     

> [!definition] Out-of-Bag (OOB) Error Estimation
> 
> - Each bagged tree uses, on average, about **two-thirds** of the observations.
>     
> - The remaining **one-third** are the **out-of-bag (OOB)** observations.
>     
> - Predict observation $i$ using only trees where it was OOB (~$B/3$ predictions), then average.
>     
> - This is essentially **LOO cross-validation** error for bagging when $B$ is large.
>     

### Random Forests

> **Context:** Random forests are like bagging but decorrelate the trees, further reducing variance when averaging. If one predictor is very strong, in bagging most trees split on it first, producing highly correlated trees; restricting the candidate predictors at each split forces variety.

> [!definition] Random Forests
> 
> - Build trees on bootstrapped samples (as in bagging).
>     
> - **At each split**, only a **random selection of $m$ predictors** (out of $p$) is allowed as split candidates; a fresh selection is made at each split.
>     
> - Typically $m \approx \sqrt{p}$.
>     
> - Restricting candidates decorrelates the trees, so averaging reduces variance more than bagging.
>     

### Boosting

> **Context:** Boosting is a general approach (here for decision trees) where trees are grown sequentially — each tree uses information from previously grown trees, with no bootstrap. Instead of fitting one large tree hard (which may overfit), boosting learns slowly by repeatedly fitting small trees to the current residuals.

> [!algorithm] Boosting Algorithm (Regression Trees)
> 
> 1. Set $\hat{f}(x) = 0$ and $r_i = y_i$ for all $i$ (residuals = response).
>     
> 2. For $b = 1, 2, \dots, B$:
>     
>     a. Fit a tree $\hat{f}^b$ with $d$ splits ($d+1$ terminal nodes) to the data $(X, r)$.
>     
>     b. Update with a shrunken version of the new tree: $\hat{f}(x) \leftarrow \hat{f}(x) + \lambda \hat{f}^b(x)$
>     
>     c. Update residuals: $r_i \leftarrow r_i - \lambda \hat{f}^b(x_i)$
>     
> 3. Output the boosted model: $\hat{f}(x) = \sum_{b=1}^{B} \lambda \hat{f}^b(x)$
>     

> [!definition] Boosting Tuning Parameters
> 
> | Parameter | Role | Notes |
> | --- | --- | --- |
> | **$B$** — number of trees | More trees | Boosting can overfit (slowly) if $B$ is too large. Choose via cross-validation. |
> | **$\lambda$** — shrinkage | Learning rate | Small positive number; typical values 0.01 or 0.001. Small $\lambda$ needs large $B$. |
> | **$d$** — splits per tree | Complexity / interaction depth | Often $d = 1$ (a **stump**) works well → additive model. $d$ splits involve at most $d$ variables. |

### Variable Importance

> **Context:** Aggregating many trees sacrifices the interpretability of a single tree. Variable importance measures recover some of it by quantifying how much each predictor contributes to reducing the splitting criterion, averaged across all trees.

> [!definition] Variable Importance
> 
> - **Regression (bagged/RF):** record the total RSS decrease due to splits over a given predictor, averaged over all $B$ trees. A large value indicates an important predictor.
>     
> - **Classification (bagged/RF):** add up the total **Gini index** decrease from splits over a predictor, averaged over all $B$ trees.
>     

### Summary of Ensemble Methods

> **Context:** Decision trees are simple and interpretable but often not competitive in prediction accuracy. Bagging, random forests, and boosting improve accuracy by growing and combining many trees; random forests and boosting are among the state-of-the-art supervised learning methods, at the cost of interpretability.

> [!definition] Comparison of Ensemble Methods
> 
> | Method | How trees are built | Key feature | Main benefit |
> | --- | --- | --- | --- |
> | **Bagging** | Independent, on bootstrap samples | Average / majority vote | Reduces variance |
> | **Random Forest** | Independent, bootstrap + random $m$ predictors per split | **Decorrelates** trees | Reduces variance more |
> | **Boosting** | **Sequential**, fit to residuals | Slow learning via $\lambda$ | Reduces bias & variance |
