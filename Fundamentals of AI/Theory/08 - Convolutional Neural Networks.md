Convolutional Neural Networks are a type of deep learning models designed to process data with a grid-like topology, most commonly used for visual data like images or videos.
Unlike standard Neural Networks, that flatten images into a single list of pixels, CNNs preserve the structural relationships between neighboring pixels. Another characteristic of the CNNs is that they don't need someone that manually defines the features to look for: the network automatically do this, this process is called Hierarchical Feature Extraction.

# Core Architecture
A standard CNN is composed by a sequence of specialized layers:

- Convolutional Layers
- Activation Layers
- Polling Layers
- Fully Connected Layers

## Convolutional Layer
What Convolution is
We start by defining the convolution, a fundamental mathematical operation used by this network.

Given two functions $f, g: [-\pi , \pi] \rightarrow \mathbb{R}$ their convolution $(\star)$ is a function s.t.:
$$(f \star g)(x) = \int_{- \pi}^\pi f(t) g(x - t) \, dt$$
where:
- $g(x-t)$ is the kernel, that is the filter (a matrix) applied at the input
- $(f \star g)(x)$ is called feature map that is the result of the kernel applied to the image

Properties
Convolution is:

- **Commutative:**
  $$(f \star g)(x) = \int_{-\pi}^{\pi} f(t) g(x - t) \, dt \stackrel{\substack{z := x - t \\ \downarrow}}{=} \int_{-\pi}^{\pi} f(x - z) g(z) \, dz = (g \star f)(x)$$
- **Shift-equivariant:**
  $$f(x - x_0) \star g(x) = (f \star g)(x - x_0)$$

### Discrete Convolution
Computers don't work with continuous quantities, so we have to introduce a convolution for discrete inputs called discrete convolution:
$$(f \star g)[n] = \sum_{k= -\infty}^\infty f[k]g[n-k]$$

![[08 - Convolutional Neural Networks-1788019976553.webp|507]]

We cannot use this simple formula as it stands, because in practice we do not work with one dimensional arrays, but rather we have multidimensional array (called matrix). This because images are compose by three matrix that represents the channels. Obviously, they have not infinite bounds.
These multidimensional arrays are often referred as **tensors**.

#### Discrete Convolution with 2D elements
On 2D domains, e.g. RGB images $f : \mathbb{R}^2 \rightarrow \mathbb{R}^2$, for each channel we get:
$$(f \star g)[m, n] = \sum_k \sum_l f[k, l] g[m - k, n - l]$$

That is a moving window:
![[08 - Convolutional Neural Networks-1788089156792.webp]]

### Convolutional Neural Network
A convolutional layer transform inputs $x_{\text{in}}$ to outputs $x_{\text{out}}$ by convolving $x_{\text{in}}$ with one or more **filters** $w$ composed of **learnable parameters**. 
Typically we consider also a **bias** $b$:
- **1D convolution:**
$$x_{\text{out}}[n] = b + \sum_{k=-K}^{K} w[k] x_{\text{in}}[n - k]$$
- **2D convolution:**
$$x_{\text{out}}[n, m] = b + \sum_{k_1, k_2=-K}^{K} w[k_1, k_2] x_{\text{in}}[n - k_1, m - k_2]$$
- ...

#### Filters
A Convolutional Layer consists of a set of:
- **Filter:** small matrix of weights that is convolved with an input volume to extract local features or patterns
- **Convolution:** the dot product between the parameters of the filter and the input

An example of filter (also called kernel):
![[08 - Convolutional Neural Networks-1788091772611.webp]]

> The network will learn filters that activate when they see some specific type of feature at some spatial position in the input.
> The key architectural characteristics of the convolutional layer are local connectivity and shared weights

![[08 - Convolutional Neural Networks-1788092254579.gif.gif|304]]
#### Multichannel Input
If our input has multiple channels, we use a **multichannel filter** $w \in \mathbb{R}^{C \times K \times K}$:
$$
x_{out} = \sum_C w[c , :, :] \star x_{in} [c , :, :] + b[c]
$$
where $c$ is the channel and $C$ is the total number of channels.

The multichannel filter:
- Spatially covers a small portion of the input volume in terms of width and height
- Extends through the full depth of the input volume
![[08 - Convolutional Neural Networks-1788093603503.webp]]
Each of the kernels is responsible for a **single input channel** and is convolved with it.

