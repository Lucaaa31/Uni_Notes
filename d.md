# Introduction to Learning Theory

## Supervised Learning

### Setup and Notation

#### Naïve Bayes Classifier

Given:

-   $P(Y)$, class prior

-   $d$ conditional independent features $X_1,...,X_d$ given the class
    label $Y$

-   For each $X_i$ feature, we have the conditional likelihood
    $P(X_i | Y)$

![image](Resources/17.png){width="75%"}

#### Supervised Learning Model

**Goal:** optimize model's parameters to obtain good predictions

![image](Resources/18.png){width="75%"}

#### Notations

![image](Resources/19.png){width="100%"}

-   $\mathcal{X}$ is the input space, and $\mathcal{Y}$ is the output
    space

-   In classification problems, $\mathcal{Y}$ is made of $K$ classes (or
    categories)

    -   $\mathcal{Y} := \{1, 2,..., K\}$

    -   if $K=2$, we use $\mathcal{Y} = \{0, 1\}$ or
        $\mathcal{Y} = \{-1, +1\}$

### Risk Theory

#### Loss

Given a **distribution** $\mathcal{D}$ over space of labeled examples
$\mathcal{X} \times \mathcal{Y}$, $\mathcal{D}$ is unknown and it
represents the population we care about. Assume the labels are produced
by a **function** $f : \mathcal{X} \rightarrow\mathcal{Y}$ and
$Y = f(X)$.

We define the **loss** $L(h(X), Y)$ of the classifier $h$ as the
function that measures the error between the assigned prediction and the prediction of the correct labeling function:
$$L(h(X), f(X)) = L(h(X), Y) = \begin{cases}
    1, & \text{if } h(X) \ne Y \\
    0, & \text{if } h(X) = Y
\end{cases}$$

#### True Risk

The true risk of a classifier $h$ is the probability that it does not
predict the correct label on a random data point drawn for
$\mathcal{D}$:
$$R_\mathcal{D}(h) := \mathbb{P}[h(X) \ne f(X)] = P[h(X) \ne Y]$$ In
other words, the true risk of the learner $h$ is the **expected value**
of the loss:
$$R_\mathcal{D}(h) := \mathbb{E}_{X \sim \mathcal{D}}\{L(h(X), Y)\}$$
$$\text{GenLoss}(h) = \sum_{(X, Y) \in \mathcal{D}}L(h(X), Y) P(X, Y)$$

#### Bayes Classifier

-   Assume $\mathcal{X}$ is discrete for now

-   Suppose $(\mathbf{x}, y) \sim \mathcal{D}$

We want to find $h : \mathcal{X} \rightarrow \mathcal{Y}$ that
minimizes:
$$\mathbb{P}[h(\mathbf{x}) \neq y] = \sum_{\mathbf{x} \in \mathcal{X}} \mathbb{P}[h(X) \neq y | X = \mathbf{x}] \cdot \mathbb{P}[X = \mathbf{x}]$$
For every $\mathbf{x} \in \mathcal{X}$, set $h(\mathbf{x})$ to minimize
$\mathbb{P}[h(X) \neq Y | X = \mathbf{x}]$. Since:
$$\mathbb{P}[h(X) \neq Y | X = \mathbf{x}] = 1 - \mathbb{P}[h(X) = Y | X = \mathbf{x}]$$
to minimize $\mathbb{P}[h(X) \neq Y | X = \mathbf{x}]$, we maximize
$\mathbb{P}[h(X) = Y | X = \mathbf{x}]$.

That is, choose:
$$h(\mathbf{x}) = \underset{y \in \mathcal{Y}}{\arg\max} \; \mathbb{P}[Y = y | X = \mathbf{x}]$$

::: tcolorbox
**Bayes (Optimal) Classifier** (this is NOT Naïve Bayes)
:::

#### Bayes Risk

Denote by $h^*$ the Bayes classifier. For the **Bayes Risk**
$R_\mathcal{D}(h^*)$ it holds $R_\mathcal{D}(h^*) \leq R_\mathcal{D}(h)$
for any $\mathcal{H}$ and $h \in \mathcal{H}$.

-   You cannot improve over the Bayes Risk, regardless of what ML
    algorithm you use!

-   We can estimate if we reach the Bayes Risk with only the human
    performance

