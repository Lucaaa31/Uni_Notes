
Probability is a very important Machine Learning tool because it allows
to summarize uncertainty from various sources, such as:
-   **Uncertain inputs:** missing or noisy data
-   **Uncertain knowledge:** stochastic effects or incomplete knowledge
    of the causality of the domain
-   **Uncertain outputs:** induction or incomplete deductive inference

## Basic Concepts
### Sample Spaces $\Omega$
A sample space $\Omega$ is the set of all possible outcomes of a random
experiment.
For example:
-   The roll of a dice has 1, 2, 3, 4, 5, 6 as outcomes
-   The flip of a coin has Head, Tail as outcomes
-   The flip of three coins has HHH, HHT, HTT, THT, TTH, HTH, THH, TTT
-   And so on\...

### Event
An event is a subset of the sample space of $\Omega$.
In the example before we could have:
-   The outcome of the dice is \<4
### Probability
The probability P(A) that event $A$ happens, **is a function that maps
the event A onto the interval \[0, 1\]**. P(A) is also called the
probability measure of A.
![[01 - Intro Probability and Statistics-1786634666739.webp]]

### The Axioms of Probability
So, what defines a reasonable theory of uncertainty?
#### All probabilities are between 0 and 1
$$0 \le P(A) \le 1$$

![[01 - Intro Probability and Statistics-1786634696938.webp]]

#### Valid propositions have probability 1, Unsatisfiable propositions have probability 0

$$P(\text{empty-set}) = 0, P(\text{everything}) = 1$$
![[01 - Intro Probability and Statistics-1786634709237.webp]]

![[01 - Intro Probability and Statistics-1786634722322.webp]]

#### The probability of a disjunction
The probability of a disjunction is given by
$$P(A \cup B) = P(A) + P(B) – P(A \cap B)$$
![[01 - Intro Probability and Statistics-1786634738041.webp|584]]+
### Random Variables
Real-valued **random variable**is a function of the outcome of a
randomized experiment: $$X: \Omega \rightarrow \mathbb{R}$$Probability that $X$ takes some value: $$P(a < X < b) = P (w: a < X(\omega) < b) \\
$$ $$P(X = a) = P(\omega : X(\omega) = a)$$

#### Continuous Random Variables
A random variable $X$ is **continuous** if its set of possible values is
an entire interval of numbers: $$P(X \in B) = \int_B f(t)dt$$Usually $B$ is an interval of the type $[a, b]$ so:
$$P(a \le X \le b) = \int_a^b f(t)dt$$

### Probability Density Function
From this definition we can say that a **probability density function**
is a nonnegative function that integrates to one.
![[01 - Intro Probability and Statistics-1786634788004.webp|616]]
Here's some properties of the pdf:
1.  $f(x) \ge 0$
2.  $\int_{-\infty}^{+\infty} f(x) = 1$
3.  $f(x) = \frac{d}{dx}F(x)$
4.  $F(x) = \int_{-\infty}^x f(t)dt$
5.  $P(a \le X \le b) = \int_a^b f(t)dt$

#### Expectation
$$\mathbb{E}[X] = \begin{cases}
    \sum_{i \in \Omega} x_i f(x_i) & \text{discrete} \\
    \int_{-\infty}^{+\infty}x f(x) dx & \text{continuous}
\end{cases}$$ Some properties of the expectation:
-   $\mathbb{E}[a]=a$ for any constant $a \in \mathbb{R}$
-   $\mathbb{E}[a\cdot f(X) + b \cdot g(x)] = a \cdot \mathbb{E}[f(X)] + b \cdot \mathbb{E}[g(X)]$
#### Variance
$$Var[X] = \mathbb{E}[(X - \mathbb{E}[X])^2] = \begin{cases}
    \sum_{i \in \Omega} (x_i - \mathbb{E}[X])^2 f(x_i) & \text{discrete} \\
    \int_{-\infty}^{+\infty} (x - \mathbb{E}[X])^2 f(x) dx & \text{continuous}
