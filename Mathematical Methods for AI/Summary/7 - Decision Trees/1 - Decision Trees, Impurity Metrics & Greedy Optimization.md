### Tree-based Methods

> **Context:** Tree-based methods provide a simple, intuitive, and powerful mechanism for both regression and classification. The core idea is to divide a complex feature space into smaller regions and fit a simple prediction function to each region.

> [!definition] Tree-based Methods Principle
> 
> The feature space $\mathcal{X}$ is divided into smaller regions, each associated with a simple prediction function (typically constant).
> 
> - **Regression:** take the *mean* of the training responses falling in the region.
>     
> - **Classification:** take the *majority vote* among the corresponding response variables.
>     

### The Decision Tree Structure

> **Context:** Both the classification procedure and the partitioning of the feature space can be represented by a binary decision tree. Each node corresponds to a region (subset) of the feature space, and the tree simultaneously encodes the partition and the decision rules.

> [!definition] Binary Decision Tree
> 
> A binary tree $T$ in which each node $v$ corresponds to a region $\mathcal{R}_v \subseteq \mathcal{X}$.
> 
> - Each **internal node** $v$ contains a logical condition that divides $\mathcal{R}_v$ into two disjoint subregions.
>     
> - The regions of the **leaf nodes** form a partition of $\mathcal{X}$.
>     
> - Associated with each leaf node $w$ is a **regional prediction function** $g^w$ on $\mathcal{R}_w$.
>     

### Overall Prediction Function

> **Context:** A binary tree partitions the feature space into as many regions as there are leaf nodes. The global prediction function is obtained by combining the regional predictions via indicator functions that select the region of membership.

> [!definition] Global Prediction Function
> 
> Letting $\mathcal{W}$ be the set of leaf nodes, the overall prediction function is:
> 
> $$ g(\boldsymbol{x}) = \sum_{w \in \mathcal{W}} g^w(\boldsymbol{x})\, \mathbb{I}\{\boldsymbol{x} \in \mathcal{R}_w\} $$
> 
> where $\mathbb{I}$ is the indicator function. The representation depends on:
> 
> 1. **How the regions** $\{\mathcal{R}_w\}$ are constructed via the logical conditions — e.g. conditions $x_j \le \xi$ split the space into axis-aligned rectangles.
>     
> 2. **How the regional prediction functions** of the leaf nodes are defined — e.g. a constant function.
>     

### Training Loss

> **Context:** Constructing a tree on a training set amounts to minimizing the training loss. Thanks to the structure of disjoint regions, this loss can be decomposed as a sum of the contributions of each region, making the problem separable.

> [!definition] Training Loss and Decomposition
> 
> Given the training set $\tau = \{(\boldsymbol{x}_i, y_i)\}_{i=1}^n$, the training loss is:
> 
> $$ \ell_\tau(g) = \frac{1}{n}\sum_{i=1}^n \text{Loss}(y_i, g(\boldsymbol{x}_i)) $$
> 
> With $g$ in the region-based form, it decomposes per region:
> 
> $$ \ell_\tau(g) = \sum_{w \in \mathcal{W}} \frac{1}{n}\sum_{i=1}^n \mathbb{I}\{\boldsymbol{x}_i \in \mathcal{R}_w\}\,\text{Loss}(y_i, g^w(\boldsymbol{x}_i)) $$
> 
> where each term is the contribution of the regional prediction function $g^w$ to the overall loss.

### Top-Down Construction

> **Context:** Finding a tree with zero training loss is easy, but it produces an overfitted tree with poor predictive ability. For this reason the search is restricted to a class of trees, and the training loss is minimized with a greedy top-down approach based on splitting rules.

> [!definition] Splitting Rule and Data Subsets
> 
> The key to constructing the tree is to specify a **splitting rule** for each node $v$:
> 
> $$ s : \mathcal{X} \to \{\text{False}, \text{True}\} \quad (\text{equiv.} \;\; \{0, 1\}) $$
> 
> Each node $v$ is associated with a region $\mathcal{R}_v$ and the training subset contained in it. Given a rule $s$, any subset $\sigma \subseteq \tau$ is divided into:
> 
> $$ \sigma_T := \{(\boldsymbol{x}, y) \in \sigma : s(\boldsymbol{x}) = \text{True}\} \qquad \sigma_F := \{(\boldsymbol{x}, y) \in \sigma : s(\boldsymbol{x}) = \text{False}\} $$
> 
> The final tree is built recursively starting from the root $v_0$.

### `Construct_Subtree` Algorithm

> **Context:** The tree is built recursively: at each node either the termination criterion is applied (creating a leaf), or the best splitting rule is found and the data is partitioned, recursively calling the algorithm on the two resulting branches.

> [!algorithm] Construct_Subtree 
> **Input:** A node $v$ and a subset of the training data $\sigma \subseteq \tau$. **Output:** A (sub) decision tree $T_v$.
> 
> 1. **if** termination criterion is met **then** _( $v$ is a leaf node)_
> 2.      Train a regional prediction function $g^v$ using training data $\sigma$.
> 3. **else** _(split the node)_
> 4.      Find the best splitting rule $s_v$ for node $v$.
> 5.      Create successors $v_T$ and $v_F$ of $v$.
> 6.      $\sigma_T \leftarrow {(\boldsymbol{x}, y) \in \sigma : s_v(\boldsymbol{x}) = \text{True}}$
> 7.      $\sigma_F \leftarrow {(\boldsymbol{x}, y) \in \sigma : s_v(\boldsymbol{x}) = \text{False}}$
> 8.      $T_{v_T} \leftarrow \texttt{Construct\_Subtree}(v_T, \sigma_T)$ _(left branch)_
> 9.      $T_{v_F} \leftarrow \texttt{Construct\_Subtree}(v_F, \sigma_F)$ _(right branch)_
> 10. **return** $T_v$
> 11. 
> To implement the algorithm three things must be specified: the **regional prediction functions** at the leaves (Line 2), the **splitting rule** (Line 4), and the **termination criterion** (Line 1).