### PAC Learning Therory

#### Batch Learning

In practical cases we do not know $f$ and we need to estimate $h$ from
the data.

-   The learner's input:

    -   Training data,
        $S = \{(\mathbf{x}_1, y_1) \cdots (\mathbf{x}_m, y_m)\} \in (\mathcal{X} \times \mathcal{Y})^m$

-   The learner's output:

    -   A prediction rule, $h : \mathcal{X} \rightarrow \mathcal{Y}$

-   What should be the goal of the learner? Intuitively, $h$ should be
    correct [on future examples]{.underline}.

#### Batch Learning -- IID Condition

When is there any hope for finding a classifier with high accuracy?

**Key assumption:** Data
$(\mathbf{x}_1, y_1), (\mathbf{x}_2, y_2), \cdots, (\mathbf{x}_n, y_n)$
are *identically and independently distributed (i.i.d.)* random labeled
examples with distribution $\mathcal{D}$, i.e., an i.i.d. sample from
$\mathcal{D}$.

This assumption is the connection between what we've seen in the past
to what we expect to see in the future.

#### Empirical Risk

Training data
$S = \{(\mathbf{x}_1, y_1) \cdots (\mathbf{x}_m, y_m)\} \in (\mathcal{X} \times \mathcal{Y})^m$
is an i.i.d. sample from some fixed but unknown probability distribution
$\mathcal{D}$ over space of labeled examples
$\mathcal{X} \times \mathcal{Y}$. A learning algorithm takes $S$ as
input and returns a predictor $h : \mathcal{X} \rightarrow \mathcal{Y}$.

Calculate the **empirical risk** by benchmarking the prediction against
ground truth:
$$R_S(h) = \frac{1}{m} \sum_{i=1}^{m} \mathbb{1}[h(\mathbf{x}_i) \neq y_i]$$

In this setting, can any learning algorithm always provide a non-trivial
guarantee on the error of the predictor it returns?

[No: some assumptions/conditions are required (\"No free lunch\"
theorem).]{style="color: red"}

#### Can Only Be Approximately Correct

[Claim:]{style="color: red"} Even when $R_\mathcal{D}(h^*) = 0$, we
can't hope to find $h$ s.t. $R_\mathcal{D}(h) = 0$.

-   **Proof:** for $\epsilon \in (0,1)$, take
    $\mathcal{X} = \{\mathbf{x}_1, \mathbf{x}_2\}$ and
    $\mathbb{P}_\mathcal{D}(\{\mathbf{x}_1\}) = 1 - \epsilon$,
    $\mathbb{P}_\mathcal{D}(\{\mathbf{x}_2\}) = \epsilon$.

-   The probability of not seeing $\mathbf{x}_2$ at all among $m$ i.i.d.
    examples is: $$(1 - \epsilon)^m \approx e^{-\epsilon m}$$

-   So, if $\epsilon \ll 1/m$ we're likely not to see $\mathbf{x}_2$ at
    all, and we will not know its label.

[Relaxation:]{style="color: red"} We'd be happy with
$R_\mathcal{D}(h) \leq \epsilon$, where $\epsilon$ is user-specified.

#### Can Only Be Probably Correct

-   Recall that the input to the learner is randomly generated.

-   There's always a (very small) chance to see the same example again
    and again.

-   [Claim:]{style="color: red"} No algorithm can guarantee
    $R_\mathcal{D}(h) \leq \epsilon$ for *sure*.

-   [Relaxation:]{style="color: red"} We'd allow the algorithm to fail
    with probability $\delta$, where $\delta \in (0,1)$ is
    user-specified.

    -   The probability is over the random choice of examples.

#### Probably Approximately Correct (PAC) Learning

-   The **learner doesn't know** $\mathcal{D}$ and the Bayes predictor.

-   The **learner receives** accuracy parameter $\epsilon$ and
    confidence parameter $\delta$.

-   The **learner can ask for training data** $S$ containing
    $m(\epsilon, \delta)$ examples.

-   Learner should output a hypothesis $h$ s.t.
    $\mathbb{P}[R_\mathcal{D}(h) \leq \epsilon] \geq 1 - \delta$.

-   That is, the learner should be **Probably** (with probability at
    least $1 - \delta$) **Approximately** (up to accuracy $\epsilon$)
    Correct: **PAC Learning**.

