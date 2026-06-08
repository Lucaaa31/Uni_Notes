## 1. The Bernoulli Model and Indicator Variables

### The M-Die Framework

Consider a six-sided die where the number $6$ has been substituted with the letter $M$.
![[Probability-1780153311605.webp]]
Assuming a fair die, the probability of rolling an $M$ is:

$$P(M) = \frac{1}{6}$$

An event that can either be true ($M$) or false ($\bar{M}$) can be mathematically coded using an event **indicator variable**. This is known as a binary random variable $X$:

$$X = \begin{cases} 1 & \text{if } M \text{ is true} \\ 0 & \text{if } M \text{ is false} \end{cases}$$

Here, $X$ is a **Bernoulli random variable** with the success parameter $p = \frac{1}{6}$.

### Multi-Trial Random Sample
If we roll this die $10$ times independently, we generate a binary random sample of $10$ **independent and identically distributed (i.i.d.)** variables:

$$X_1, X_2, \dots, X_{10} \sim \text{Bernoulli}\left(p = \frac{1}{6}\right)$$

In the language of random variables, each single component $X_i$ is a **discrete binary random variable** with a probability density (or mass) function defined as:

$$f(x) = \begin{cases} \frac{1}{6} & \text{if } x = 1 \\ \frac{5}{6} & \text{if } x = 0 \\ 0 & \text{otherwise} \end{cases}$$

> [!note] Core Corollaries
> 
> - $\mathrm{X}$ represents the formal _name_ of the random variable.
>     
> - $x$ represents the specific _possible values_ that $\mathrm{X}$ can take ($0$ or $1$).
>     
> - $f(x)$ is the _probability density_ of $X$ evaluated at the specific value $x$.
>     

For instance, the probability of rolling an $M$ on the first trial followed by nine non-$M$ rolls is written as:

$$P(X_1 = 1, X_2 = 0, X_3 = 0, \dots, X_{10} = 0) = P(M_1 \cap \bar{M}_2 \cap \bar{M}_3 \cap \dots \cap \bar{M}_{10}) = \frac{1}{6} \left(\frac{5}{6}\right)^9$$

## 2. Joint Density and Functions of Random Variables

### The Sum of Two Dice

To understand how variables interact, consider a separate example: tossing two independent, fair, regular six-sided dice (without an $M$).

Let $Y$ be a new random variable defined as the sum of the two outcomes:

$$Y = X_1 + X_2$$

Because $Y$ is a function of the random variables $X_1$ and $X_2$, it is a random variable itself. Its sample space consists of the possible sums:

$$y \in \{2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12\}$$

The density of $Y$ computed at a given point $y$ is denoted as $f(y) = P(Y = y)$.
![[Probability-1780230403065.webp]]
- **Calculating $P(Y = 2)$:** There is only one unique combination that yields a sum of $2$ (rolling a $1$ on both dice).
    
    $$P(Y = 2) = P(X_1 = 1, X_2 = 1) = f(1, 1) = \frac{1}{6} \cdot \frac{1}{6} = \frac{1}{36}$$
    
- **Calculating $P(Y = 3)$:** There are two disjoint combinations that yield a sum of $3$ (a $1$ then a $2$, or a $2$ then a $1$).
    
    $$P(Y = 3) = P((X_1 = 1 \cap X_2 = 2) \cup (X_1 = 2 \cap X_2 = 1))$$
    
    $$= f(1, 2) + f(2, 1) = \left(\frac{1}{6} \cdot \frac{1}{6}\right) + \left(\frac{1}{6} \cdot \frac{1}{6}\right) = \frac{2}{36}$$
    

### The Joint Random Vector

Returning to our $10$ rolls of the $M$-die, we can collect all ten tracking variables into a single mathematical structure called a **random vector**: $\mathbf{X} = (X_1, X_2, \dots, X_{10})$.

The probability of observing any specific sequence of outcomes is dictated by its **joint density function**:

$$f(x_1, x_2, \dots, x_{10}) = P(X_1 = x_1, X_2 = x_2, \dots, X_{10} = x_{10})$$

Because each trial is entirely independent, the joint density simplifies to the product of individual marginal densities ($\prod$):

$$f(x_1, x_2, \dots, x_{10}) = \prod_{i=1}^{10} f_X(x_i)$$

Using our earlier calculation:

$$f(1, 0, 0, \dots, 0) = \frac{1}{6} \left(\frac{5}{6}\right)^9$$

> [!note]
> Due to the commutative property of multiplication, every individual sequence that contains exactly one $1$ and nine $0$s (regardless of position) shares this exact same joint density value.

## 3. The Binomial Distribution

Instead of tracking the exact sequence of rolls, we frequently only care about total counts. Let's define a new random variable $Y$ as the **total number of times $M$ comes out** across the $10$ independent rolls:

$$Y = \sum_{i=1}^{10} X_i$$

The possible values for this aggregate variable are $y \in \{0, 1, 2, \dots, 10\}$. We can construct its density function $f_Y(y)$ by analyzing the probability of getting exactly $y$ successes:

- $f_Y(0) = \left(\frac{5}{6}\right)^{10}$
    
- $f_Y(1) = 10 \left(\frac{1}{6}\right)^1 \left(\frac{5}{6}\right)^9$
    

Generalizing this for any value of $y$:

