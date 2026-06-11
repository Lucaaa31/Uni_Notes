
> [!definition] The Wald Interval
> $$\hat{p}_1 \pm z_{\frac{\alpha}{2}} \sqrt{\frac{\hat{p}_1(1 - \hat{p}_1)}{n}}$$
> - $\hat{p}_1 =$ **Point Estimated**, it is the center of the interval. And, also **Maximum Likelihood (ML)** estimate.
> -  $z_{\frac{\alpha}{2}} =$  **Critical Value**, a value taken by the normal standard distribution based from the interval
> - $\sqrt{\frac{\hat{p}_1(1 - \hat{p}_1)}{n}} =$ **Standard Error**
> 
> Warning: This is an approximation, for low data in practice we use other procedures e.g., the **Clopper-Pearson** interval).

### Goodness-of-Fit
> [!definition] Definition
> For an hypothesis $H_0$ we quantify our skepticism using Goodness of Fit Test.
> Ee decide a **decision rule** that will be our filter:
> $$\begin{cases} H_0 = \text{ rejected } & \text{ if ...}  \\ H_0 = \text{ accetted } & \text{ if ...} \end{cases}$$
> 
> There are various methods to calculate the goodness of a fit.

> [!definition] Pearson's $\chi^2$ Statistic
> A method to calculate the goodness of a fit, using results calculated using $H_0$ and the theoretical results:
> $$\chi^2 = \sum_{i=1}^{D} \frac{(N_i - n p_i)^2}{n p_i}$$
> 
> Where:
> 
> - **$D$:** Number of classes .
>     
> - **$N_i$:** Observed count of the $i$-th class .
>     
> - **$p_i$:** Proportions under the null hypothesis ($H_0$).
>     
> - **$n p_i$:** Expected counts if $H_0$ were true.
>     
> 
> The numerator represents the squared Euclidean distance, while the denominator acts as a correction factor based on expected variance.

> [!theorem] How to interpret the Pearson Theorem
> If $H_0$ is true, the test statistic asymptotically follows a Chi-square distribution with $D - 1$ degrees of freedom:
> 
> $$\chi^2 \sim \chi^2(D - 1)$$
> 
> - If $H_0$ is true, $\chi^2$ should be close to the degrees of freedom.
>     
> - If $\chi^2$ is significantly larger, we reject $H_0$.
> 
> ![[2 -  Inference, Goodness-of-Fit, and Sampling Models-1780945827743.webp|400]]
> 
The decision is guided by the **p-value** (the area to the right of the observed  value). 
The lower the p-value, the more extreme the observed distance, and the less plausible  becomes.

### The Multivariate Hypergeometric Distribution (No replacement)

> [!warning] NO REPLACEMENT = NO INDIPENDENCE

> [!definition] Multivariate Hypergeometric distribution
> $$\mathbb{P}(Y_1 = y_1, Y_2 = y_2, \dots, Y_c = y_c) = \frac{\prod_{i=1}^{c} \binom{N_i}{y_i}}{\binom{N}{n}} = \frac{\binom{N_1}{y_1}\binom{N_2}{y_2}\dots\binom{N_c}{y_c}}{\binom{N}{n}}$$
> - $c =$ number of categories of the population
> - $N_i =$ number of elements for the i-th category
> - $N =$ total population
> - $n =$ number of samples extracted

### Multinoulli and Multinomial Random Vectors
> [!definition] Multinomial Random Variable
> $$(N_1, \dots, N_D) \sim \text{Multinomial}(n, p_1, \dots, p_D)$$
>
Where:
>
>- $D =$ # of classes of categorical variable (also called factor)
  >  
>- $n =$ # of observations (sample size)
  >  
>- $N_i =$ # of observations in class $i$
>

> [!definition] Multinoulli vs Multinomial
> 
| **Number of Classes (D)** | **Single-Trial Variable Type** | **Multi-Trial Aggregated Counts** |
| ------------------------- | ------------------------------ | --------------------------------- |
| **$D = 2$**               | Bernoulli (binary)             | Binomial                          |
| **$D > 2$**               | Multinoulli                    | Multinomial                       |
> **Multinoulli:** particular Multinomial where $n=1$, so just $1$ trial