#### Multiple Outputs
If our output has multiple channels, use a set of filters, called **filter bank**:
$$
x_{out} [0, :, :] = w[0, :, :] \star x_{in} + b[0]
$$
$$\vdots$$
$$
x_{out} [C, :, :] = w[C-1, :, :] \star x_{in} + b[C-1]
$$
We call each channel of the output a **feature map**.
![[08 - Convolutional Neural Networks-1788093969853.webp]]
### General Convolution
Combine the previous cases to define general convolution:
![[08 - Convolutional Neural Networks-1788094068023.webp]]

### Stride
As already set before, convolutional layers maintain the spatial resolution of what they process, but sometimes we may want to lower the output resolution.

In order to do that we can modify the **stride** (that it is $1$ on default):
$$
x_{\text{out}}[n, m] = b + \sum_{k_1, k_2=-K}^{K} w[k_1, k_2] x_{\text{in}}[\textcolor{lightblue}{s_n}n - k_1, \textcolor{lightblue}{s_m}m - k_2]
$$
### Padding
One problem when applying convolutional layers is that we tend to lose information on the perimeter of our input.
So, we set with $0$s the input borders before applying the filter.

### Recap
![[08 - Convolutional Neural Networks-1788094947925.webp]]
### Receptive fields
In a neural network, the receptive field of a neuron refers to the portion of the input seen by that neuron.
As we already said, unlike MLP, in CNN a neuron sees only a portion of the previous layer because the layers are not fully connected.

![[08 - Convolutional Neural Networks-1788095359204.webp]]

## Pooling Layer
> Pooling layers are **down-sampling** layers that summarize the information in a patch using some aggregate statistic
 - **down-sampling:** reducing the resolution (i.e. height and width)
> - **patch:** small region of pixel
> - **aggregate statistic:** mathematical **non-linear** operation (e.g. Max, Avg, etc.)

These layers work like convolutional layers but:
- they apply a **non-linear function** to a window
- they do not have a kernel with learnable weights.![[08 - Convolutional Neural Networks-1788104608085.webp|364]]
The non-linear transformations can be:
- Avg Pooling
- Max Pooling
- $L^2$ normalization
- and so on...

### Properties
Pooling is very useful to achive **invariance**.
If the pooling areas are contiguous areas and they are generated using the same filter, then they are **translation invariant**.
![[08 - Convolutional Neural Networks-1788105367661.webp|339]]Pooling can also be performed across channels, which can allow to achieve other types of invariance e.g. rotation.

### Global Pooling
In some cases it is useful to apply the pooling to the entire features map by setting the dimensione of the window of the same size.
This is useful when we are close to the output layer because it can summarize the responses to a single embedding vector.

---
## Activation Function
Activation functions introduce pointwise nonlinearity, they are applied after each convolutional layer. Without them, the entire network would be linear.
### Sigmoid
![[08 - Convolutional Neural Networks-1788275757989.webp]]
$$
\sigma(x) = \frac{1}{1+e^{-x}}
$$
- It squashes numbers in a range of $[0, 1]$
- Historically popular since they have nice interpretation as a saturating firing rate of a neuron

#### Problems
1. **Vanishing Gradient:** for very large (positive) and very small (negative) $x$, her derivative is close to 0
2. **Sigmoid outputs are not zero-centered:** if the outputs are always positive it will cause a zig-zag update
3. **Computationally expensive:** because of the $e$


### Tanh
![[08 - Convolutional Neural Networks-1788278755772.webp]]

- It squashes numbers in a range of $[-1, 1]$
- It is zero centered
#### Problems
- **Vanishing Gradient**

### Rectified Linear Unit - ReLU
![[08 - Convolutional Neural Networks-1788278874023.webp]]

$$
f(x) = \max(0, x)
$$
#### Pros
- It does not saturate in positive region
- Very computationally efficient
- Converges 6 times faster than sigmoid and tanh
- More biologically plausible than sigmoid
#### Cons
- The output is not zero centered
- In the negative region gradient vanishing can happen

### Leaky ReLU
![[08 - Convolutional Neural Networks-1788279193110.webp]]
$$
f(x) = \max(\alpha x, x)
$$
It has the same pros of the classic ReLU but the parameter $\alpha$ is used for creating a small inclination (gradient) for negative inputs. This does cancel the risk of Vanishing Gradient.
The $\alpha$ parameter can be learned via backpropagation.