::: tcolorbox
**How many training samples does a learner need for PAC Learning with
specified parameters $\epsilon$ and $\delta$?**
:::

#### Realizability Assumption

Consider binary classification with $\mathcal{Y} = \{0, 1\}$.

-   **Realizability assumption:** Assume that, for a given class
    $\mathcal{H}$ of functions from $\mathcal{X} \rightarrow \{0,1\}$,
    there exists $h^* \in \mathcal{H}$ such that
    $R_\mathcal{D}(h^*) = 0$.

    -   This implies that $h^*$ is the Bayes classifier.

-   [The learner knows $\mathcal{H}$]{style="color: red"}

Examples of function classes $\mathcal{H}$:

![image](Resources/21.png){width="75%"}

#### Learning in the Realizable Setting

What is a sensible learning algorithm for the realizable setting?

-   Given training data $S$ with $m$ samples, return any
    $h \in \mathcal{H}$ such that $R_S(h) = 0$.

-   This is a special case of **Empirical Risk Minimization** (ERM),
    that is:
    $$h_S = \underset{h \in \mathcal{H}}{\arg\min} \; \frac{1}{m} \sum_{i=1}^{m} \mathbb{1}[h(\mathbf{x}_i) \neq y_i]$$

-   We can show that for the consistent classifier $h \in \mathcal{H}$
    obtained from the data by searching for the minimal empirical risk
    (ERM: $R_S(h) = 0$), the true risk $R_\mathcal{D}(h)$ remains
    bounded.

::: tcolorbox
A consistent classifier is one for which the probability of correct
classification, given a training set, approaches the Bayes risk as the
size of the training set increases.
:::

#### Analysis of a Consistent Classifier

Define
$\mathcal{H}_B = \{h \in \mathcal{H} : R_\mathcal{D}(h) > \epsilon\}$,
the subset of the "bad" hypotheses. First: $$\begin{aligned}
\mathbb{P}[R_\mathcal{D}(h) > \epsilon] &\leq \mathbb{P}[\exists h \in \mathcal{H} : R_S(h) = 0, R_\mathcal{D}(h) > \epsilon] \quad (A \subset B \Rightarrow \mathbb{P}(A) \leq \mathbb{P}(B)) \\
&= \mathbb{P}[\exists h \in \mathcal{H}_B : R_S(h) = 0] \quad \text{(by definition of } \mathcal{H}_B\text{)} \\
&= \mathbb{P}\left[\bigcup_{h \in \mathcal{H}_B} \{R_S(h) = 0\}\right] \\
&\leq \sum_{h \in \mathcal{H}_B} \mathbb{P}[R_S(h) = 0] \quad \text{(Union bound: } \mathbb{P}[A \cup B] \leq \mathbb{P}[A] + \mathbb{P}[B]\text{)} \\
&= \sum_{h \in \mathcal{H}_B} \prod_{i=1}^{m} \mathbb{P}[h(x_i) = y_i] \\
&\leq \sum_{h \in \mathcal{H}_B} (1-\epsilon)^m = |\mathcal{H}_B|(1-\epsilon)^m \quad (h \in \mathcal{H}_B \Rightarrow \mathbb{P}[h(X) \neq Y] > \epsilon)
\end{aligned}$$

Second: $$\begin{aligned}
\mathbb{P}[R_\mathcal{D}(h) > \epsilon] &\leq |\mathcal{H}_B|(1-\epsilon)^m \\
&\leq |\mathcal{H}|(1-\epsilon)^m \quad (\mathcal{H}_B \subset \mathcal{H}) \\
&\leq |\mathcal{H}|e^{-\epsilon m} \quad (1 - x \leq \exp(-x))
\end{aligned}$$

Let $\delta \in (0,1)$, $\epsilon > 0$, and
$m \geq \frac{\ln(|\mathcal{H}|/\delta)}{\epsilon}$. Then, with
probability $1 - \delta$, we have $R_\mathcal{D}(h) \leq \epsilon$.

[Clearly these bounds are useful if $\ln|\mathcal{H}|$ is finite and not
too large w.r.t. $|S|$.]{style="color: red"}

#### PAC Learning

