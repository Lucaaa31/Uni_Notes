
> [!theorem] Random Variable 
> - $X$ name of the variable
> - $x$ possible value that $X$ can assume
> - $f(x)$ probability density of $X$ evaluated on the specific value $x$

> [!theorem] Joint Random Vector
> - Random vector of probabilities: $$\mathbf{X} = (X_1, X_2, \dots, X_{10})$$
> - Joint density function: $$f(x_1, x_2, \dots, x_{10}) = P(X_1 = x_1, X_2 = x_2, \dots, X_{10} = x_{10})$$
> - Because the variables are independents the **joint density** is given by the moltiplications of the marginal densities: $$f(x_1, x_2, \dots, x_{10}) = \prod_{i=1}^{10} f_X(x_i)$$

> [!definition] Binomial Random Variable
>  $$P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}$$
> 
> - $n =$ the total number of independent trials
>     
> - $p =$ the stable probability of success on a single trial
> - $k =$ number of success that we want to measure 

> [!definition] Multinomial Distribution
> $$P(Y_1 = y_1, Y_2 = y_2, \dots, Y_D = y_D) = \frac{n!}{\prod_{d=1}^{D} y_d!} \prod_{d=1}^{D} p_d^{y_d}$$
> 
> - $n =$ number of trials
> - $D =$ number of possible outcomes
> - $p_d =$ probability of a given event
> - $\frac{n!}{\prod_{d=1}^{D} y_d!}$ = multinomial coefficient, representing the number of ways to partition $n$ items into $D$ distinct groups of sizes $y_1, y_2, \dots, y_D$
> - $y_d =$ number of success for the value $d$ across all trials