### Exponential Linear Units - ELU
![[08 - Convolutional Neural Networks-1788279511484.webp]]
$$
f(x) = \begin{cases} x & \text{if } x > 0 \\ \alpha(e^{x} - 1) & \text{if } x \leq 0 \end{cases}
$$
#### Pros
Same of the ReLU but:
- It is closer to zero mean outputs
- Negative saturation regime compared with Leaky ReLU adds some robustness to noise
#### Cons
- Computational expensive because of the $\exp$
### Gaussian Error Linear Units - GELU
![[08 - Convolutional Neural Networks-1788279712776.webp]]
$$
f(x) = x \cdot \Phi(x)
$$
where $\Phi$ is the Gaussian Cumulative Distribution Function. 
It is a smooth approximation of the ReLU and it is still used in some Transformers like BERT.
### In practice
- Just use the **ReLU**
- Try **Leaky ReLU/ELU** if you want the last 0,1% of improvement 
- **Do not use Sigmoid or tanh**

---
## Normalization Layers
These layers add another type of non-linearity: **they act on a set of neurons based on their collective behaviors.**
They are useful because the network does not have to work with very small or large values.
There are a lot of types of normalization.

### Data Normalization
Used when the input data have variables with very different ranges. 
For example:
- height of a person has a range of $[1.5, 2.2]$
- blood platelet has a range of $[200000, 300000]$
This variation can make more challenging the training, because a change in the larger value can produce a much higher changes in the weights w.r.t. the smaller one.

Normalization means re-scale the input values:
- Statistics from training data $$\mu_i = \frac{1}{N} \sum_{n=1}^{N} x_{ni}$$$$\sigma_i^2 = \frac{1}{N} \sum_{n=1}^{N} (x_{ni} - \mu_i)^2$$
- Re-scaling $$\widetilde{x}_{ni} = \frac{x_{ni} - \mu_i}{\sigma_i}$$
 ![[08 - Convolutional Neural Networks-1788353495814.webp]]
### Batch Normalization
Data Normalization act in the input data, but:
- The weights of the layers are changed simultaneously during the backpropagation
- So each have in input a changing distribution every iteration because the layer beside him have changed his weights
This phenomenon is called **Internal Covariate Shift (ICS)**.

Batch Normalization use the same principles of the Data Normalization to solve this problem, **it wants to standardize each neural activation (the activation function) w.r.t. its mean and variance over a batch of datapoints**.
![[08 - Convolutional Neural Networks-1788354251803.webp|298]]
We compute the empirical mean and variance independently for each channel:
- Per-channel mean$$\mu_j = \frac{1}{N} \sum_{i=1}^{N} x_{i,j}$$
- Per-channel std$$\sigma_j^2 = \frac{1}{N} \sum_{i=1}^{N} (x_{i,j} - \mu_j)^2$$
- Normalized $x$$$\hat{x}_{i,j} = \frac{x_{i,j} - \mu_j}{\sqrt{\sigma_j^2 + \varepsilon}}$$
The $\epsilon$ is a small constant used to avoid $0$ at the denominator.

#### Loss of Representational Capability
The Batch Normalization standardizes the pre-activation in a layer of the network, by doing this we reduce degrees of freedom in the parameters of the layer.
In order to solve this problem, the algorithm re-scale the pre-activations of the batch to have mean $\beta^{(k)}$ and std $\mu^{(k)}$:
$$
\hat{x}^{(k)} = \frac{x^{(k)} - \mathbb{E}[x^{(k)}]}{\sqrt{\text{var}[x^{(k)}] + \epsilon}} \implies y^{(k)} = \gamma^{(k)} \hat{x}^{(k)} + \beta^{(k)}
$$
where $\beta^{(k)}$ and $\mu^{(k)}$ are **learnable parameters**.

#### The Layer
![[08 - Convolutional Neural Networks-1788361301151.webp|125]]
The layer is inserted:
- **after** Fully Connected or Convolutional Layers
- **before** non-linearity
![[08 - Convolutional Neural Networks-1788361394732.webp|596]]
#### Pros
- **Higher Learning Rates:** By preventing small parameter changes from amplifying into large activation changes, BN makes the network much more tolerant of high learning rates that would otherwise cause the model to explode.
- **Reduced Sensitivity to Initialization:** Deep networks become less fragile to the initial scale of weights, making careful initialization less critical.
- **Regularization Effect:** Because the mean and variance are estimates based on minibatches, they introduce a small amount of "noise" to the activations. This acts as a form of **stochastic regularization**, often allowing practitioners to reduce or eliminate the use of Dropout.
- **Faster Convergence:** Models trained with BN typically reach state-of-the-art accuracy in significantly fewer training steps—sometimes as much as 14 times faster
#### Cons
- Not well-understood theoretically
- Behaves differently during training and testing

--- 
# How to Train a Deep Neural Network
## Training Neural Networks: Overview
1. **One time setup**
	- activation functions, preprocessing, weight initialization, regularization
