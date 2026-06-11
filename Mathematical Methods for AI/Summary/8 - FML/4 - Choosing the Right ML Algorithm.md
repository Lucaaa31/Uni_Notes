### Workflow Overview

> **Context:** Selecting a machine learning algorithm follows a practical workflow: know your data, clean it, augment it, categorize the problem, understand constraints, then pick the algorithm. The choice depends as much on data properties and operational constraints as on the algorithm itself.

> [!definition] The Selection Workflow
> 
> 1. **Know your data** — understand it through summary statistics and visualization.
>     
> 2. **Clean your data** — handle missing values, outliers, and aggregation.
>     
> 3. **Augment your data** — feature engineering to prepare raw data for modeling.
>     
> 4. **Categorize the problem** — by input and by output.
>     
> 5. **Understand constraints** — storage, prediction speed, learning speed.
>     
> 6. **Find available algorithms** — those applicable and practical for the goals.
>     

### Know Your Data

> **Context:** Before modeling, summary statistics and visualizations reveal the structure, central tendency, spread, and relationships in the data, guiding later choices.

> [!definition] Statistics and Visualizations
> 
> - **Percentiles** identify the range covering most of the data.
>     
> - **Averages and medians** describe central tendency.
>     
> - **Correlations** can indicate strong relationships.
>     
> 
> | Plot | What it shows |
> | --- | --- |
> | **Box plots** | Identify outliers |
> | **Density plots / histograms** | Spread of data |
> | **Scatter plots** | Bivariate relationships |

### Clean Your Data

> **Context:** Data quality issues — missing values and outliers — affect models differently. Some algorithms tolerate them, others are highly sensitive, so cleaning decisions depend on the intended model.

> [!definition] Missing Values and Outliers
> 
> - **Missing data** affects some models more than others; even models that handle it can give poor predictions when key variables are missing.
>     
> - **Outliers** are common in multidimensional data. **Tree models** are usually less sensitive; **regression / equation-based models** can be strongly affected.
>     
> - Outliers may come from **bad data collection** or be **legitimate extreme values**.
>     

### Augment Your Data

> **Context:** Feature engineering transforms raw data into a form ready for modeling. Different models have different requirements, and some include built-in feature engineering.

> [!definition] Purposes of Feature Engineering
> 
> - **Make models easier to interpret** — e.g. binning.
>     
> - **Reduce redundancy and dimensionality** — e.g. PCA.
>     
> - **Rescale variables** — e.g. standardizing or normalizing.
>     

### Categorize the Problem

> **Context:** The problem type is determined in two steps, by the nature of the input data and by the nature of the desired output, which together narrow the space of applicable algorithms.

> [!definition] Categorize by Input and Output
> 
> **By input:**
> 
> | Input | Problem type |
> | --- | --- |
> | Labelled data | **Supervised learning** |
> | Unlabelled data + want structure | **Unsupervised learning** |
> | Optimize an objective by interacting with an environment | **Reinforcement learning** |
> 
> **By output:**
> 
> | Output | Problem type |
> | --- | --- |
> | A number | **Regression** |
> | A class | **Classification** |
> | A set of input groups | **Clustering** |
> | Detecting an anomaly | **Anomaly detection** |

### Understand Your Constraints

> **Context:** Operational constraints can rule out otherwise suitable algorithms, independently of accuracy.

> [!definition] Key Constraints
> 
> - **Data storage capacity** — the system may not store large models or datasets (e.g. embedded systems).
>     
> - **Prediction speed** — critical in real-time applications.
>     
> - **Learning speed** — sometimes a model must be updated rapidly on the fly with new data.
>     

### Find the Available Algorithms

> **Context:** Among applicable algorithms, the choice balances several practical factors, and model complexity is a key dimension since increasing it raises the risk of overfitting.

> [!definition] Selection Factors and Complexity
> 
> Factors affecting model choice: whether it meets the goals, preprocessing needed, accuracy, explainability, speed (build and prediction time), and scalability.
> 
> A model is generally **more complex** when it relies on more features, more complex feature engineering (polynomial terms, interactions, principal components), or has more computational overhead (a single tree vs. a 100-tree forest). The same algorithm can be made more complex via more parameters or hyperparameter choices, which increases the chance of **overfitting**.

### Linear Regression

> **Context:** The simplest ML algorithm, used to compute a continuous value rather than a category.

> [!definition] Linear Regression
> 
> - Use when predicting some future value of a currently-running process.
>     
> - **Unstable when features are redundant** (multicollinearity).
>     

### Logistic Regression

> **Context:** Performs binary classification by applying a non-linear sigmoid to a linear combination of features — effectively a very small neural network.

> [!definition] Logistic Regression
> 
> **Strengths:**
> 
> - Many ways to **regularize**; less worry about correlated features.
>     
> - Nice **probabilistic interpretation**.
>     
> - Easy to **update** with new data (unlike decision trees or SVMs).
>     
> - Reveals contributing factors — **not a black box**.
>     

### Decision Trees

> **Context:** Single trees are rarely used alone, but in composition (Random Forest, Gradient Tree Boosting) they are very efficient.

> [!definition] Decision Trees
> 
> **Strengths:**
> 
> - Easily handle **feature interactions**.
>     
> - **Non-parametric** — no worry about outliers or linear separability.
>     
> 
> **Weaknesses:**
> 
> - No **online learning** — must rebuild the tree for new examples.
>     
> - Easily **overfit** (mitigated by ensembles).
>     
> - Can take a lot of **memory** (more features → larger tree).
>     

### K-Means

> **Context:** A clustering task where labels are unknown and assigned based on object features, used to divide a large group into subgroups by common attributes.

> [!definition] K-Means
> 
> - **Biggest disadvantage:** the number of clusters $K$ must be known in advance, often requiring many trials to guess the best $K$.
>     

### Principal Component Analysis (PCA)

> **Context:** Provides dimensionality reduction, useful when there are many highly-correlated features that would otherwise cause overfitting.

> [!definition] PCA
> 
> - Provides both a low-dimensional **sample** representation and a low-dimensional representation of the **variables**.
>     
> - These representations help **visually find variables** characteristic of a group of samples.
>     

### Support Vector Machines (SVM)

> **Context:** A supervised technique for pattern recognition and classification when data has exactly two classes.

> [!definition] SVM
> 
> **Strengths:**
> 
> - **High accuracy**, with nice theoretical guarantees on overfitting.
>     
> - With an appropriate **kernel**, works well even when data isn't linearly separable in the base feature space.
>     
> - Especially popular in **text classification** (very high-dimensional spaces).
>     
> 
> **Weaknesses:**
> 
> - **Memory-intensive**, hard to interpret, difficult to tune.
>     

### Random Forest

> **Context:** An ensemble of decision trees that solves both regression and classification on large datasets.

> [!definition] Random Forest
> 
> **Strengths:**
> 
> - Identifies the most **significant variables** from thousands of inputs.
>     
> - Highly **scalable** to any number of dimensions, with generally acceptable performance.
>     
> 
> **Weaknesses:**
> 
> - Learning may be **slow** (depending on parameterization).
>     
> - Cannot **iteratively improve** the generated models.
>     
> 
> Related: **genetic algorithms** scale to any dimension with minimal knowledge of the data, the simplest being the microbial genetic algorithm.
### Applied Machine Learning
![[4 - Choosing the Right ML Algorithm-1780868671813.webp|413]]