\end{cases}$$ Some properties of the variance:
-   $Var[a] = 0$ for any constant $a \in \mathbb{R}$
-   $Var[a f(x)] = a^2 Var[f(x)]$ for any constant $a \in \mathbb{R}$

### Joint Probability Distributions
Two or more variables may **interact**. So, the probability of one
variable to take a certain value depends on what values the others
variables are taking: $$f_{X, Y} (x, y) = P(X = x \text{ and } Y = y)$$
![[01 - Intro Probability and Statistics-1786634858079.webp|618]]
### Marginal Distribution
Marginal distributions are sub-tables which eliminate variables,
**marginalization** means combine collapsed rows using the addiction.![[01 - Intro Probability and Statistics-1786640198992.webp|559]]

### Conditional Probabilities

$P(X|Y) =$ Fraction of worlds in which X event is true given Y event is
true.

![[01 - Intro Probability and Statistics-1786640262204.webp|561]]

### Conditional Distributions

Conditional distributions are probability distributions over some
variables given fixed values of others.

![[01 - Intro Probability and Statistics-1786640279296.webp]]

### The Product
![[01 - Intro Probability and Statistics-1786640291160.webp]]
### Chain Rule
We can always write any joint distribution as an incremental product of
conditional distributions:
$$P(x_1, x_2, x_3) = P(x_1)P(x_2|x_1)P(x_3|x_1, x_2)$$
$$P(x_1, x_2, ..., x_n) = \prod_i P(x_i | x_1 ... x_{i-1})$$
$$Pr(X, Y, Z) = \frac{Pr(X, Y, Z)}{P(Y, Z)} \frac{P(Y, Z)}{P(Z)} P(Z)$$

### Independence
$Y$ and $X$ are independent if they don't contain information about each
others:
-   Observing $Y$ doesn't help predicting $X$
-   Observing $X$ doesn't help predicting $Y$

Properties:
-   $P(X, Y) = P(X)P(Y)$
-   $P(X | Y) = P(X)$
### Bayes' Rule
The **Bayes' Rule** is one of the most fundamental results in
probability theory, and it connects prior knowledge with observed
evidence. It is derived directly from the definition of conditional
probability: $$P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}$$ In the context
of machine learning and inference, Bayes' rule is typically written as:
$$\underbrace{P(x | y)}_{\text{posterior}} = \frac{\underbrace{P(y | x)}_{\text{likelihood}} \cdot \underbrace{P(x)}_{\text{prior}}}{\underbrace{P(y)}_{\text{evidence}}}$$
-   **Prior** $P(x)$: our belief about $\theta$ before observing data $y$
-   **Likelihood** $P(y|x)$: the probability of observing $y$ given $x$
-   **Posterior** $P(x|y)$: our updated belief about $x$ after observing $y$
-   **Evidence** $P(y)$: a normalizing constant ensuring the posterior sums/integrates to 1

### Naïve Bayes Classifier
A common application of Bayes' rule is **classification**. Given a class
label $y$ and a set of attributes (**features**)
$\mathbf{x} = (x_1, x_2, \ldots, x_n)$, we want to compute:
$$P(y | \mathbf{x}) = \frac{P(\mathbf{x} | y) \cdot P(y)}{P(\mathbf{x})}$$

-   Estimating $P(y)$ is easy: for a discrete class label we simply count the frequency of each class in the training data.
-   Estimating $P(\mathbf{x}|y)$ is hard in general, because it requires modeling the full joint distribution over all attributes given the class.

However, if we make the **conditional independence assumption** --- that all attributes $x_i$ are independent of each other given the class label $y$ --- estimation becomes tractable:
$$P(\mathbf{x}|y) = \prod_{i=1}^{n} P(x_i | y)$$This leads to the **Naïve Bayes classifier**:
$$\hat{y} = \arg\max_y \; P(y) \prod_{i=1}^{n} P(x_i | y)$$