2. **Training dynamics**
	- babysitting the learning process, parameter updates, hyperparameter optimization
3. **After training**
	- model ensembles, transfer learning

## Data Preprocessing
![[08 - Convolutional Neural Networks-1788364759923.webp]]
Zero centering is helpful in the case of data with always positive or always negative features.

Consider what happens when a neuron receives always positive data:
$$  
y=f\left(\sum_iw_ix_i\right)\text{ where }x_i>0  
$$
The gradient will have all the components of the same sign
$$  
\frac{\partial f}{\partial w_i}=\frac{\partial f}{\partial a}\cdot \frac{\partial a}{\partial w_i}=\frac{\partial f}{\partial a}\cdot x_i  
$$
Therefore
$$  
\nabla f=\frac{\partial f}{\partial a}\begin{bmatrix}x_1\\x_2\\ \vdots \\x_n\end{bmatrix}  
$$
![[08 - Convolutional Neural Networks-1788365047931.webp|299]]

We can also use:
- **PCA:** The new data has a diagonal Variance-Covariance matrix. This means that the data is uncorrelated.
- **Whitening:**  The new data has a unitary Variance-Covariance matrix. We obtain this by dividing each feature of the PCA transformed data by its standard deviation.![[08 - Convolutional Neural Networks-1788365071398.webp|567]]
#### Advantages of the Preprocess
On the left there is the classification before the normalization, as we can see the loss is very sensitive towards the changes of the weight, making it hard to optimize. After the normalization we can see that is less sensitive:![[08 - Convolutional Neural Networks-1788365444173.webp|584]]
#### Rule of Thumb for images center only
- Subtract the mean image (e.g. AlexNet)
- Subtract per-channel mean (e.g. VGGNet)
- Subtract per-channel mean and Divide by per-channel std (e.g. ResNet)
It is not common to normalize variance, nor is it common to perform PCA or whitening for image data.

## Weight Initialization
Deep models are highly sensitive to the **initial point** of their parameters, which can determine whether the algorithm converges at all and how well it generalizes.
### Problems
#### 1) Symmetry Breaking
The most fundamental requirement is that initial weights must break symmetry between units: if two neurons with the same activation function are connected to the same inputs and have the same initial weights, they will have the same gradient, so they will be affected by the same update throughout the whole training.
In order to solve this we can use a **Random Initialization**:
- **Small random numbers:** a gaussian with $0$ mean and $10^{-2}$ std. It works well for small deep networks, but have problems with deeper one
#### 2) Zero Weights
If all weights are initialized to zero, we fall in the symmetry problem listed before.
#### 3) Small Weights
In deep networks, these small values cause the activation function to **collapse to zero** as they propagate forward, causing **vanishing gradient**.
Why does the signal collapse through the network?

$$  
h^{(i+1)}=f(W^{(i)}h^{(i)})  
$$

Since the values of $W^{(i)}h^{(i)}\approx 0$, the activation function behaves linearly

So we have that

$$  
h^{(i+1)}=W^{(i)}h^{(i)}=W^{(i)}W^{(i-1)}\cdots W^{(0)}x  
$$

If we consider all the $W$ to be approximately the same (since all their values are near 0)

$$  
h^{(i)}=W^ix  
$$

For $i>>0$ $W^i \rightarrow0_{n\times m}$, that’s why in deeper layers the signal vanishes.
If $x \sim \mathcal N(0,1)$ and we feedforward through the neural network we can see, for each layer that
![[08 - Convolutional Neural Networks-1788372466744.webp]]
The standard deviation of the distribution of $h^{(i)}$ shrinks towards 0, therefore the signal in the last layers is almost always 0.

#### 4) Big Weights
Initialization with bigger numbers causes the opposite problem, **activation functions saturate**. Causing gradients equals to 0 and the model doesn’t learn.
![[08 - Convolutional Neural Networks-1788372537847.webp]]
### Standard Initialization Heuristics
They are predefined rules and mathematical formulas used to set the initial values of the weight in order to avid vanishing or exploding gradient. 
#### Xavier Initialization
Designed to keep the variance of activation functions and gradients the same across layers. It is standard for layers using **symmetric activation functions** like tanh.

It works by setting the std for each matrix weight to
$$
\sqrt{\frac{1}{n^{(i)}}}
$$
where $n^{(i)}$ is the size of the i-th layer’s input.
**Goal:** set the standard deviation of the input equal to the one of the pre-activation value.

