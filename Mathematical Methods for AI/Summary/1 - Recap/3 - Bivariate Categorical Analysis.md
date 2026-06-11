
> [!info] Notations
> - **$N_{ij}$:** The observed frequency count in the cell corresponding to the $i$-th row and $j$-th column (where row variable $X = i$ and column variable $Y = j$).
>
>- **$N_{i\cdot}$ (Row Marginals):** The total count for the $i$-th row, summing across all columns:
 >   $$N_{i\cdot} = \sum_{j} N_{ij}$$
>    
>- **$N_{\cdot j}$ (Column Marginals):** The total count for the $j$-th column, summing across all rows:
 >     $$N_{\cdot j} = \sum_{i} N_{ij}$$

#### The Chi-Square ($\chi^2$) Test of Independence
Test used in order to understand if there is a relationship (and how strong it is) between two variable.
> [!definition] Independence Hypothesis
> If the two variables are completely independent, the joint cell probability $p_{ij}$ must equal the product of their respective marginal probabilities:
> 
> $$H_0: p_{ij} = p_{i\cdot} \cdot p_{\cdot j}$$
> 
> Where the theoretical row and column marginal probabilities are defined as:
> 
> $$p_{i\cdot} = \sum_{j} p_{ij} \quad \text{and} \quad p_{\cdot j} = \sum_{i} p_{ij}$$

> [!definition] Expected Count $E_{ij}$
> Represents the theoretical frequency we would expect to see if $H_0$ were perfectly true: $$E_{ij} = n \cdot \left(\frac{N_{i\cdot}}{n}\right) \cdot \left(\frac{N_{\cdot j}}{n}\right) = \frac{N_{i\cdot} \cdot N_{\cdot j}}{n} = \frac{\text{Row total } \times \text{Column total}}{\text{Grand Total}}$$

So, if the $H_0$ of independence holds true, as the sample size $n$ grows, this test statistic asymptotically follows a Chi-Square distribution:
 $$\chi^2 \sim \chi^2_{\nu}$$
 The **degrees of freedom ($\nu$)** are strictly determined by the physical dimensions of the contingency table:
$$\nu = (\#\text{rows} - 1) \times (\#\text{columns} - 1)$$
Decision rule:
- **Large $\chi^2$ values:** Indicate that the observed counts are very far from what independence predicts $\implies$ Evidence against $H_0$.
    
- **Small $\chi^2$ values:** Indicate that the observed data fits the independence model well $\implies$ Fail to reject $H_0$.
