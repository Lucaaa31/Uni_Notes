## 1. Structure of a Contingency Table

When evaluating two categorical variables simultaneously—such as a three-level factor **Color** ($X \in \{R, B, G\}$) and a binary factor **Mark?** ($Y \in \{\text{yes}, \text{no}\}$)—we record their cross-classifications. This matrix structure is known as a **Contingency Table** or a table of crossed counts.

### The Observed Counts Matrix

Based on a total sample size of $n = 107$, the observed frequencies are structured as follows:

|**Color**|**Mark? (no)**|**Mark? (yes)**|**Row Marginals (Ni⋅​)**|
|---|---|---|---|
|**Red (R)**|$30$|$7$|**$37$**|
|**Blue (B)**|$15$|$20$|**$35$**|
|**Green (G)**|$20$|$15$|**$35$**|
|**Column Marginals ($N_{\cdot j}$)**|**$65$**|**$42$**|**Total ($n = 107$)**|

### Mathematical Notation

- **$N_{ij}$:** The observed frequency count in the cell corresponding to the $i$-th row and $j$-th column (where row variable $X = i$ and column variable $Y = j$).
    
- **$N_{i\cdot}$ (Row Marginals):** The total count for the $i$-th row, summing across all columns:
    
    $$N_{i\cdot} = \sum_{j} N_{ij}$$
    
- **$N_{\cdot j}$ (Column Marginals):** The total count for the $j$-th column, summing across all rows:
    
    $$N_{\cdot j} = \sum_{i} N_{ij}$$
    

### The Vectorized Joint Model

Instead of viewing the data as a matrix, we can unroll the cells into a single, comprehensive joint vector:

$$\mathbf{Y} = (R_{\text{no}}, R_{\text{yes}}, B_{\text{no}}, B_{\text{yes}}, G_{\text{no}}, G_{\text{yes}})$$

Mathematically, this allows us to treat the table as a single **joint Multinomial random vector** driven by a probability vector containing individual cell probabilities:

$$\mathbf{p} = (p_{11}, p_{12}, p_{21}, p_{22}, p_{31}, p_{32})$$

## 2. The Chi-Square ($\chi^2$) Test of Independence

The primary objective in bivariate categorical analysis is determining whether a statistically significant relationship exists between the two variables (e.g., whether the _Color_ of an item is independent of its _Mark?_).

### The Null Hypothesis ($H_0$)

> [!definition] Independence Hypothesis
> If the two variables are completely independent, the joint cell probability $p_{ij}$ must equal the product of their respective marginal probabilities:
> 
> $$H_0: p_{ij} = p_{i\cdot} \cdot p_{\cdot j}$$
> 
> Where the theoretical row and column marginal probabilities are defined as:
> 
> $$p_{i\cdot} = \sum_{j} p_{ij} \quad \text{and} \quad p_{\cdot j} = \sum_{i} p_{ij}$$

### Calculating Expected Frequencies ($E_{ij}$)

To test how much our observed data deviates from the independence model, we calculate an **Expected Count ($E_{ij}$)** for each cell. This represents the theoretical frequency we would expect to see if $H_0$ were perfectly true:

$$E_{ij} = n \cdot \hat{p}_{i\cdot} \cdot \hat{p}_{\cdot j}$$

The estimated marginal probabilities are derived directly from our sample matrix:

$$\hat{p}_{i\cdot} = \frac{N_{i\cdot}}{n} \quad \text{and} \quad \hat{p}_{\cdot j} = \frac{N_{\cdot j}}{n}$$

By substituting these sample estimates back into the expected count formula, the expression simplifies directly to:

$$E_{ij} = n \cdot \left(\frac{N_{i\cdot}}{n}\right) \cdot \left(\frac{N_{\cdot j}}{n}\right) = \frac{N_{i\cdot} \cdot N_{\cdot j}}{n}$$

> [!tip] Rule of Thumb
> The expected count for any cell is simply its matching **(Row Total $\times$ Column Total) / Grand Total**.

## 3. Evaluation Criteria and Decision Rules

### The Test Statistic

To quantify the total discrepancy across the entire table, statistical software like JASP computes the **Contingency Chi-Square ($\chi^2$)** test statistic:

$$\chi^2 = \sum_{i} \sum_{j} \frac{(N_{ij} - E_{ij})^2}{E_{ij}}$$

### Sampling Distribution and Degrees of Freedom

> [!theorem] Asymptotic Distribution of the Contingency $\chi^2$
> Assuming the null hypothesis ($H_0$) of independence holds true, as the sample size $n$ grows, this test statistic asymptotically follows a Chi-Square distribution:
> 
> $$\chi^2 \sim \chi^2_{\nu}$$
> 
> The **degrees of freedom ($\nu$)** are strictly determined by the physical dimensions of the contingency table:
> 
> $$\nu = (\#\text{rows} - 1) \times (\#\text{columns} - 1)$$
> 
> For our specific $3 \times 2$ table:
> 
> $$\nu = (3 - 1) \times (2 - 1) = 2 \times 1 = 2 \text{ degrees of freedom}$$

### Decision Rule

- **Large $\chi^2$ values:** Indicate that the observed counts are very far from what independence predicts $\implies$ Evidence against $H_0$.
    
- **Small $\chi^2$ values:** Indicate that the observed data fits the independence model well $\implies$ Fail to reject $H_0$.
    

> [!important] Statistical Decision Rule
> We reject the null hypothesis $H_0$ if our calculated $\chi^2$ value is exceptionally large. In practical software output, this corresponds to a **p-value** that falls below our chosen significance threshold (typically $p < 0.05$). Falling below this threshold indicates that the two categorical variables are statistically dependent.