#### Kaiming Initialization (or MSRA)
A problem of the Xavier one is that it assumes to have zero-centered activation function (like tanh or sigmoid), because of this it fails with ReLU.
Kaiming initialization corrects this by using a std of $\frac{2}{n}$, keeping activations nicely scaled for ReLU networks.

![[08 - Convolutional Neural Networks-1788446593216.webp]]
ReLU puts 50% of the distribution to 0, while the rest is the same. This, of course, shifts the mean a little to the right and reduces the variance (approximately by a factor of 2).

#### Residual Neural Network
![[08 - Convolutional Neural Networks-1788448165457.webp]]
In this case we have that:
$$
y=F(x)+x
$$
so, even if we applied MSRA or Xavier ($\text{Var}(F(x))=\text{Var}(x)$) we would have that
$$  
\text{Var}(y)=\text{Var}(F(x)+x)>2\text{Var}(x)  
$$
**The variance increases from a layer to another** causing exploding gradients or saturation of later activations.

The solution is to initialize the first convolutional layer with MSRA, then initialize the second one to zero, so that we obtain:
$$
Var(x + F(x)) = Var(x)
$$
---
# Regularization
Regularization is used in order to improve the performance of the model by reducing the overfitting.

Mathematically consists in an additional terms added at the loss function:
$$
L = \text{Data Loss} + \textcolor{red}{\lambda R(W)}
$$
where:
- $\lambda$ is an hyperparameter that tune the intensity of the regularization
- $R(W)$ is the penalty function that can be of different types:
	- **L2 Regularization**: $R(W) = \sum_k \sum_l W^2_{k, l}$
	- **L1 Regularization**: $R(W) = \sum_k \sum_l | W_{k, l}|$
	- **Elastic net (L1 + L2):** $R(W) = \sum_k \sum_l \beta W^2_{k, l} + | W_{k, l}|$

## Dropout
In each forward pass, randomly set some neurons to zero. The probability for a neuron of being dropped is a tuning hyperparameter, in default it is $0.5$.
**Advantages:**
1. prevents the co-adaptation of the features 
	- co-adaptation means that neurons tends to adapt or compensate the output of another neuron
2. Forces the network to have a redundant representation
	- we force the fully connected layers to interpret the output with some information missing so that when all the neurons will be "on" they will be more robust
3. It is like training an ensemble of models that share parameters
![[08 - Convolutional Neural Networks-1788450031967.webp|498]]
Here is a clean, well-structured, and publication-ready version of your notes in academic English.

### **How to Perform Testing with Dropout**

We cannot simply turn off random neurons during testing. As noted previously, doing so would be equivalent to selecting a single random tree from a random forest to make the final decision, which directly contradicts the fundamental principle of ensemble methods.

Ideally, the target output for an input $x$ is the expected value over all possible dropout configurations $Z$:

$$y = \mathbb{E}_Z[f(x, Z)]$$

Where:
- $Z$ is a random variable representing the specific mask of active/inactive parameters.
- $f(x, z)$ is the model's output for a specific network configuration $z$.

Expanding the expectation yields:

$$y = \sum_{z} p(z) \cdot f(x, z)$$

This evaluates the output of every possible sub-network, weighted by the probability $p(z)$ of picking that specific configuration. However, because the number of possible sub-networks ($2^N$) grows exponentially with the number of neurons $N$, computing this exact sum is computationally intractable. Instead, we apply a fast heuristic to approximate the ensemble output using a single forward pass.

### **1. Standard Dropout (Test-Time Scaling / Weight-Scaling Rule)**

Consider a single perceptron with two inputs, $x$ and $y$, and suppose the survival probability is $p = 0.5$.

- **During Training:**
    The expected activation $\mathbb{E}[a]$ across all $2^2 = 4$ possible dropout masks is:$$\mathbb{E}[a] = \frac{1}{4}(w_1 x + w_2 y) + \frac{1}{4}(w_1 x + 0) + \frac{1}{4}(0 + w_2 y) + \frac{1}{4}(0 + 0) = \frac{1}{2}(w_1 x + w_2 y)$$
- **During Testing (without Dropout):**
    All neurons remain active, so the output is:$$a = w_1 x + w_2 y$$
Because no dropout is applied at test time, the raw output scale differs from training by a factor of $p$. To compensate for this discrepancy and match the expected training scale, we multiply the output (or equivalently, the weight matrix) by $p = \frac{1}{2}$ during testing:

$$a_{\text{test}} = p \cdot (w_1 x + w_2 y)$$
### **2. Inverted Dropout**

An alternative strategy—widely adopted as the modern standard—is **Inverted Dropout**. Instead of adjusting activations at test time, we scale the activations upward **during training**.

