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