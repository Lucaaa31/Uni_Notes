# Decision Trees

> [!abstract] 
> Purpose Statistical learning methods based on decision trees are popular for their **simplicity**, **intuitive representation**, and **predictive accuracy**. This lecture covers:
> 
> - Decision trees for classification
> - Construction of decision trees
> - Implementation of decision trees

---

## Tree-based Methods

Tree-based methods provide a simple, intuitive, and powerful mechanism for both **regression** and **classification**.

The main idea is to divide a (potentially complicated) feature space $\mathcal{X}$ into smaller regions and fit a **simple prediction function** to each region.

- **Regression setting** → take the _mean_ of the training responses for features that fall in a region.
- **Classification setting** → take the _majority vote_ among the corresponding response variables.

---

## Decision Tree for Classification

Consider a training set of **15 two-dimensional points** falling into two classes (red and blue). The question: _how should a new feature vector (black point) be classified?_

We can partition the feature space $\mathcal{X} = \mathbb{R}^2$ into **rectangular regions** and assign a class (color) to each region.

The partition defines a classifier (prediction function) $g$ that assigns to each feature vector $\boldsymbol{x}$ a class "red" or "blue".
![[1 - Decision Trees-1780855597465.webp|479]]

> [!example] For $\boldsymbol{x} = [-15, 0]^\top$ (the solid black point), $g(\boldsymbol{x}) = \text{"blue"}$, since it falls inside a blue region of the feature space.

---

## The Decision Tree Structure

Both the classification procedure and the partitioning of the feature space can be represented by a **binary decision tree**, where each node $v$ corresponds to a region (subset) $\mathcal{R}_v$ of the feature space $\mathcal{X}$.

![[1 - Decision Trees-1780855636871.webp|163]]

> [!note] Node properties
> 
> - Each **internal node** $v$ contains a logical condition that divides $\mathcal{R}_v$ into two **disjoint subregions**.
> - The regions of the **leaf nodes** form a partition of $\mathcal{X}$.
> - Associated with each leaf node $w$ is a **regional prediction function** $g^w$ on $\mathcal{R}_w$.

### Example traversal

For input $\boldsymbol{x} = [x_1, x_2]^\top = [-15, 0]^\top$:
![[1 - Decision Trees-1780855657804.webp|206]]
1. $x_2 \le 12.0$? → **True** (0 ≤ 12)
2. $x_2 \le -20.5$? → **False** (0 > −20.5)
3. $x_1 \le 20.0$? → **True** (−15 ≤ 20)
4. $x_1 \le 2.5$? → **True** (−15 ≤ 2.5)
5. $x_1 \le -5.0$? → **True** (−15 ≤ −5) → leaf = **blue** ✅

---

## Overall Prediction Function

A binary tree $T$ partitions the feature space $\mathcal{X}$ into as many regions as there are leaf nodes. Denote the set of leaf nodes by $\mathcal{W}$.

The overall prediction function $g$ corresponding to the tree is:

$$ g(\boldsymbol{x}) = \sum_{w \in \mathcal{W}} g^w(\boldsymbol{x}) \mathbb{I}\{\boldsymbol{x} \in \mathcal{R}_w\} \tag{1} $$

where $\mathbb{I}$ denotes the **indicator function**.

This representation depends on two things:

1. **How the regions** $\{\mathcal{R}_w\}$ are constructed via the logical conditions — e.g. conditions $x_j \le \xi$ split a Euclidean feature space into **axis-aligned rectangles**.
2. **How the regional prediction functions** of the leaf nodes are defined — e.g. a _constant_ function.

---

## Training Loss
Constructing a tree with a training set $\tau = {(\boldsymbol{x}_i, y_i)}_{i=1}^n$ amounts to minimizing the **training loss**:
$$ \ell_\tau(g) = \frac{1}{n}\sum_{i=1}^n \text{Loss}(y_i, g(\boldsymbol{x}_i)) \tag{2} $$

With $g$ of the form (1), this can be decomposed per region:

$$ \ell_\tau(g) = \sum_{w \in \mathcal{W}} \underbrace{\frac{1}{n}\sum_{i=1}^n \mathbb{I}\{\boldsymbol{x}_i \in \mathcal{R}_w\},\text{Loss}(y_i, g^w(\boldsymbol{x}_i))}_{(*)} $$

where $(*)$ is the contribution of regional prediction function $g^w$ to the overall training loss.

---

## Top-Down Construction of Decision Trees

> [!warning] Overfitting 
> Finding a tree with **zero** squared-error or zero–one training loss is easy, but such an _overfitted_ tree has poor predictive behavior.

Instead, restrict to a class of decision trees and minimize training loss within it, using a **top-down greedy approach**.

The key to constructing a binary tree $T$ is to specify a **splitting rule** for each node $v$: $$ s : \mathcal{X} \to \{\text{False}, \text{True}\} \quad \text{equivalently} \quad s : \mathcal{X} \to \{0, 1\} $$
> [!example] In the example tree, the root has splitting rule $\boldsymbol{x} \mapsto \mathbb{I}\{x_1 \le 12.0\}$, corresponding to the logical condition $\{x_1 \le 12.0\}$.

### Splitting Rule and Data Subsets
Each node $v$ is associated with a region $\mathcal{R}_v \subseteq \mathcal{X}$ and the training subset ${(\boldsymbol{x}, y) \in \tau : \boldsymbol{x} \in \mathcal{R}_v} \subseteq \tau$.

Using a splitting rule $s$, divide any subset $\sigma \subseteq \tau$ into two sets:
$$ \sigma_T := \{(\boldsymbol{x}, y) \in \sigma : s(\boldsymbol{x}) = \text{True}\} \qquad \sigma_F := \{(\boldsymbol{x}, y) \in \sigma : s(\boldsymbol{x}) = \text{False}\} $$

The final tree is built recursively: $T = \texttt{Construct\_Subtree}(v_0, \tau)$, where $v_0$ is the root.

---

## Algorithm 1: `Construct_Subtree`

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

### Three things to specify
The splitting rule $s_v$ divides $\mathcal{R}_v$ into disjoint parts $\mathcal{R}_{v_T}$ and $\mathcal{R}_{v_F}$, with:
$$ g^v(\boldsymbol{x}) = g^T(\boldsymbol{x})\mathbb{I}\{\boldsymbol{x} \in \mathcal{R}_{v_T}\} + g^F(\boldsymbol{x})\mathbb{I}\{\boldsymbol{x} \in \mathcal{R}_{v_F}\}, \quad \boldsymbol{x} \in \mathcal{R}_v $$

To implement Algorithm 1 we must address:
1. **Regional prediction functions** $g^v$ at the leaves (Line 2) — kept very simple in practice.
2. **The splitting rule** (Line 4).
3. **The termination criterion** (Line 1).

---

## Regional Prediction Functions

### For Classification
With class labels $0, \dots, c-1$, the regional prediction function $g^w$ is constant and equals the **most common class label** in region $\mathcal{R}_w$ (ties broken randomly).

Let $n_w$ be the number of feature vectors in $\mathcal{R}_w$, and define the class proportion:
$$ p_z^w = \frac{1}{n_w}\sum_{\{(\boldsymbol{x}, y) \in \tau : \boldsymbol{x} \in \mathcal{R}_w\}} \mathbb{I}{\{y = z\}} $$

Then the regional prediction function is:
$$ g^w(\boldsymbol{x}) = \operatorname*{argmax}_{\{z \in {0, \dots, c-1}\}} p_z^w \tag{3} $$
### For Regression
$g^w$ is the **mean response** in the region:

$$ g^w(\boldsymbol{x}) = \overline{y}_{\mathcal{R}_w} := \frac{1}{n_w}\sum_{\{(\boldsymbol{x}, y) \in \tau : \boldsymbol{x} \in \mathcal{R}_w\}} y \tag{4} $$