By dividing the training activations by $p$ (i.e., multiplying by $\frac{1}{p} = 2$ for $p = 0.5$), the expected training activation becomes:
$$\mathbb{E}[a] = \frac{1}{4}\Big[2(w_1 x + w_2 y)\Big] + \frac{1}{4}\Big[2(w_1 x + 0)\Big] + \frac{1}{4}\Big[2(0 + w_2 y)\Big] + \frac{1}{4}(0) = w_1 x + w_2 y$$
By absorbing the scaling factor during training, the test phase requires no post-processing adjustments ($a_{\text{test}} = a$), leaving the test pipeline clean and computationally efficient.

### **Dropout in Famous Architectures**
Classic architectures such as **AlexNet** and **VGG-Net** concentrate the vast majority of their trainable parameters in their Fully Connected (FC) layers, which is where dropout regularization is applied to prevent overfitting.

In contrast, modern architectures rarely rely on dropout; they replace dense fully connected layers with **Global Average Pooling (GAP)** layers, drastically reducing the overall parameter count and making explicit dropout unnecessary.

## Model Ensembles
Key intuition:
1. Train multiple independent models
2. At test time average their results

### Tips and Tricks
1. Instead of training independent models, use multiple snapshots of a single model during training
2. **Polyak averaging:** Instead of using actual parameter vector, keep a moving average of the parameter vector and use that at test time

## Transfer Learning
### Pre-training Dataset: ImageNet
- **ImageNet:** Large-scale visual database structured according to the WordNet hierarchy.
- **Scale:** Over 14 million images (`14,197,122` images) spanning over 21,000 synsets (`21,841`).

### 1. Feature Extraction (Linear Classifier / Fixed Backbone)
The process consists into:
1. Take a network pre-trained on ImageNet (e.g., AlexNet/VGG)
2. **Freeze** all convolutional and early fully-connected layers, meaning that all the weights remain unchanged
3. Remove the final classification layer
4. **Reinitialize** and train only the last layer (or linear classifier) on the target dataset ($C$ classes).
![[08 - Convolutional Neural Networks-1788535115634.webp]]
### 2. Fine-Tuning
The process consists into:
1. **Replace** the final output layer to match target classes ($C$).
2. Instead of freezing all early layers, **train additional layers** (or the entire network) using the target dataset.
It is recommended to use a **lower learning rate** when compared, typically $\frac{1}{10}$ of the one that was used in the network training.  
![[08 - Convolutional Neural Networks-1788535795524.webp]]

#### Fine-Tuning Strategy Decision Matrix
Layer representations transition from **generic** (early convolutional layers) to **specific** (late fully-connected layers).

|                                                           | Very Similar Dataset                   | Very Different Dataset                                                                  |
| --------------------------------------------------------- | -------------------------------------- | --------------------------------------------------------------------------------------- |
| **Very Little Data**<br><br>_(10–100 samples/class)_      | **Use Linear Classifier** on top layer | **Troublesome Case:** Try linear classifiers trained from different intermediate stages |
| **Quite a Lot of Data**<br><br>_(100–1000 samples/class)_ | Finetune a few layers                  | Finetune a larger number of layers                                                      |
![[08 - Convolutional Neural Networks-1788535998020.webp]]

### Summary / Practical View
1. **Speed & Efficiency:** Pre-training + Fine-tuning accelerates convergence/training time, making it highly practical.
2. **Data Availability:** Training from scratch functions well given sufficient target data.

---
# Notable CNN Architectures
## ImageNet
The paper **ImageNet Classification with Deep Convolutional Neural Networks** published in 2012 and describing the AlexNet architecture represents a milestone in CNN architecture and vision tasks in general.

It is composed by $8$ layers:
- $5$ are convolutional layers
- $3$ are fully connected layers

Here's a comparison with the, at the time, state-of-art network LeNet5: 
![[08 - Convolutional Neural Networks-1788538760724.webp|700]]

The most important elements of the network are:

### ReLU Non-Linearity
![[08 - Convolutional Neural Networks-1788538887352.webp]]
If $x$ is the output of a perceptron, the standard for non-linearities in that period where $\tanh(x)$ and $(1+e^{-x})^{-1}$. These are referred to as “saturating” non linearities, as their output is limited to 1, no matter the input given.

The authors of the paper stated that the training with descent gradient was much slower using this saturating function (e.g. tanh) w.r.t. non-saturating one $f(x) = \max(0, x)$. For this reason they used the Rectified Linear Units.

### Data Augmentation
The authors of the paper have used two distinct technique of data augmentation, both of them are really **computationally cheap**, because of that there was no need to store them on disk, they are generated at run-time by the CPU while the GPU is training the previous batch.

