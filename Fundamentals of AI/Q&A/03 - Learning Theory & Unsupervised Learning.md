# 1) What is a loss function and why is this choice crucial when tackling a Machine Learning problem?
The Loss is a function that measures how far off the predictions of the model are w.r.t. the correct label. 
$$
L : Y \times Y \rightarrow R_{\ge 0}
$$
The Loss maps the tuple $(y, \hat y)$ to a real positive number, where:
- $y$ is the ground truth
- $\hat y$ is the prediction of the label
### Type of Loss Functions
#### Classification
In classification a common choice is the 0-1 Loss, where:
$$
L(y, \hat y) = \begin{cases}1, & y \ne \hat y \\ 0, & y = \hat y\end{cases}
$$
In another cases, when we want to differenciate through the loss (if we use NN), the Cross Entropy is preferred:
$$
L(y, \hat y) = - \sum^C_{c=1} y_c \log(\hat y_c)
$$
- If the model is accurate ($\hat y_c \approx 1$) then the loss is low because $\log(\hat y_c)$ tends to $0$.
- If the model in not accurate, so assignees with large probability the wrong class, the loss will be wide
#### Regression
For the regression the most common choice is the Square Loss:
$$
L(Y, \hat Y) = (y - \hat y)^2
$$
### Importance of the loss
The Loss in essential, because it tells us how close our model is from the function that we want. Because of this during training we try to reduce the loss in order to have better predictions.