::: tcolorbox
A hypothesis class $\mathcal{H}$ is PAC learnable if there exists a
function $m_\mathcal{H} : (0,1)^2 \rightarrow \mathbb{N}$ and a learning
algorithm with the following property:

-   for every $\epsilon, \delta \in (0,1)$

-   for every distribution $\mathcal{D}$ over
    $\mathcal{X} \times \mathcal{Y}$ such that the realizability
    assumption holds in $\mathcal{H}$

when running the learning algorithm on
$m \geq m_\mathcal{H}(\epsilon, \delta)$ i.i.d. examples generated by
$\mathcal{D}$, the algorithm returns a hypothesis $h$ such that, with
probability of at least $1 - \delta$ (over the choice of the examples),
$R_\mathcal{D}(h) \leq \epsilon$.
:::

-   $m_\mathcal{H}$ is called the **sample complexity** of learning
    $\mathcal{H}$.

PAC learnability formalizes the question:

-   Can I learn anything with this hypothesis class (under the
    realizability assumption)?

-   And how many samples do I need?

#### Finite Hypothesis Classes

::: tcolorbox
Let $\mathcal{H}$ be a [finite]{style="color: red"} hypothesis class.
Then

-   $\mathcal{H}$ is PAC learnable with sample complexity
    $m_\mathcal{H}(\epsilon, \delta) \leq \frac{\ln(|\mathcal{H}|/\delta)}{\epsilon}$

-   This sample complexity is obtained by using the ERM learning rule
    over $\mathcal{H}$
:::

With the realizability assumption we knew that within the chosen class
of functions $\mathcal{H}$ there was the perfect learner
$\mathbb{P}[h^*(X) \neq Y] = 0$.

In many practical cases this assumption does not hold, i.e. we cannot be
sure that our family of learners includes the Bayes classifier.

We can still require that the learning algorithm will find a predictor
whose error is **not much larger than the best possible error** of a
predictor in some given hypothesis class.

Of course the strength of such a requirement depends on the choice of
that hypothesis class.

### Agnostic (Non-Realizable) Setting

::: tcolorbox
Let $\mathcal{H}$ be a [finite]{style="color: red"} hypothesis class and
$L$ a loss function [bounded]{style="color: blue"} between 0 and 1. Then

-   $\mathcal{H}$ is Agnostic PAC learnable with sample complexity
    $m_\mathcal{H}(\epsilon, \delta) \leq \frac{\ln\!\left(\frac{2|\mathcal{H}|}{\delta}\right)}{2\epsilon^2}$

-   This sample complexity is obtained by using the ERM learning rule
    over $\mathcal{H}$
:::

### VC Dimension

#### Infinite Hypothesis Classes -- The Problem

![image](Resources/22.png){width="75%"}

::: tcolorbox
We can no longer use the cardinality of the hypothesis space
$|\mathcal{H}|$ for bounding the sample complexity.
:::

#### Shattering

::: tcolorbox
$\mathcal{H}[S]$ = the set of splittings of dataset $S$ using concepts
from $\mathcal{H}$.\
$\mathcal{H}$ *shatters* $S$ if $|\mathcal{H}[S]| = 2^{|S|}$.
:::

A set of points $S$ is shattered by $\mathcal{H}$ if there are
hypotheses in $\mathcal{H}$ that split $S$ in all of the $2^{|S|}$
possible ways; i.e., all possible ways of classifying points in $S$ are
achievable using concepts in $\mathcal{H}$.

#### VC Dimension (Definition)

::: tcolorbox
The VC-dimension of a hypothesis space $\mathcal{H}$ is the cardinality
of the largest set $S$ that can be shattered by $\mathcal{H}$.
:::

If arbitrarily large finite sets can be shattered by $\mathcal{H}$, then
$\text{VCdim}(H) = \infty$.

#### Example: Threshold Functions over $\mathbb{R}$

Let $\mathcal{H}$ be the class of threshold functions over $\mathbb{R}$.

![image](Resources/24.png){width="100%"}

#### Example: Linear Classifiers in 2D

Consider $\mathcal{H}$ as the set of linear classifiers in two
dimensions.

![image](Resources/23.png){width="100%"}

#### Example: Linear Classifiers in 2D (\|S\|=4)

Consider $\mathcal{H}$ as the set of linear classifiers in two
dimensions.

