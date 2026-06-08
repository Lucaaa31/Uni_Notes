

> [!abstract] Overview
> A practical workflow for selecting an ML algorithm:
>  **know your data → clean it → augment it → categorize the problem → understand constraints → pick the algorithm**. 
>  The final sections cover the strengths and weaknesses of common algorithms.

---

## 1. Know Your Data
### Summary statistics
Look at summary statistics and visualizations to understand the data before modeling.
- **Percentiles** help identify the range for most of the data.
- **Averages and medians** describe central tendency.
- **Correlations** can indicate strong relationships.

### Visualize the data

| Plot | What it shows |
|------|---------------|
| **Box plots** | Identify outliers |
| **Density plots / histograms** | Spread of data |
| **Scatter plots** | Bivariate relationships |

---

## 2. Clean Your Data

### Missing values
> [!warning] Missing data
> Missing data affects some models more than others. Even models that *handle* missing data can be sensitive to it — missing values for certain variables can result in poor predictions.

### Outliers
- Outliers can be very common in multidimensional data.
- Some models are less sensitive than others:
	- **Tree models** are usually less sensitive to outliers.
	- **Regression models** (or any equation-based model) can definitely be affected.
- Outliers may come from **bad data collection** *or* be **legitimate extreme values**.

> [!question] Does the data need to be aggregated?

---

## 3. Augment Your Data

**Feature engineering** is the process of going from raw data to data ready for modeling. It serves multiple purposes:
- **Make models easier to interpret** — e.g. binning.
- **Reduce redundancy and dimensionality** — e.g. PCA.
- **Rescale variables** — e.g. standardizing or normalizing.

> [!note]
> Different models have different feature-engineering requirements. Some have built-in feature engineering.

---

## 4. Categorize the Problem

A **two-step** process.
### Categorize by input

| Input | Problem type |
|-------|--------------|
| Labelled data | **Supervised learning** |
| Unlabelled data + want structure | **Unsupervised learning** |
| Optimize an objective by interacting with an environment | **Reinforcement learning** |

### Categorize by output

| Output | Problem type |
|--------|--------------|
| A number | **Regression** |
| A class | **Classification** |
| A set of input groups | **Clustering** |
| Detecting an anomaly | **Anomaly detection** |

---
## 5. Understand Your Constraints
- **Data storage capacity** — your system may not be able to store gigabytes of models or data to be clustered (e.g. embedded systems).
- **Does prediction have to be fast?** — critical in real-time applications. *Example:* autonomous driving needs fast road-sign classification to avoid accidents.
- **Does learning have to be fast?** — sometimes you must rapidly update a model on the fly with a different dataset.

---

## 6. Find the Available Algorithms

Identify algorithms that are **applicable and practical** given your tools. Factors affecting model choice:

- Whether the model meets the expected goals.
- How much **preprocessing** it needs.
- How **accurate** it is.
- How **explainable** it is.
- How **fast** it is (build time *and* prediction time).
- How **scalable** it is.

### Model complexity
> [!info] A model is generally *more complex* when it…
> - Relies on **more features** to learn and predict (2 vs 10 features).
> - Relies on **more complex feature engineering** (polynomial terms, interactions, principal components).
> - Has **more computational overhead** (single decision tree vs. a random forest of 100 trees).

The *same* algorithm can be made more complex via the number of parameters or hyperparameter choices:
- A **regression model** can add features, polynomial terms, interaction terms.
- A **decision tree** can have more or less depth.

> [!danger] Overfitting
> Making the same algorithm more complex increases the chance of **overfitting**.

---

# Algorithm Reference
## Linear Regression
Probably the simplest ML algorithm. Used to compute a **continuous value** (vs. classification, where output is categoric).
- Use when predicting some future value of a currently-running process.
- **Unstable when features are redundant** (multicollinearity).

## Logistic Regression
Performs **binary classification** — outputs are binary. Takes a linear combination of features and applies a non-linear **sigmoid** function, so it's a very small instance of a neural network.

**Strengths**
- Many ways to **regularize**; less worry about correlated features.
- Nice **probabilistic interpretation**.
- Easy to **update** with new data (unlike decision trees or SVMs).
- Reveals contributing factors — **not a black box**.

> [!example] Use cases
> - Predicting customer churn
> - Credit scoring & fraud detection
> - Measuring effectiveness of marketing campaigns

## Decision Trees
Single trees are rarely used alone, but in composition (Random Forest, Gradient Tree Boosting) they're very efficient.
**Strengths**
- Easily handle **feature interactions**.
- **Non-parametric** — no worry about outliers or linear separability.

**Weaknesses**
- No **online learning** — must rebuild the tree for new examples.
- Easily **overfit** (mitigated by ensembles like random forests / boosted trees).
- Can take a lot of **memory** (more features → deeper, larger tree).

> [!example] Use cases — great for choosing between courses of action
> - Investment decisions
> - Customer churn
> - Bank loan defaulters
> - Build vs. buy decisions
> - Sales lead qualification

## K-Means
A **clustering** task: you don't know labels and assign them based on object features. Use it to divide a large group (e.g. users) into groups by common attributes.

> [!tip] Signal words
> "How is this organized?", "grouping something", "concentrating on particular groups" → **clustering**.

- **Biggest disadvantage:** K-Means needs to know **the number of clusters (K) in advance** — often requires many trials to guess the best K.

## Principal Component Analysis (PCA)
Provides **dimensionality reduction**. Useful when you have many, highly-correlated features and models would easily overfit.
- Provides both a low-dimensional **sample** representation *and* a corresponding low-dimensional representation of the **variables**.
- These representations let you **visually find variables** characteristic of a group of samples.

## Support Vector Machines (SVM)
A **supervised** technique widely used in pattern recognition and classification — when data has **exactly two classes**.

**Strengths**
- **High accuracy**, nice theoretical guarantees on overfitting.
- With an appropriate **kernel**, works well even when data isn't linearly separable in the base feature space.
- Especially popular in **text classification** (very high-dimensional spaces).

**Weaknesses**
- **Memory-intensive**, hard to interpret, difficult to tune.

> [!example] Use cases
> - Detecting common diseases (e.g. diabetes)
> - Hand-written character recognition
> - Text categorization (news articles by topic)
> - Stock market price prediction

## Random Forest
An **ensemble of decision trees**. Solves both regression and classification on large datasets.

**Strengths**
- Identifies the most **significant variables** from thousands of inputs.
- Highly **scalable** to any number of dimensions; generally acceptable performance.

**Weaknesses**
- Learning may be **slow** (depending on parameterization).
- Cannot **iteratively improve** the generated models.

> [!note] Related
> **Genetic algorithms** scale admirably to any dimension/data with minimal knowledge of the data — the simplest being the *microbial genetic algorithm*.

> [!example] Use cases
> - Predict high-risk patients
> - Predict parts failures in manufacturing
> - Predict loan defaulters

---

## Appendix
### Type of Data
![[4 - Choosing the Right ML Algorithm-1780868644156.webp]]
### Applied Machine Learning
![[4 - Choosing the Right ML Algorithm-1780868671813.webp]]![[4 - Choosing the Right ML Algorithm-1780868697145.webp]]