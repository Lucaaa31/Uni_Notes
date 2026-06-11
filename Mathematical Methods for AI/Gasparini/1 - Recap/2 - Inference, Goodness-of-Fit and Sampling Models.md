## 1. Point Estimation and Confidence Intervals for Proportions

### The Pencils Framework

Suppose you have a population of $30$ pencils colored **Red (R)**, **Blue (B)**, and **Green (G)**, but the exact composition is unknown. To infer the composition, we sample $n = 5$ pencils at random **with replacement** (allowing us to use multinomial probabilities).

After performing the experiment, we observe the following counts:

- $y_1 = \#R = 4$
    
- $y_2 = \#B = 1$
    
- $y_3 = \#G = 0$
    

Our first statistical task is to estimate $p_1$ (the true proportion of Red pencils in the population) and calculate a **95% Confidence Interval (CI)**.

### The Wald (Asymptotic) Interval

The standard formula for a confidence interval for a proportion is:

$$\hat{p}_1 \pm z_{\frac{\alpha}{2}} \sqrt{\frac{\hat{p}_1(1 - \hat{p}_1)}{n}}$$

Where:

- **$\hat{p}_1$ (Point Estimate):** The center of your interval. In this case, $\hat{p}_1 = \frac{4}{5} = 0.8$. This is also the **Maximum Likelihood (ML)** estimate.
    
- **$z_{\frac{\alpha}{2}}$ (Critical Value / Multiplier):** A value from the standard normal distribution based on your desired confidence level. For a 95% confidence level, $\alpha = 0.05 \implies \frac{\alpha}{2} = 0.025 \implies z_{0.025} = 1.96$.
    
- **$\sqrt{\frac{\hat{p}_1(1 - \hat{p}_1)}{n}}$ (Standard Error):** Measures the uncertainty of your estimate. Since the sample size ($n$) is in the denominator, a larger sample reduces this error, resulting in a narrower and more precise interval.
    

### Calculation

$$\left[ \frac{4}{5} - 1.96 \sqrt{\frac{\frac{4}{5} \cdot \frac{1}{5}}{5}}, \quad \frac{4}{5} + 1.96 \sqrt{\frac{\frac{4}{5} \cdot \frac{1}{5}}{5}} \right]$$

> [!warning]
> This asymptotic interval is an approximation. For such a small sample size ($n = 5$), its coverage probability is too low. In practice, better exact procedures (e.g., the **Clopper-Pearson** interval) should be used.

## 2. Pearson's $\chi^2$ Goodness-of-Fit Test

Suppose, based on a prior theory, you want to test the null hypothesis ($H_0$) of equal proportions (a uniform distribution):

$$H_0: p_1 = p_2 = p_3 = \frac{1}{3}$$

To quantify our skepticism towards $H_0$ objectively, we use a **Goodness-of-Fit test**. Any statistical test requires a decision rule (e.g., _"Reject $H_0$ if..."_). Here, we reject $H_0$ if the "distance" between our observed sample frequencies and the theoretical frequencies is too large.

### The Chi-Square Statistic

In 1900, Karl Pearson proposed the $\chi^2$ statistic:

> [!definition] Pearson's $\chi^2$ Statistic
> $$\chi^2 = \sum_{i=1}^{D} \frac{(N_i - n p_i)^2}{n p_i}$$
> 
> Where:
> 
> - **$D$:** Number of classes (here, $D = 3$).
>     
> - **$N_i$:** Observed count of the $i$-th class ($N_1 = Y_1, N_2 = Y_2, N_3 = n - Y_1 - Y_2$).
>     
> - **$p_i$:** Proportions under the null hypothesis ($H_0$).
>     
> - **$n p_i$:** Expected counts if $H_0$ were true. In our case: $5 \cdot \left(\frac{1}{3}\right) = \frac{5}{3}$.
>     
> 
> The numerator represents the squared Euclidean distance, while the denominator acts as a correction factor based on expected variance.

### Test Distribution and P-Value

> [!theorem] Pearson's Theorem
> If $H_0$ is true, the test statistic asymptotically follows a Chi-square distribution with $D - 1$ degrees of freedom:
> 
> $$\chi^2 \sim \chi^2(D - 1)$$
> 
> - If $H_0$ is true, $\chi^2$ should be close to the degrees of freedom ($D - 1 = 2$).
>     
> - If $\chi^2$ is significantly larger, we reject $H_0$.
> 
> ![[2 -  Inference, Goodness-of-Fit, and Sampling Models-1780945827743.webp]]
> 
The decision is guided by the **p-value** (the area to the right of the observed  value). The lower the p-value, the more extreme the observed distance, and the less plausible  becomes.

### Example Calculation

Given $N_1 = 4$, $N_2 = 1$, $N_3 = 0$, and expected counts $n p_i = \frac{5}{3}$:

$$\chi^2_{\text{obs}} = \frac{(4 - \frac{5}{3})^2}{5/3} + \frac{(1 - \frac{5}{3})^2}{5/3} + \frac{(0 - \frac{5}{3})^2}{5/3} = 5.2$$

The corresponding p-value is:

$$\mathbb{P}(\chi^2(2) > 5.2) \approx 0.074$$

## 3. Sampling Without Replacement: The Multivariate Hypergeometric Distribution

Sampling _with_ replacement allows us to treat trials as independent and use multinomial probabilities. 
If we instead sample **without replacement** from a finite population, independence is lost.

Let:

- $N = \text{total population size} = 30$
    
- $n = \text{sample size} = 5$
    
- $N_1, N_2, N_3$ be the true (unknown) counts of R, B, and G pencils in the population ($N_1 + N_2 + N_3 = 30$).
    

> [!definition] Multivariate Hypergeometric Distribution
> If we assume a specific composition, such as $N_1 = 15$, $N_2 = 8$, and $N_3 = 7$, the joint probability of obtaining specific counts $Y_1, Y_2, Y_3$ follows a **Multivariate Hypergeometric distribution**:
> 
> $$\mathbb{P}(Y_1 = 2, Y_2 = 3, Y_3 = 0) = \frac{\binom{15}{2}\binom{8}{3}\binom{7}{0}}{\binom{30}{5}}$$

## 4. Categorical Data Analysis in Practice & Software (JASP)

When analyzing data using software like JASP or R, the p-value dictates the strength of evidence against $H_0$. A smaller p-value implies stronger evidence against the null hypothesis. For instance, a p-value of $0.07$ represents relatively weak evidence against $H_0$ (failing to reject it at the typical $\alpha = 0.05$ threshold).

### Scaled-Up Example

Consider a larger categorical sample with the following observed counts ($n = 57$):

- **Red (R):** 40
    
- **Blue (B):** 10
    
- **Green (G):** 7
    

In JASP, a Multinomial Goodness-of-Fit test evaluates $H_0: p_1 = p_2 = p_3 = \frac{1}{3}$. Under $H_0$, the observed random vector of counts $(N_1, N_2, N_3)$ follows a Multinomial distribution:

$$\mathbf{Y} \sim \text{Multinomial}\left(n = 57, \mathbf{p} = \left(\frac{1}{3}, \frac{1}{3}, \frac{1}{3}\right)\right)$$

## 5. Mathematical Framework: Multinoulli and Multinomial Random Vectors

A multivariate discrete random vector collects the counts of different categories across multiple observations:
$$(N_1, \dots, N_D) \sim \text{Multinomial}(n, p_1, \dots, p_D)$$
Where:

- $D =$ # of classes of categorical variable (also called factor)
    
- $n =$ # of observations (sample size)
    
- $N_i =$ # of observations in class $i$

### Mapping Binary to Multi-Class Trackers

The relationship between single-trial indicators and multi-trial aggregations scales as follows:

| **Number of Classes (D)** | **Single-Trial Variable Type** | **Multi-Trial Aggregated Counts** |
| ------------------------- | ------------------------------ | --------------------------------- |
| **$D = 2$**               | Bernoulli (binary)             | Binomial                          |
| **$D > 2$**               | Multinoulli                    | Multinomial                       |

### One-Hot Encoding (Multinoulli Vector)

To process categorical data mathematically, classes are converted into numerical vectors via **one-hot encoding**:

- $\textbf{R} = (1, 0, 0)$
    
- $\textbf{B} = (0, 1, 0)$
    
- $\textbf{G} = (0, 0, 1)$
    

A single trial results in a **Multinoulli random vector** $\mathbf{X} \sim \text{Multinoulli}(p_1, p_2, p_3)$, which is a special case of the Multinomial distribution where $n = 1$.

Therefore, a categorical variable can be viewed in two ways:
1. A one-dimensional categorical variable (factor) with $D$ levels and probabilities $p_1, \dots, p_D$.
    
2. A multi-dimensional **Multinoulli** random vector under one-hot encoding.
    

When summing $n$ independent, identically distributed (i.i.d.) Multinoulli vectors, we obtain a **Multinomial random vector**:

$$\mathbf{Y} = \sum_{i=1}^{n} \mathbf{X}_i \sim \text{Multinomial}(n, p_1, \dots, p_D)$$

> [!summary] Summary of Discrete Random Vectors
> 
> The three foundational discrete random vectors covered up to this point are:
> 
> - **$\text{Multinomial}(n, p_1, \dots, p_D)$:** Counts of $D$ categories over $n$ independent trials with replacement.
>     
> - **$\text{Multinoulli}(p_1, \dots, p_D)$:** A special case of the Multinomial vector for a single trial ($n = 1$).
>     
> - **$\text{Hypergeometric}(n, C_1, \dots, C_D)$:** Counts of categories when sampling _without_ replacement from a finite population with known category capacities $C_i$.