::: tcolorbox
For the general case of linear classifiers in $d$ dimensions:
$$\textit{VCdim} = d + 1 \quad \text{(holds for } d \geq 2\text{)}$$
:::

![image](Resources/24ge.png){width="100%"}

#### Not PAC Learnable

::: tcolorbox
Let $\mathcal{H}$ be a class of infinite VC-dimension. Then
$\mathcal{H}$ is not PAC learnable.
:::

**Intuitive Proof:**

-   If $\mathcal{H}$ shatters some set $S$ of size $2m$, then we cannot
    learn $\mathcal{H}$ using $m$ examples.

-   Indeed, the first $m$ samples give us no information about the
    labels of the rest of the instances in $S$.

-   Every possible labeling of the rest of the instances can still be
    explained by some hypothesis in $\mathcal{H}$.

#### Infinite Hypothesis Classes -- The Discretization Trick

Because of the nature of the computer all the parameters of the
algorithms will be stored in a floating point representation, so the
class is actually finite. v

With $d$ parameters we will have:

-   $|\mathcal{H}_\text{discrete}| \le 2^{32d}$

-   $|\mathcal{H}_\text{discrete}| \le 10d$

### Overfitting and Model Selection

#### Polynomial Curve Fitting -- Motivation

Example: regression using a polynomial curve of degree $M$. The true
function is: $t = \sin(2\pi x) + \epsilon$ (where $\epsilon$ is noise).

$$y(x, \mathbf{w}) = w_0 + w_1 x + w_2 x^2 + \ldots + w_M x^M \sum_{j=0}^{M} w_j x^j$$

<figure id="fig:iterations_M">
<img src="Resources/25.png" />
<img src="Resources/26.png" />
<img src="Resources/27.png" />
<figcaption>Evolution of the fit based on <span
class="math inline"><em>M</em></span>.</figcaption>
</figure>

z

### Overfitting

General Phenomenon:

![image](Resources/28.png){width="75%"}

#### Overfitting

**Definition:** When a predictor has excellent performance on the
training set, but its performance on the true "world" is very poor.

-   We already know some cases when overfitting does not happen: finite
    hypothesis classes and enough samples.

-   Hence, overfitting is not about noise nor about having training
    error 0, but about the fact that training error and true error are
    two different things.

-   Only under certain conditions do they behave similarly, i.e., with
    enough samples with respect to the complexity of your hypothesis
    class.

#### Cross Validation

![image](Resources/29.png){width="75%"}

#### $k$-fold Cross Validation

![image](Resources/30.png){width="75%"}

:::: algorithm
::: algorithmic
**Input:** training set
$S = \{(\mathbf{x}_1, y_1), \ldots, (\mathbf{x}_m, y_m)\}$, learning
algorithm $A$ and a set of parameter values $\Theta$ Partition $S$ into
$S_1, S_2, \ldots, S_k$ $h_{i,\theta} = A(S \setminus S_i;\, \theta)$
$\text{error}(\theta) = \frac{1}{k} \sum_{i=1}^{k} L_{S_i}(h_{i,\theta})$
**Output:** $\theta^* = \arg\min_\theta[\text{error}(\theta)]$;
$h_{\theta^*} = A(S;\, \theta^*)$
:::
::::

#### Train -- Validation -- Test

In practice, we usually have one pool of examples and we split them into
three sets:

-   **Training set:** apply the learning algorithm with different
    parameters on the training set to produce $H = h_1, \ldots, h_r$

-   **Validation set:** choose $h^*$ from $H$ based on the validation
    set

-   **Test set:** estimate the true error of $h^*$ using the test set

Note: you cannot avoid the use of a test set -- the error on the
validation set is negatively biased!

-   **If the number of samples in the test set is too small, you cannot
    reliably estimate the true error!**

-   If the gain over the baseline is too small, you might not be able to
    exclude the case that the gain is just due to a random fluctuation.

#### Model Selection In Summary

-   There is an *unavoidable* trade-off between complexity of the model
    and its variance.

-   More powerful models approximate better the Bayes function, but need
    more data.

-   **Never use the test set for any selection of
    parameters/algorithms.**

#### Regularization

::: tcolorbox
Control complexity by penalizing complex models in learning.
:::

#### Regularized ERM

Given training data $S$ with $m$ samples, return any $h \in \mathcal{H}$
such that $R_S(h) = 0$.