$$f_Y(y) = \binom{10}{y} \left(\frac{1}{6}\right)^y \left(1 - \frac{1}{6}\right)^{10-y} = \frac{10!}{y!(10-y)!} \left(\frac{1}{6}\right)^y \left(\frac{5}{6}\right)^{10-y}$$

### Formula Components

- **$\binom{10}{y}$:** The binomial coefficient, which counts the total number of distinct sequence arrangements that contain exactly $y$ successes.
    
- **$\left(\frac{1}{6}\right)^y \left(\frac{5}{6}\right)^{10-y}$:** The standalone probability of any single, specific sequence containing exactly $y$ successes and $(10-y)$ failures.
    

> [!definition] Binomial Random Variable
> When an aggregate variable behaves this way, $Y$ is called a **Binomial random variable** defined by two key parameters:
> 
> - $n = 10$ (the total number of independent trials)
>     
> - $p = \frac{1}{6}$ (the stable probability of success on a single trial)
    

## 4. Categorical Coding and the Multinomial Distribution

### Expanding Beyond Binary Outcomes

What happens if we want to track more than just a simple "true or false" condition? Let's classify the outcomes of our $M$-die into three distinct categories ($C$):

$$C = \begin{cases} m & \text{if } M \text{ comes out} \\ e & \text{if a } 2 \text{ or } 4 \text{ comes out (Even)} \\ o & \text{if a } 1, 3, \text{ or } 5 \text{ comes out (Odd)} \end{cases}$$

The underlying probabilities for each category are:

$$P(C = m) = \frac{1}{6}, \quad P(C = e) = \frac{2}{6}, \quad P(C = o) = \frac{3}{6}$$

To process this categorical variable numerically, we apply **one-hot encoding** to map the states into a vector:

$$X = \begin{cases} (1, 0, 0) & \text{if } C = m \\ (0, 1, 0) & \text{if } C = e \\ (0, 0, 1) & \text{if } C = o \end{cases}$$

> [!note] Vector Simplification
> Because the three states must add up to $1$, the final state is completely redundant. If the vector is not $m$ or $e$, it must be $o$. Therefore, handwritten notes often simplify this into a $2$-dimensional vector where the third state is implied when both tracking slots are $0$:
> 
> - $P(X = (1, 0)) = \frac{1}{6}$
>     
> - $P(X = (0, 1)) = \frac{2}{6}$
>     
> - $P(X = (0, 0)) = \frac{3}{6}$
>     

### Multi-Class Aggregate Counts

Suppose we toss this die $10$ times independently and aggregate our group counts:

- $Y_1 = \text{number of } m\text{'s observed}$
    
- $Y_2 = \text{number of } e\text{'s observed}$
    
- $Y_3 = 10 - Y_1 - Y_2 = \text{number of } o\text{'s observed}$
    

This can be written as the sum of our one-hot encoded vectors: $(Y_1, Y_2) = \sum_{i=1}^{10} (X_{i1}, X_{i2})$.

To find a specific joint probability—such as rolling exactly $3$ $m$'s, $2$ $e$'s, and $5$ $o$'s—we use a multinomial structure:

$$P(Y_1 = 3, Y_2 = 2, Y_3 = 5) = \binom{10}{3 \quad 2 \quad 5} \left(\frac{1}{6}\right)^3 \left(\frac{2}{6}\right)^2 \left(\frac{3}{6}\right)^5 = \frac{10!}{3! 2! 5!} \left(\frac{1}{6}\right)^3 \left(\frac{2}{6}\right)^2 \left(\frac{3}{6}\right)^5$$

Where $\frac{10!}{3! 2! 5!}$ calculates the total number of unique ordering permutations for a sequence like $mmmeeooooo$, and each independent configuration carries the exact same probability.

### Generalization to the Multinomial Distribution

> [!definition] Multinomial Distribution
> If we generalize this framework to $n$ independent trials that result in $D$ possible alternatives, each with a stable probability $p_d$ such that $\sum_{d=1}^{D} p_d = 1$, the collection of category counts $(Y_1, Y_2, \dots, Y_D)$ forms a **multinomial random vector**. Its joint probability mass function is formally defined as:
> 
> $$P(Y_1 = y_1, Y_2 = y_2, \dots, Y_D = y_D) = \frac{n!}{\prod_{d=1}^{D} y_d!} \prod_{d=1}^{D} p_d^{y_d}$$
> 
> _(Note: One component remains mathematically redundant because $Y_D = n - \sum_{d=1}^{D-1} Y_d$.)_

## 5. Summary: Probability vs. Statistics

To solidify these concepts, consider rolling $5$ $M$-dice and recording the experimental outcome:

$$y_1 = 0, \quad y_2 = 3 \quad (\implies y_3 = 2)$$

This observation is concrete data. If you were to repeat the experiment out of curiosity, you could compute the probability of getting this exact same distribution again using the multinomial distribution:

$$\mathbb{P}(\text{"second outcome same as first"}) = \mathbb{P}(Z_1 = 0, Z_2 = 3) = \frac{5!}{0!3!2!} \left(\frac{1}{6}\right)^0 \left(\frac{2}{6}\right)^3 \left(\frac{3}{6}\right)^2$$

> [!summary] Probability vs. Statistics
> This highlights a vital distinction in data science:
> 
> - **Probability Problem:** We already know the exact structural mechanics of our data generator (a fair, six-sided die). We use this framework to predict the likelihood of future data.
>     
> - **Statistical Problem:** We observe experimental data counts first, and must work backwards to infer the unknown properties of the system that generated them.