#### Summary and Properties
-   **Computationally very fast**:
    -   Training: only one pass over the training set
    -   Classification: linear in the number of attributes
-   Despite the conditional independence assumption (which rarely holds
    in practice), Naïve Bayes shows good performance in many application
    domains.
-   Best used when a moderate or large training set is available and
    instances are represented by a large number of attributes.
#### Laplacian Correction
An issue arises when an attribute value never appears together with a
certain class label in the training set. For example:
$$P(\text{cold} | \text{yes}) = 0 \implies P(\text{yes} | \text{cold}, \ldots) = 0$$
This is problematic because a single zero probability wipes out the
entire product. The **Laplacian correction** (also called Laplace
smoothing) fixes this by adding one or more virtual samples for each
combination: $$\hat P(X_j = a_{jk} | C = c_i) = \frac{n_c + mp}{n 
 m}$$ where:
-   **$n_c$**: number of training examples for which $X_j = a_{jk}$ and $C = c_i$
-   **$n$**: number of training examples for which $C = c_i$
-   **p**: prior estimate (usually, $p = 1/t$ for $t$ possible values of $X_j$)
-   **m**: weight to prior (number of \"virtual\" examples, $m \ge 1$)

## Estimating Probabilities
We are often faced with the situation of having random data which we
know (or believe) is drawn from a **parametric model**, whose parameters
we do not know.

For example: the outcome of tossing a coin is drawn from a $\text{Bernoulli}(\theta)$ distribution with unknown parameter $\theta$.

The flips are assumed to be **independently and identically
distributed** (i.i.d.):
-   **Independent**: each flip does not affect the others
-   **Identically distributed**: each flip follows the same Bernoulli
    distribution

### Maximum Likelihood Estimation (MLE)
**Maximum likelihood estimation** chooses the parameter $\theta$ that
maximizes the probability of the observed data **D**:
![[01 - Intro Probability and Statistics-1786640441672.webp|466]]

$$\hat{\theta}_{\text{MLE}} = \arg\max_\theta \; P(D | \theta)$$
$$= \arg\max_\theta \prod_{i=1}^{n} P(x_i | \theta) \qquad \text{independent draws}$$
$$= \arg \max_\theta \prod_{i: X_i=H} \theta \prod_{i: X_i=T} (1 - \theta) \qquad \text{Identical distributed}$$
$$= \arg \max_\theta \underbrace{\theta^{\alpha H}(1 - \theta)^{\alpha T}}_{J(\theta)}$$
$$\frac{\partial J(\theta)}{\partial \theta} = \alpha_H \theta^{\alpha_H - 1}(1-\theta)^{\alpha_T} - \alpha_T \theta^{\alpha_H}(1-\theta)^{\alpha_T - 1}\Big|_{\theta = \hat{\theta}_{\text{MLE}}} = 0$$

$$\alpha_H(1-\theta) - \alpha_T \theta \Big|_{\theta = \hat{\theta}_{\text{MLE}}} = 0$$

$$\boxed{\hat{\theta}_{MLE} = \frac{\alpha_H}{\alpha_H + \alpha_T}}$$

**Example --- coin flipping**: if we observe 3 heads and 2 tails, the
MLE gives: $$\hat{\theta} = \frac{3}{5}$$which is simply the *frequency of heads* --- exactly what one would intuitively expect.

#### MLE for Gaussian Mean and Variance
Given $m$ i.i.d. samples $\{x^{(1)}, \ldots, x^{(m)}\}$ drawn from
$\mathcal{N}_x(\mu, \sigma)$, the MLE estimates are:
$$p(x | \mu , \sigma) = \frac{1}{\sqrt{2 \pi \sigma^2}}^{(- \frac{(x - \mu)^2}{2 \sigma^2})} = \mathcal{N}_x(\mu, \sigma)$$
![[01 - Intro Probability and Statistics-1786640478732.webp|439]]
Choose $\theta=(\mu, \sigma^2)$ that maximize the probability of observed data: $$\hat \theta_{MLE} = \arg \max_\theta P(D | \theta)$$
$$= \arg\max_\theta \prod_{i=1}^{n} P(x_i | \theta) \qquad \text{independent draws}$$
$$= \arg\max_\theta \prod_{i=1}^{n} \frac{1}{\sigma \sqrt{2 \pi}} e^{- \frac{(x_i - \mu)^2}{2 \sigma^2}} \qquad \text{identically distributed}$$
$$= \arg\max_{\theta=(\mu, \sigma^2)} \underbrace{( \frac{1}{\sigma \sqrt{2 \pi}})^n e^{- \frac{1}{2 \sigma^2} \sum_{i=1}^n (x_i - \mu)^2}}_{J(\theta)}$$

$$\begin{aligned}
\log(f(x_1, x_2, \dots, x_n | \sigma, \mu)) &= \log \left( \left( \frac{1}{\sigma \sqrt{2\pi}} \right)^n e^{-\frac{1}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2} \right) \\
&= n \log \frac{1}{\sigma \sqrt{2\pi}} - \frac{1}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2 \\
&= -\frac{n}{2} \log(2\pi) - n \log \sigma - \frac{1}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2
\end{aligned}$$

We call $\log(f(x_1, x_2, \dots, x_n | \sigma, \mu))$ as $\mathcal{L}$, so:

$$\frac{d\mathcal{L}}{d\mu} = \frac{1}{2\sigma^2} \sum_{i=1}^{n} (2\hat{\mu} - 2x_i) \Big|_{\mu} = 0$$

Because $\sigma^2$ should be greater than $0$:
$$\hat{\mu} = \frac{\sum_{i=1}^{n} x_i}{n}$$
$$\boxed{
\begin{aligned}
\frac{d\mathcal{L}}{d\sigma} &= -\frac{n}{\sigma} + \sum_{i=1}^{n} (x_i - \mu)^2 \sigma^{-3} = 0 \\[10pt]
\hat{\sigma}^2 &= \frac{\sum_{i=1}^{n} (x_i - \hat{\mu})^2}{n}
\end{aligned}
}$$

$$\hat{\mu}_{MLE} = \frac{1}{n} \sum_{i=1}^{n} x_i \qquad
\hat{\sigma}^2_{MLE} = \frac{1}{n} \sum_{i=1}^{n} (x_i - \hat{\mu})^2$$

### Properties of Estimators
Consider two properties of the estimator:
-   **BIAS**: how close is the estimate to the true value?
-   **VARIANCE**: how much does the estimate change for different runs (e.g. different datasets)?
![[01 - Intro Probability and Statistics-1786640532401.webp|384]]
#### Bias
The **bias** of an estimator $\hat{\theta}_m = g(x^{(1)}, ..., x^{(m)})$ for a parameter $\theta$:$$\text{Bias}(\hat{\theta}) = \mathbb{E}[\hat{\theta}] - \theta$$An estimator is called **unbiased** if $\text{Bias}(\hat{\theta}) = 0$.
-   The MLE for the Gaussian mean $\hat{\mu}_{\text{MLE}}$ is **unbiased**: $\mathbb{E}[\hat{\mu}_{\text{MLE}}] = \mu$
-   The MLE for the Gaussian variance $\hat{\sigma}^2_{\text{MLE}}$ is **biased**: $$\mathbb{E}[\hat{\sigma}^2_{\text{MLE}}] = \frac{m-1}{m}\sigma^2 \ne \sigma^2$$
The unbiased estimator for the variance is the **sample variance**:
$$S^2 = \frac{1}{m-1} \sum_{i=1}^{m} \left(x^{(i)} - \hat{\mu}\right)^2$$
#### Variance
The **variance** of an estimator indicates how much we expect it to vary as a function of the data: $$\text{Var}(\hat{\theta})$$Its square root is the **standard error** $\text{SE}(\hat{\theta})$. 
For the sample mean, the standard error is:
$$\text{SE}(\hat{\mu}) = \frac{\sigma}{\sqrt{m}}$$where $\sigma$ is the true standard deviation and $m$ is the dataset size. In practice,
$\sigma$ is estimated from the data.

When computing prediction error on a test set:
-   The number of test samples determines the accuracy of the error estimate.
-   Since the mean is approximately normally distributed (by the Central Limit Theorem), we can compute confidence intervals.
-   Algorithm $A$ is better than $B$ if the upper bound of $A$'s error is less than the lower bound of $B$'s error.
#### Mean Squared Error (MSE)
A single measure that combines both bias and variance is the **Mean Squared Error**:
$$\text{MSE}(\hat{\theta}) = \mathbb{E}\left[(\hat{\theta} - \theta)^2\right] = \text{Bias}(\hat{\theta})^2 + \text{Var}(\hat{\theta})$$
If $\text{MSE} = 0$ the estimator is perfect. This decomposition is known as the **Bias-Variance decomposition**.
## Maximum A Posteriori Estimation (MAP)
MLE ignores any prior knowledge we may have about $\theta$. **MAP
estimation** incorporates a prior $P(\theta)$ via Bayes' rule:
$$\hat{\theta}_{\text{MAP}} = \arg\max_\theta \; P(\theta | D) = \arg\max_\theta \left[ \log P(D | \theta) + \log P(\theta) \right]$$

-   If $P(\theta)$ is **uniform** (uninformative prior), then MAP $=$ MLE.
-   **Conjugate priors** are priors such that the posterior $P(\theta|D)$ has the same functional form as the prior $P(\theta)$, allowing for closed-form updates.
### Frequentist vs. Bayesian --- MLE vs. MAP
-   **Frequentist** (MLE): parameters are fixed but unknown constants; only data matters.
-   **Bayesian** (MAP / full posterior): parameters are random variables; prior beliefs are updated with data.
A practical issue with MAP: the result depends on the choice of prior,
and can perform poorly when the number of samples is small.
![[01 - Intro Probability and Statistics-1786640951208.webp|542]]
## Discriminative vs. Generative Learning
Many supervised learning methods can be viewed as estimating $P(X, Y)$.
They generally fall into two categories:

-   **Generative learning**: estimates the full joint $P(X, Y) = P(X|Y) \cdot P(Y)$, then uses Bayes' rule to compute $P(Y|X)$. Example: Naïve Bayes.
-   **Discriminative learning**: directly estimates $P(Y|X)$ without modelling the data distribution. Example: Logistic Regression.
## Limit Theorems

### Law of Large Numbers
As the number of i.i.d. samples $m \to \infty$, the sample mean
converges to the true expectation:
$$\frac{1}{m}\sum_{i=1}^{m} x^{(i)} \xrightarrow{m \to \infty} \mathbb{E}[X]$$
This justifies why MLE estimates become more accurate with more data.
### Central Limit Theorem (CLT)
Let $x^{(1)}, \ldots, x^{(m)}$ be i.i.d. samples with mean $\mu$ and variance $\sigma^2$. Then the sample mean
$\bar{X}_m = \frac{1}{m}\sum_i x^{(i)}$ satisfies:
$$\sqrt{m} \cdot \frac{\bar{X}_m - \mu}{\sigma} \xrightarrow{d} \mathcal{N}(0, 1) \quad \text{as } m \to \infty$$
In other words, regardless of the original distribution, the sample mean
is approximately normally distributed for large $m$. This is the
theoretical foundation for confidence intervals and hypothesis testing.
![[01 - Intro Probability and Statistics-1786641029355.webp|507]]