This is a special case of **Empirical Risk Minimization** (ERM), that
is:

$$h_S = \underset{h \in \mathcal{H}}{\arg\min} \left( \frac{1}{m} \sum_{i=1}^{m} \mathbbm{1}[h(\mathbf{x}_i) \neq y_i] \quad \textcolor{red}{+ \; \lambda \; \text{Complexity}(h)} \right)$$

-   $\lambda$ is a parameter, a positive number that serves as a
    conversion rate between the loss and the hypothesis complexity (they
    might not have the same scale).

-   The form of the complexity/regularization function depends on the
    hypothesis space.

-   We still need cross validation, and in this case we need to include
    the parameter $\lambda$ -- select the one that gives the best
    validation score.

## Linear Regression

### Setup

#### Introduction

::: tcolorbox
**Regression** analysis considers prediction problems where the label
space $\mathcal{Y}$ is continuous (e.g. $[0,1]$, $\mathbb{R}^d$, etc.)
:::

-   Measuring quality of a predictor
    $h : \mathcal{X} \rightarrow \mathcal{Y}$ using prediction error
    $\mathbb{P}[h(X) \neq Y]$ doesn't make much sense in the regression
    context (why?)

-   **Goal:** find $h : \mathcal{X} \rightarrow \mathcal{Y}$, from some
    function class $\mathcal{H}$, minimizing error measured using a loss
    function
    $L : \mathcal{Y} \times \mathcal{Y} \rightarrow \mathbb{R}$, i.e.:
    $$\mathbb{E}[L(Y, h(X))]$$

#### Loss Function

::: tcolorbox
A loss function
$L : \mathcal{Y} \times \mathcal{Y} \rightarrow \mathbb{R}$ maps
decisions to costs: $L(\hat{y}, y)$ defines the penalty paid for
predicting $\hat{y}$ when the true value is $y$.
:::

-   Standard choice for classification: 0/1 loss
    $$L_{0/1}(\hat{y}, y) = \begin{cases} 0 & \hat{y} = y \\ 1 & \text{otherwise} \end{cases}$$

-   Standard choice for regression: squared loss
    $L(\hat{y}, y) = (\hat{y} - y)^2$

    -   Not the only possible choice.

#### Empirical Loss

We consider a parametric function $h_\mathbf{w}(\mathbf{x})$. For
example, a linear function:
$h_\mathbf{w}(\mathbf{x}) = \mathbf{w}^\top \mathbf{x} + b$.

The empirical loss of function $y = h_\mathbf{w}(\mathbf{x})$ on a set
$S$:
$$L_S(\mathbf{w}) = \frac{1}{N} \sum_{i=1}^{N} L(h_\mathbf{w}(\mathbf{x}_i), y_i)$$

[We will find an ERM over $\mathbb{R}^d$ w.r.t.
$L$.]{style="color: red"}

-   E.g., Least Squares minimizes the empirical loss for the squared
    loss.

#### Linear Predictors in 1D

-   Our features are vectors in $\mathbb{R}$, i.e.
    $\mathcal{X} = \mathbb{R}$

-   The labels are real numbers, i.e. $\mathcal{Y} = \mathbb{R}$

-   Consider predictors of the form:
    $$\hat{y} = h_{(w,b)}(x) = w\, x + b$$

-   Hypothesis class:
    $\mathcal{H} = \{h_{(w,b)} : w, b \in \mathbb{R}\}$

    -   Infinite, but remember the discretization trick!

### Least Squares

#### Linear Fitting to Data

[We want to fit a linear function]{style="color: red"} to an observed
set of points $\mathcal{X} = [\mathbf{x}_1, \cdots, \mathbf{x}_N]$ with
associated labels $\mathcal{Y} = [y_1, \ldots, y_N]$.

**Least squares (LSQ) fitting criterion:** find the function that
minimizes the sum (or average) of square distances between actual $y_i$
in the training set and predicted ones -- that is ERM.

![image](Resources/31.png){width="50%"}

#### Linear Functions

General form:
$$\hat{y} = h_{(b,\mathbf{w})}(\mathbf{x}) = b + w_1 x_1 + \cdots + w_d x_d = b + \langle \mathbf{w}, \mathbf{x} \rangle$$