> [!tip] The mean $\overline{y}_{\mathcal{R}_w}$ **minimizes the squared-error loss** over all constant functions within region $\mathcal{R}_w$.

---

## Splitting Rules & Loss Reduction

When splitting region $\mathcal{R}_v$ into $\sigma_T$ and $\sigma_F$, what is the benefit in terms of reduced training loss?

**If $v$ is kept as a leaf**, its contribution to training loss: $$ \frac{1}{n}\sum_{i=1}^n \mathbb{I}_{(\boldsymbol{x}, y) \in \sigma},\text{Loss}(y_i, g^v(\boldsymbol{x}_i)) \tag{5} $$
**If $v$ is split instead**: $$ \frac{1}{n}\sum_{i=1}^n \mathbb{I}_{(\boldsymbol{x}, y) \in \sigma_T}\text{Loss}(y_i, g^T(\boldsymbol{x}_i)) + \frac{1}{n}\sum_{i=1}^n \mathbb{I}
_{(\boldsymbol{x}, y) \in \sigma_F}\text{Loss}(y_i, g^F(\boldsymbol{x}_i)) \tag{6} $$
### Greedy Rule

> [!important] Greedy heuristic 
> Pretend the algorithm terminates immediately after the split, so $v_T$ and $v_F$ are leaves and $g^T$, $g^F$ are easily evaluated.
> 
> - For **any** splitting rule, contribution (5) $\ge$ (6).
> - Therefore: **choose the splitting rule that minimizes (6)**.
> - The termination criterion may compare (6) with (5): if the difference is too small, splitting is not worthwhile.

---

## Optimal Splitting Rule

For feature space $\mathcal{X} = \mathbb{R}^p$, consider **standard splitting rules**:

$$ s(\boldsymbol{x}) = \mathbb{I}\{x_j \le \xi\} \tag{7} $$

for some $1 \le j \le p$ and $\xi \in \mathbb{R}$ (with $0 \equiv$ False, $1 \equiv$ True). We choose $j$ and $\xi$ to minimize (6).

### For Regression

Using squared-error loss and constant predictions, minimize:

$$ \frac{1}{n}\sum_{(\boldsymbol{x}, y) \in \tau : x_j \le \xi} (y - \overline{y}_T)^2 + \frac{1}{n}\sum_{(\boldsymbol{x}, y) \in \tau : x_j > \xi} (y - \overline{y}_F)^2 \tag{8} $$

where $\overline{y}_T$ and $\overline{y}_F$ are the average responses for $\sigma_T$ and $\sigma_F$.

> [!note] Search procedure
> Let $\{x_{j,k}\}_{k=1}^m$ be the possible values of $x_j$ in subset $\sigma$ (with $m \le n$). To minimize (8) over all $j, \xi$, evaluate (8) for each of the $m \times p$ values $x_{j,k}$ and pick the minimizing pair $(j, x_{j,k})$.

### For Classification

Using indicator (zero–one) loss and constant predictions, minimize:

$$ \frac{1}{n}\sum_{(\boldsymbol{x}, y) \in \sigma_T} \mathbb{I}\{y \ne y_T^*\} + \frac{1}{n}\sum_{(\boldsymbol{x}, y) \in \sigma_F} \mathbb{I}\{y \ne y_F^*\} \tag{9} $$

where $y_T^* = g^T(\boldsymbol{x})$ is the **majority class** in $\sigma_T$ and $y_F^*$ the majority class in $\sigma_F$.

The optimal standard splitting rule is found the same way as in regression, just replacing (8) with (9).

---

## Impurities

Minimizing (9) is equivalent to minimizing a **weighted average of node impurities**.

For an arbitrary subset $\sigma \subseteq \tau$ with most prevalent label $y^*$:

$$ \frac{1}{|\sigma|}\sum_{(\boldsymbol{x}, y) \in \sigma} \mathbb{I}\{y \ne y^*\} = 1 - p_{y^*} = 1 - \max_{z \in {0, \dots, c-1}} p_z $$

