# Artificial Neural Network
An ANN is a **computational model** inspired by the biological neural networks that constitute the human brain. It is the foundation of deep learning and it is used, for example, to recognize patterns in the data, cluster them or make predictions. 
An ANN is composed by a set of **interconnected processed units** called **neurons**, that are organized in layers.

From a structural point of view, an ANN is composed of:
- **Input Layer:** it receives the raw data (e.g. an image), this layer doesn't learn weights, it simply holds data and pass it forward
- **Hidden Layers:** often there are more than one. Here the actual computation and feature extraction happen. When a network has a lot of them is called Deep Neural Network
- **Output Layer:** it produces the final prediction or the classification result.
## Neuron
Each neuron processes the input using a **linear transformation** followed by a **non-linear activation function**. This allow the network overcome the limitations of a simple linear model and find non-linear relationships in the data.
Mathematically, the output of a neuron can be computed as:
$$
y = f(\sum_{i=0}^n x_i w_i +b)
$$
where:
- $x$ is the inputs vector 
- $w$ is the vector of the weights
- $b$ is the bias term
- $f()$ is the activation function (e.g. ReLU, Sigmoid, tanh) which introduces the non-linearity in the model
## Learning Process
The **learning** in an ANN consists into an optimization problem-finding a specific configuration of million of weights and biases that minimize the network's prediction error.
It is an iterative process that involves three phases:
- **Forward Propagation:** input data moves through the network passing in all the layers in order to output a prediction
- **Loss Calculation:** a loss function (e.g MSE, Cross-Entropy) evaluates the performance of the network by confronting the differences from the network's prediction and the actual target value
- **Backpropagation:** the network calculates the gradient of the loss function w.r.t. every weight using the chain rule of calculus. An optimization algorithm (e.g. the Gradient Descent) updates the values of the weights in order minimize the loss

---
# The Gradient Descent Algorithm
IN the GD we consider a function $L(W)$, where $W$ are the **parameters** of our model. 
The main idea of this algorithm is to compute the gradient of the loss function w.r.t. the weight:
$$
\nabla_W L(W) = [\frac{\partial L}{\partial w_1}, \frac{\partial L}{\partial w_2}, ..., \frac{\partial L}{\partial w_n} ]
$$
where each $\frac{\partial L}{\partial w_i},$ represents how fast the loss function increases with a change in $w_i$.

We are trying to minimize the loss function, but the gradient points always towards the greater increase, so we have to move toward the negative gradient $- \nabla_W L(W)$.

We denote $W^t$ the weight at the step $t$ and what we do is set
$$
W^{t+1} = W^t - \alpha \nabla_W L(W^t)
$$
where $\alpha$ is the learning rate.
## Choosing the Learning Rate
There are many different ways to choose the LR, for example it cab be a small constant (e.g. 0.001), other times it can change through different updates.
In this second case, the way to choose the LR at each weight update we could use the **line search**. It consist into choosing the LR that produces the lowest loss function:
$$
\alpha^* = \arg\min_\alpha L(W_t - \alpha \nabla (W_t))
$$
## Convergence and Limitations
This approach guarantees convergence to local minima or saddle points under mild conditions, although it might take a long time. This delay is especially prevalent because of saddle points, where the gradient is zero, but they are neither minima (which would be good enough for our purposes) nor maxima (from which the algorithm would quickly escape).

Gradient Descent is an algorithm used in the training phase to choose the best parameters for a model, minimizing the error between the model's prediction and the ground truth.

### Main Limitations
1. **Slow convergence:** Especially in complex loss landscapes with many saddle points.
2. **Poor scaling with dataset size:** In a standard Batch Gradient Descent algorithm, the total cost function $\mathcal{L}(W)$ is evaluated by considering all samples in the dataset for a single update. Mathematically, the overall gradient is the average of the gradients computed for every single sample $m$:
$$\nabla_W \mathcal{L}(W) = \frac{1}{m} \sum_{i=1}^m \nabla_W L(x^i, y^i, W)$$

Because evaluating this sum requires parsing the entire dataset (where $m$ is the total number of samples) just to perform one step ($W^{t+1}$), the computational cost per update becomes prohibitive for large datasets.

This scaling problem is directly addressed by the **Stochastic Gradient Descent (SGD)** algorithm.

---
# Stochastic Gradient Descent Algorithm
One of the main limitations of the classic Gradient Descent is that performs poorly with large datasets because it has to compute the loss function over the whole training set.
SGD has been made in order to overcome this problem and nowadays it is one of the most used algorithm for the deep learning models.

We recall that, for computing the loss function with the standard Gradient Descent we have:
$$
\mathcal L (W) = \frac{1}{m} \sum_{i=1}^m L(x^i, y^i, W)
$$where:
- is the loss of one sample of input $x$, label $y$ and parameters $W$.

Its gradient would then be equal to:
$$  
\nabla_W \mathcal L (W) = \frac 1 m \sum_{i=1}^m \nabla_W L(x^i, y^i, W)  
$$

so we’d have to compute the gradient of the loss of every single sample and then average them out.

SGD proposes an important change:
> The gradient we use in GD is an expectation that can be approximately estimated using a small set of sample.

The idea is to choose a certain number of samples $m$ from the training set and compute the gradient:
$$
g^{(t)} = \frac 1{m} \sum_{i=1}^{m} \nabla_W L(X^i, Y^i, W)
$$
The mathematical basis for this is that:
$$
g = E[g^{(t)}]
$$
Meaning that $g^{(t)}$ is an unbiased estimator for the true gradient over the whole dataset.