The two technique are:
- Cropping + Horizontal flipping
- RGB jittering

#### Cropping + Horizontal flipping
The image of the dataset are $256 \times 256$, the first layer of the AlexNet receives resolution of $224 \times 224$, this resolution is achieved by cropping the images. 
In particular, from the original images **we extract 5 different patches**: the 4 corners + the center.
![[08 - Convolutional Neural Networks-1788588313627.webp|650]]Each patch is then duplicated through the horizontal flip, so from $1$ image we obtain $10$ samples.

This augmentation is not made only at training time, but also during the test: the prediction in made by averaging all the predictions made by the softmax layer over the $10$ images.

#### RGB Jittering
The idea is to slightly shift the colors of each image in a way that mimics natural variation in lighting and color. This is done by:
1. Apply a PCA over the entire dataset in order to find the **principal directions of color variation**
2. Then, **add a small perturbation** along each image of the dataset.
Authors says that this technique is very useful because object identity becomes invariant towards changes in the intensity and color illumination.
#### Dropout
Already discussed before.

## VGGNet
This network was the winner of ILSRVC of 2014, developed by Visual Geometry Group, and was deeper than the AlexNet.
![[08 - Convolutional Neural Networks-1788590980080.webp]]

VGG has some rules:
- Every convolutional layer is $3 \times 3$ with stride 1, in many layers the input size matches the output size of the previous layer, so even the padding is $1$
- All Max Pooling layers are $2 \times 2$ with stride $2$
- After a Pooling layer, the number of channel in the convolutional layer is doubled

At the end of the convolutional block there’s a ReLU activation function.

### Core Idea
The idea of VGG is to use only 3x3 convolutional blocks, and this is why the architecture is said to be uniform.
The main reason behind this is that if we stack multiple 3x3 blocks we get the same receptive field of a larger filter with less parameters and flops.

In general if we have:
- $L$, the number of consecutive convolutional layers
- $K \times K$ the kernel dimension with stride 1
The total receptive field size is:
$$
1 + L(K \times K)
$$
![[08 - Convolutional Neural Networks-1788592277175.webp]]
Substituting a larger convolution with a stack of smaller ones has two advantages:
- The model can create more complex decision boundaries increasing representative power, because the number of non linearities increases.
- The number of parameters and floating point operations decreases

### Overall Structure
Both versions of VGG have 5 stages. At the end of each stage there is a $2 \times 2$ stride $2$ Max-pooling layer that halves the image resolution. The first convolutional layer of the next stage will then double the number of channels.

Max Polling main goal is to enlarge receptive fields through down-sampling:
- Stage 1: 2 blocks
- Stage 2: 2 blocks
- Stage 3: 2 blocks
- Stage 4: 3 blocks (4 blocks in VGG-19)
- Stage 5: 3 blocks (4 in VGG-19)

The fully connected layers are similar to AlexNet. The difference between VGG and AlexNet is therefore in the feature extraction.

One thing to notice is that the FLOPs remain constant in each convolutional block.

### Representative power
VGG has a very good representative power. It is demonstrated that if we remove the fully connected layer and the softmax the network can be used for a variety of computer vision tasks with good results.

To sum up the steps are:
- Train VGG 16 on ImageNet
- Remove the FC1000
- For the new task you work with the “frozen” VGG architecture
- Eventually add task-specific FC’s (like a FC100 if you have to perform classification on 100 classes, or any other header for any task), train and test on the transformed vectors by the VGG

This was a sign that with deeper networks you could get better features.

### Depth and Feature Quality
Early layers detect simple and general features, like edges. As we go deeper the features start getting more complex and specific, in deeper layers features detect entire objects.
This is allowed by the fact that the **receptive filter size increases as we go deeper**.

For this reason narrow networks cannot capture complex interactions between the basic features, meanwhile deep networks can. Being able to use abstract features is a key factor in telling similar images apart.

Enlarging the filters on the shallow network works but not as efficiently as increasing depth.

DNN prior hierarchy: more complex features are built upon simpler features. Therefore building complex features on basic features works better than learning complex features right away.

Lastly, deeper networks have way more activations than short networks, therefore the model’s function is more complex and representative


## GoogLeNet
This network was introduced in the $2014$. 
It does not use **global average pooling**.
Like we said before at the time, thanks to VGGNet, there was the idea that to enhance the performances we had to make larger and deeper networks:
- **Deeper:** adding more layers in sequence. 
	- AlexNet (2012) had $8$ layers, VGGNet (2014) had 16-19 layers