> [!definition] Misclassification impurity 
> $$1 - \max_{z \in {0,\dots,c-1}} p_z$$ Measures the **diversity of labels** in $\sigma$.

So (9) is the weighted sum of misclassification impurities of $\sigma_T$ and $\sigma_F$, weighted by $|\sigma_T|/n$ and $|\sigma_F|/n$.

### Other Impurity Measures

These also depend only on the label proportions $p_z$:
- **Misclassification**
$$1 - \max_z p_z$$
- **Entropy**
$$-\sum_{z=0}^{c-1} p_z \log_2(p_z)$$
- **Gini**
$$\dfrac{1}{2}\left(1 - \sum_{z=0}^{c-1} p_z^2\right)$$

> [!info] All these impurities are **maximal when label proportions equal** $1/c$.

### Comparison (Binary Case)

For two labels with class probabilities $p$ and $1-p$, the three impurity curves have similar shapes. Cross-entropy and Gini are smooth, while misclassification is piecewise-linear (a "tent").

After normalizing entropy (dividing by 2), all three attain the same maximum value of $1/2$ at $p = 1/2$, and equal $0$ at $p = 0$ and $p = 1$ (pure nodes).
![[1 - Decision Trees-1780856626401.webp|346]]

---
## Termination Criterion

Common stopping conditions when building a tree:
- Stop when the node size ($|\sigma|$) is $\le$ some predefined number.
- Choose the **maximal tree depth** in advance.
- Stop when there is **no significant training-loss advantage** to splitting.

> [!important] Bias–variance balance
> The ultimate quality of a tree is its **predictive performance (generalization risk)**. The termination condition should balance minimizing the **approximation error** against the **statistical error**.

### Example: Fixed Tree Depth

Using ten-fold cross-validation loss as a proxy for generalization risk, loss drops sharply, reaches a minimum, then rises again as depth increases (overfitting). In the lecture example, the **optimal maximal tree depth is 6**.
![[1 - Decision Trees-1780856674982.webp|448]]

---

## Python Implementation (`TreeDepthCV.py`)

Uses `make_blobs` from `sklearn` to produce a training set of size $n = 5000$ with ten-dimensional feature vectors ($p = 10$, $\mathcal{X} = \mathbb{R}^{10}$), each classified into one of $c = 3$ classes.

```python
import numpy as np
from sklearn.datasets import make_blobs
from sklearn.model_selection import cross_val_score
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import zero_one_loss
import matplotlib.pyplot as plt

def ZeroOneScore(clf, X, y):
    y_pred = clf.predict(X)
    return zero_one_loss(y, y_pred)

# Construct the training set
X, y = make_blobs(n_samples=5000, n_features=10, centers=3,
                  random_state=10, cluster_std=10)

# Construct a decision tree classifier
clf = DecisionTreeClassifier(random_state=0)

# Cross-validation loss as a function of tree depth (1 to 30)
xdepthlist = []
cvlist = []
tree_depth = range(1, 30)
for d in tree_depth:
    xdepthlist.append(d)
    clf.max_depth = d
    cv = np.mean(cross_val_score(clf, X, y, cv=10, scoring=ZeroOneScore))
    cvlist.append(cv)

plt.xlabel('tree depth', fontsize=18, color='black')
plt.ylabel('loss', fontsize=18, color='black')
plt.plot(xdepthlist, cvlist, '-*', linewidth=0.5)
```

---

## Summary / Key Takeaways

- Decision trees **partition the feature space** into regions and assign a simple prediction (constant) per region.
- The overall predictor is $g(\boldsymbol{x}) = \sum_{w} g^w(\boldsymbol{x}),\mathbb{I}{\boldsymbol{x} \in \mathcal{R}_w}$.
- Trees are built **top-down and greedily**, choosing the split that minimizes immediate training loss.
- **Leaf predictions:** majority vote (classification) or mean (regression).
- **Split quality** is measured by impurity reduction (misclassification, entropy, or Gini).
- A **termination criterion** (min node size, max depth, min loss gain) controls overfitting and the bias–variance tradeoff.