-   1D case ($\mathcal{X} = \mathbb{R}$): a line

-   $\mathcal{X} = \mathbb{R}^2$: a plane

-   Hyperplane in general, $d$-dimensional case

-   Hypothesis class:
    $\mathcal{H} = \{h_{(b,\mathbf{w})} : b \in \mathbb{R},\, \mathbf{w} \in \mathbb{R}^d\}$

![image](Resources/32.png){width="50%"}

#### Least Squares Criterion

Given pairs of input/output values, find $(b, \mathbf{w})$ to minimize
the prediction errors (on average).

That is, using square loss on
$\mathcal{H} = \{h_{(b,\mathbf{w})} : b \in \mathbb{R},\, \mathbf{w} \in \mathbb{R}^d\}$:
the squared error on $(\mathbf{x}, y)$ is:
$$(y - (b + \langle \mathbf{w}, \mathbf{x} \rangle))^2$$

#### Least Squares in Matrix / Vector Form

Given training data:
$$\mathbf{X} = \begin{bmatrix} - & \mathbf{x}_1^\top & - \\ - & \mathbf{x}_2^\top & - \\ & \vdots & \\ - & \mathbf{x}_N^\top & - \end{bmatrix} \in \mathbb{R}^{N \times d} \qquad \mathbf{y} = \begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_N \end{bmatrix} \in \mathbb{R}^N$$

find $b \in \mathbb{R}$ and $\mathbf{w} \in \mathbb{R}^d$ to minimize:
$$\sum_{i=1}^{N}(y_i - (b + \langle \mathbf{w}, \mathbf{x}_i \rangle))^2 = \left\| \mathbf{y} - \begin{bmatrix} \mathbf{1} & \mathbf{X} \end{bmatrix} \begin{bmatrix} b \\ \mathbf{w} \end{bmatrix} \right\|_2^2$$

**Simplification:** Replace $\mathbf{X}$ with
$\begin{bmatrix} \mathbf{1} & \mathbf{X} \end{bmatrix}$ and $\mathbf{w}$
with $\begin{bmatrix} b \\ \mathbf{w} \end{bmatrix}$, to get the simpler
expression: $$\|\mathbf{y} - \mathbf{X}\mathbf{w}\|_2^2$$

#### Least Squares via Calculus

The least squares criterion is a convex function of $\mathbf{w}$; so it
suffices to find $\mathbf{w}$ where the gradient is zero.

::: tcolorbox
$$f(\alpha x + (1-\alpha)y) \leq \alpha f(x) + (1-\alpha)f(y)$$ for all
$x, y$ and $\alpha \in [0,1]$.

![image](Resources/33.png){width="75%"}

Let us further assume that $f$ is a
[differentiable]{style="color: blue"} function, so that we can compute
its [derivative]{style="color: blue"} $\frac{df}{dx}$ at all points $x$.
Intuition tells us that the minimizer $x$ is where
$\frac{df(x)}{dx} = 0$.
:::

Taking the gradient with respect to $\mathbf{w}$:
$$\nabla_\mathbf{w} \|\mathbf{y} - \mathbf{X}\mathbf{w}\|_2^2 = 2\mathbf{X}^\top(\mathbf{X}\mathbf{w} - \mathbf{y})$$

This is zero when:
$$(\mathbf{X}^\top \mathbf{X})\mathbf{w} = \mathbf{X}^\top \mathbf{y}$$
This is a linear system of equations in $\mathbf{w}$.

If $\mathbf{X}^\top \mathbf{X}$ is invertible, the solution is:
$$\mathbf{w}_{\text{ols}} := (\mathbf{X}^\top \mathbf{X})^{-1} \mathbf{X}^\top \mathbf{y}$$
"ordinary least squares"

#### Linear Predictor in 1D

Forget about $b$ for one second. We want to solve:
$$\underset{w}{\arg\min} \; \sum_i (y_i - w x_i)^2$$

Taking the derivative w.r.t. $w$ and setting it to zero:
$$\frac{\partial}{\partial w} \sum_i (y_i - w x_i)^2 = 2\sum_i -x_i(y_i - w x_i) \;\Rightarrow$$
$$2\sum_i x_i(y_i - w x_i) = 0 \;\Rightarrow\; 2\sum_i x_i y_i - 2\sum_i w x_i^2 = 0$$
$$\sum_i x_i y_i = \sum_i w x_i^2 \;\Rightarrow$$
$$w = \frac{\sum_i x_i y_i}{\sum_i x_i^2}$$