- **Wider:** increasing the number of filters (that means adding more channels in output) per layer

The problem was that both directions scaled very poorly:
- More layers → vanishing gradients, harder to train
- More filters → quadratic growth in parameters and computation

GoogLeNet tried to find a solution.
### Core Idea
The core idea was based on **finding the right filter size to use at each layer**, from theory we know that:
- **small filters:** capture fine-grained local patterns
- **large filters:** capture broader spatial structures

We use the **Inception module answers: use all of them in parallel**.
A single Inception module takes the same input and passes it through four parallel branches simultaneously:
![[08 - Convolutional Neural Networks-1788614047110.webp]]

All four branches produce outputs with the **same spatial dimensions** (height × width) thanks to appropriate **padding**, so they can be **concatenated along the channel axis** and passed to the next layer. The network then learns which filter responses are most useful for the task, rather than the designer having to choose a single scale.

### Computational Problem
The naive version of this idea is **computationally prohibitive**.
The core issue is that **every spatial filter must operate across the full channel depth of the input**, and large filters like 5×5 are extremely expensive when the input has hundreds of channels.

For example, if we apply a filter $5 \times 5$ on an input of 256 for make 128 channel in output we have:
$$
5 \times 5 \times 256 \times 128 = 819.200 \text{ Parameters}
$$
#### Solution: $1 \times 1$ convolution as bottleneck
Google researchers introduced a **$1 \times 1$ convolutional layer before the bigger ones**.
This layer is useful because it **reduce the dimensionality** (imbuto) by:
1. Takes the high depth input, e.g. 256 channels
2. It compresses the input, e.g. 32 channels
3. The successive layers (e.g. a $5 \times 5$) works on this, so it is less expensive
![[08 - Convolutional Neural Networks-1788617392650.webp|624]]

```Pseudo
Without bottleneck:
  Input (256ch) ──→ 5×5 conv ──→ Output (32ch)
  Cost: 32 × 5×5 × 256 = 204,800 parameters

With bottleneck:
  Input (256ch) ──→ 1×1 conv (16ch) ──→ 5×5 conv ──→ Output (32ch)
  Cost: (16×1×1×256) + (32×5×5×16) = 4,096 + 12,800 = 16,896 parameters
```
This bottleneck layer became a **foundational building block of modern** CNN like ResNet. 

The insight that channel reduction via 1×1 convolutions decouples spatial filtering cost from input depth turned out to be one of the most reusable ideas in deep learning architecture design.

## Residual Network 

### Context
Thanks to batch normalization it became possible to train more and more deep networks. 
The idea was to create deep NN that **emulates the smaller state-of-art ones** and add them **extra layers** in order to enhance them. 
**Example:** imagine we have a classic NN of 20 layers, we want to create a deeper one of 56 layers that is better, we use:
- 20 layers equals to the small model
- The other 36 will be extra layers that add complexity enhancing the model

In theory a deeper network should performs at least the same compared to a smaller one, but not in this case: **the deeper network on the test set performed worse**.

The initial guess was that the deep model was **overfitting** but, after having trained the small models on the same training set, they discovered that the deep model was **underfitting** because even on training data it performed worse.
In particular they trained the deeper network of 56 with:
- 20 layers equals to the small model
- the other 36 as identity function
In theory the performance should be the same.
![[08 - Convolutional Neural Networks-1788618013407.webp]]

The problem is that the deeper NN had problems to emulates the classic network: **they had trouble in approximating the identity functions**

### Solution - Residual Blocks
The idea is to change the network architecture in order to facilitate the emulation of the identity function.

This change is made by adding an **additive shortcut** (or residual connection).

Let's see the difference between the plain block and the residual block:
- **Plain block:** In a classic convolutional layer the block try to learn from an input $x$ the full transformation $H(x)$ $$\text{Output=}H(x)$$
- **Residual block:** In this configuration the input $x$ takes two path:
	1. Goes in the convolutional layer that calculates a **Residual Transformation**
	2. Takes the shortcut and skip the layer
	3. At the end of the two paths the results are summed and then we apply the activation (e.g. ReLU) $$\text{Output} = F(x) + x$$
The residual solution is better because if the network sees that the convolutional block does not optimizes the prediction it can turn it off by setting $F(x) = 0$, so that in output we have $0 + x = x$ that is the identity function.
Also it helps during the backpropagation because the gradient w.r.t. the loss goes directly backwards without calculating the new weights for the layer, this prevents vanishing gradient.
![[08 - Convolutional Neural Networks-1788619649789.webp]]



