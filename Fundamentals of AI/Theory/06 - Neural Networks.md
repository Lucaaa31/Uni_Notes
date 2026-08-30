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

---
# Backpropagation Algorithm
Backpropagation is the foundational algorithm used for training feedforward ANN. It provides a way for computing the gradients of the loss function w.r.t. the NN's weights and biases.

Backpropagation is a practical application of the **chain rule** from differential calculus, it allow the network to understand exactly **how much each parameter contributed to the final output error**.

## Backpropagation in NN
A neural network can be seen as a **composition of functions**, where each layer receives an input and applies a transformation.
Training a NN using backpropagation occurs in an iterative cycle:
### 1. Forward Pass
Input data is fed into the network. The data propagates through the hidden layer, each layer apply a linear transformation followed by a non-linear activation. At the end of this process data arrive at the output layer that produces the prediction. 
### 2. Loss Calculation
The prediction will be used to compute a certain loss function (s.t. MSE or Cross-Entropy) by evaluating, using a certain distance, the difference between the network's generated prediction and the ground-truth target. The loss calculation produces a singe scalar value representing the total error.
### 3. Backward Pass
The Backpropagation computes the partial derivate of the loss function w.r.t. each weight and bias. Starting by the output layer, the last one, it moves backward to the input layer applying iteratively the chain rule. 
This calculates an error gradient for every node, indicating how a change in a specific weight will impact the overall loss.
### 4. Weight Update
An optimization algorithm , such as GD (or one of his variances e.g. Adam) uses the calculated gradients to update the network's parameters (weight and bias). In particular, weights are adjusted in the opposite direction of the gradient in order to minimize the loss.

--- 
# Batches, Epochs, and Learning Rate within the context of optimization algorithms for ML
In the context of machine learning optimization, specifically when using Gradient Descent and its variants, the concepts of batches, epochs, and the learning rate are fundamental parameters that determine how a model learns from data.

## 1. Learning Rate
The LR determinates the step taken at each iteration while moving toward the minimum of the loss function.
In the GD, the parameter update rule is defined as:
$$\theta_{t+1} = \theta_t - \eta \nabla J(\theta_t)$$
where:
- $\theta$ is the model weights
- $\nabla J(\theta_t)$ is the gradient of the loss function w.r.t. the parameters at step $t$
- $\eta$ is the learning rate
### Impact on training
- **Low Learning Rate:** The algorithm takes very small steps, this ensures a stable convergence, but it is computationally slow and it increases the risk of getting stuck into a local minima rather than the global one.
- **High Learning Rate:** The algorithm takes large steps, because of this there is the risk of overshoot the minimum and lead to erratic oscillation or mathematical divergence. In other words, the model won't learn.
To recap, there's no a standard value. It is an empirical value that must be find through hyperparameter tuning or managed dynamically via scheduling algorithms.

## 2. Batches
A batch is a subset of the total training dataset used to compute the gradient and execute a single parameter updates.
This happens because calculating the gradient across the total datasets require too much power, so nowadays it is preferred to compute the gradient after the model has elaborated a batch. 
A more proper name for the batch is **batch size**.

There are different types of optimizations:
- **Batch Gradient Descent:** Computes the gradient using the entire dataset. Deprecated because of the efficiency, but it produces the best convergence.
- **Stochastic Gradient Descent:** Computes the gradient using 1 sample (1 batch). It is based on the mathematical basis that $g = E[g']$, in other words the gradient of the whole dataset can be estimated using only a small size of the dataset. 
- **Mini-batch Gradient Descent:** Computes the gradient using a certain number of sample, typically powers of 2: 32, 62, 256. It is the industry standard solution because it is a good compromise between the other 2.
## 3. Epochs
An epoch is defined when, during the training, the model process the entire dataset.
Because one process of the training set is not sufficient to adjust the weights, models are trained iteratively over multiple epoch.

Relationships between epochs, batches and iteration:
- **Iteration:** One update of the weights, it process one batch
- **Calculation:** $\text{Iteration per Epoch} = \frac{\text{Total Dataset Size}}{\text{Dataset Size}}$

The number of epochs dictates how long a model is exposed to the training data. Too few epochs result in an underfit model that has not captured the underlying data distributions. Conversely, training for too many epochs forces the model to memorize the training set (including its noise), resulting in overfitting and poor generalization to unseen data.

---
# The Concept of Momentum in the context of optimization methods for learning models