#### Add the Bias Term

So far we assumed that the line passes through the origin. What if the
line does not? No problem: simply change the model to:
$$y = w_0 + w_1 x$$

We can use least squares to determine $w_0$ and $w_1$:

$$w_0 = \frac{\sum_i y_i - w_1 x_i}{n} \qquad w_1 = \frac{\sum_i x_i(y_i - w_0)}{\sum_i x_i^2}$$

![image](Resources/34.png){width="50%"}

### Extensions

#### Ridge Regression

The regularized objective to minimize is:
$$\frac{1}{2}\|\mathbf{y} - \mathbf{X}\mathbf{w}\|_2^2 + \frac{\alpha}{2}\|\mathbf{w}\|_2^2$$

-   Now our objective consists in minimizing both the loss function and
    a regularization term.

-   The regularizer prevents the model from becoming too complex.

-   It is a standard way to control overfitting and becomes particularly
    important for small number of samples ($N$) and large vector
    dimensionality ($d$).

Taking the gradient and setting it to zero:
$$\nabla_\mathbf{w} \left( \|\mathbf{y} - \mathbf{X}\mathbf{w}\|_2^2 + \frac{\alpha}{2}\|\mathbf{w}\|_2^2 \right) = \mathbf{X}^\top(\mathbf{X}\mathbf{w} - \mathbf{y}) + \alpha \mathbf{w} = 0$$

$$\Rightarrow \mathbf{w} = (\mathbf{X}^\top \mathbf{X} + \alpha \mathbf{I})^{-1} \mathbf{X}^\top \mathbf{y} \qquad \textcolor{red}{\text{Ridge Regression}}$$

#### Polynomial Regression

We can easily learn *non-linear* predictors in the same way. Consider
$\mathcal{X} = \mathbb{R}$, and the predictor:
$$\hat{y} = f(x) = b + w_1 x + w_2 x^2 + \cdots + w_m x^m$$

-   Is this a linear model? No! Yet it is linear in
    $(b, w_1, \cdots, w_m)$.

-   It can be learned with Least Squares, as:
    $$\hat{y} = h(x) = \langle \phi(x), \mathbf{w} \rangle = b + w_1 x + w_2 x^2 + \cdots + w_m x^m$$
    where $\phi(x) = [1, x, x^2, x^3, \cdots, x^m] \in \mathbb{R}^{m+1}$

-   The same trick can be used when $\mathcal{X} = \mathbb{R}^d$.

#### Fitting Non-linear Models (RBF)

Transform $\mathbf{x} \in \mathbb{R}^d$ into a vector
$\phi(\mathbf{x}) \in \mathbb{R}^m$:
$$\hat{y} = h_\mathbf{w}(\mathbf{x}) = \sum_{i=1}^{m} w_i \phi_i(\mathbf{x}) = \mathbf{w}^\top \phi(\mathbf{x}), \quad \text{still linear}$$

Examples of basis functions:

-   $\phi_i(\mathbf{x}) = \exp\!\left(-\frac{\|\mathbf{x} - \boldsymbol{\mu}_i\|^2}{s_i^2}\right)$,
    Isotropic Gaussians $\leftarrow$ **Radial Basis Function**

For example, for Least Squares we have:
$$\sum_{i=1}^{N}(\mathbf{w}^\top \phi(\mathbf{x}_i) - y_i)^2 = \|\mathbf{y} - \boldsymbol{\Phi}\mathbf{w}\|_2^2$$
where:
$$\boldsymbol{\Phi} = \begin{bmatrix} \phi_1(\mathbf{x}_1) & \phi_2(\mathbf{x}_1) & \cdots & \phi_m(\mathbf{x}_1) \\ \phi_1(\mathbf{x}_2) & \phi_2(\mathbf{x}_2) & \cdots & \phi_m(\mathbf{x}_2) \\ \vdots & \vdots & \ddots & \vdots \\ \phi_1(\mathbf{x}_N) & \phi_2(\mathbf{x}_N) & \cdots & \phi_m(\mathbf{x}_N) \end{bmatrix}$$
We solve it exactly as before.