### Regional Prediction Functions

> **Context:** At the leaves the prediction functions are kept very simple (constant). The form of the constant depends on the type of problem: most frequent class for classification, mean of the responses for regression.

> [!definition] Regional Prediction for Classification and Regression
> 
> Let $n_w$ be the number of feature vectors in $\mathcal{R}_w$ and $p_z^w$ the proportion of class $z$ in the region.
> 
> - **Classification:** $g^w$ is the most common class in the region (ties broken randomly):
>     
>     $$ g^w(\boldsymbol{x}) = \operatorname*{argmax}_{z \in \{0, \dots, c-1\}} p_z^w $$
>     
> - **Regression:** $g^w$ is the mean response in the region:
>     
>     $$ g^w(\boldsymbol{x}) = \overline{y}_{\mathcal{R}_w} := \frac{1}{n_w}\sum_{\{(\boldsymbol{x}, y) : \boldsymbol{x} \in \mathcal{R}_w\}} y $$
>     
>     The mean minimizes the squared-error loss among all constant functions within the region.
>     

### Splitting Rules and Loss Reduction

> **Context:** To decide whether a split is worthwhile, the loss contribution of the node kept as a leaf is compared with that of the split node. Since splitting cannot increase the loss, the greedy heuristic chooses the split that minimizes it as much as possible.

> [!definition] Greedy Splitting Heuristic
> 
> Compare the loss contribution if $v$ is kept as a **leaf** (5) with that if $v$ is **split** (6), pretending the algorithm terminates immediately after the split (so $v_T$, $v_F$ are leaves and $g^T$, $g^F$ are easily evaluated).
> 
> - For any splitting rule: contribution (leaf) $\ge$ contribution (split).
>     
> - **Therefore choose the splitting rule that minimizes the post-split loss.**
>     
> - The termination criterion may compare the two contributions: if the difference is too small, splitting is not worthwhile.
>     

### Optimal Splitting Rule

> **Context:** For a Euclidean feature space, "standard" splitting rules are considered: thresholds on a single coordinate. The optimal pair (coordinate, threshold) is found by exhaustive search over the observed feature values.

> [!definition] Standard Splitting Rules
> 
> For $\mathcal{X} = \mathbb{R}^p$, rules of the form are considered:
> 
> $$ s(\boldsymbol{x}) = \mathbb{I}\{x_j \le \xi\}, \qquad 1 \le j \le p,\; \xi \in \mathbb{R} $$
> 
> Choose $j$ and $\xi$ to minimize the post-split loss.
> 
> - **Regression** (squared-error loss, constant prediction): minimize
>     
>     $$ \frac{1}{n}\sum_{x_j \le \xi} (y - \overline{y}_T)^2 + \frac{1}{n}\sum_{x_j > \xi} (y - \overline{y}_F)^2 $$
>     
> - **Classification** (zero–one loss): minimize
>     
>     $$ \frac{1}{n}\sum_{\sigma_T} \mathbb{I}\{y \ne y_T^*\} + \frac{1}{n}\sum_{\sigma_F} \mathbb{I}\{y \ne y_F^*\} $$
>     
>     where $y_T^*$, $y_F^*$ are the majority classes in $\sigma_T$, $\sigma_F$.
>     
> - **Search procedure:** letting $\{x_{j,k}\}_{k=1}^m$ be the possible values of $x_j$ in $\sigma$, evaluate the loss for each of the $m \times p$ pairs and choose the minimizing one.
>     

### Impurities

> **Context:** Minimizing the zero–one classification loss is equivalent to minimizing a weighted average of the node impurities. Impurity measures the diversity of labels in a subset and depends only on the class proportions.

> [!definition] Impurity Measures
> 
> For a subset $\sigma$ with most prevalent label $y^*$, the misclassification impurity is:
> 
> $$ 1 - \max_{z \in \{0,\dots,c-1\}} p_z $$
> 
> The classification loss is the weighted sum of the impurities of $\sigma_T$ and $\sigma_F$, with weights $|\sigma_T|/n$ and $|\sigma_F|/n$. Other measures depending only on the proportions $p_z$:
> 
> | **Measure** | **Formula** |
> | --- | --- |
> | **Misclassification** | $1 - \max_z p_z$ |
> | **Entropy** | $-\sum_{z=0}^{c-1} p_z \log_2(p_z)$ |
> | **Gini** | $\frac{1}{2}\left(1 - \sum_{z=0}^{c-1} p_z^2\right)$ |
> 
> All are **maximal when the proportions equal** $1/c$. In the binary case, entropy and Gini are smooth, misclassification is piecewise-linear; after normalization they attain the same maximum $1/2$ at $p=1/2$ and equal $0$ at pure nodes ($p=0$ or $p=1$).

### Termination Criterion

> **Context:** The ultimate quality of a tree is its predictive performance (generalization risk). The stopping criterion must balance the approximation error against the statistical error, controlling overfitting.

> [!definition] Stopping Conditions
> 
> Common conditions to stop the growth of the tree:
> 
> - Stop when the node size $|\sigma|$ is $\le$ a predefined threshold.
>     
> - Choose the **maximal tree depth** in advance.
>     
> - Stop when there is **no significant advantage** in terms of loss in splitting.
>     
> 
> The criterion must balance **approximation error** and **statistical error** (bias–variance trade-off). Using the cross-validation loss as a proxy for the generalization risk, the loss drops, reaches a minimum, then rises again as depth increases (overfitting).
