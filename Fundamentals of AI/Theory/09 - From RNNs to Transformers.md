# Recurrent Neural Networks
With feed forward neural networks and CNNs we tackle single-input problems (predict the label for one record or one image). Many forms of data are sequential (videos, text) and so we’d want to expand the scope of our NNs to these kinds of data too.

We cannot expect our model to handle multiple inputs together in a single processing unit:
1. This would become closer to a feedforward network, rather than a sequential one
2. It would be an architecture highly independent from the input size
	Example of the case capitalization. 


![[09 - From RNNs to Transformers-1788632171385.webp|103]]
**Main Idea:** To have an **internal stat**e that is updated as the input sequence is processed
$$
h_t = f_W (h_{t-1}, x_t)
$$where:
- $h_t$ is the new state
-  $h_{t-1}$ is the old state
- $f_W$ some function with parameters $W$ 
- $x_t$ the input vector at some point
**N.B.:** $f$ and $W$ are the same at every step

once the current state is computed, we extract the output from it, as:
$$
y_t = W_{hy} \cdot h_t
$$

**The output of a processing unit for sequential data depends on the current input and the previous ones.**
## Vanilla RNN
As function we use $\tanh$ and the **state** consists of a single hidden vector $h$:
$$
h_t = \tanh(W_{hh} h_{t-1} + W_{xh} x_t) 
$$
## Computational graph
As already said, it re-use the same weight matrix at every time-step
![[09 - From RNNs to Transformers-1788632319692.webp]]
### Many to Many
![[09 - From RNNs to Transformers-1788632406909.webp]]
### Many to One
![[09 - From RNNs to Transformers-1788632442921.webp]]
### One to Many
![[09 - From RNNs to Transformers-1788632463172.webp]]
## Sequence to Sequence
An interesting idea that will later be developed is that of seeing a “many-to-many” architecture as the concatenation of a “many-to-one” + “one-to-many” architecture:
![[09 - From RNNs to Transformers-1788633143224.webp]]

## Backpropagation through time
Forward through entire sequence to compute loss, then backward through entire sequence to compute gradient:
![[09 - From RNNs to Transformers-1788633307014.webp]]
### Truncated Backpropagation through time
- Run forward and backward through chunks of the sequence instead of whole sequence
- Carry hidden states forward in time forever, but only backpropagate for some smaller number of steps
![[09 - From RNNs to Transformers-1788633369928.webp]]
### Vanilla RNN Gradient Flow
Let's how backpropagation works, in particular since $y_t = W_{hy} \cdot h_t$ we are interested to see how $h_t$ propagates back to $h_{t-1}$:
![[09 - From RNNs to Transformers-1788633945658.webp]]
Computing gradient of h0 involves many factors of $W$:
- **Largest singular value > 1:** Exploding gradients
	- To address this we scale the gradient using **gradient clipping**
- **Largest singular value < 1:** Vanishing gradients
	- We have to change the RNN architecture

## Long Short Term Memory











---
# Attention
## Context Concept
Consider two sentences:
1. I swam across the river to get to the other **bank**
2. I walked across the road to get cash from the **bank**
The word bank assume two different meanings, in the first sentence it refers to the side of the river, in the second one it refers to the institution.

Form this example we can deduce that, the meaning of a word does not depends only by itself, but from the context of the sentence.

Obviously, some words in the sentence gives us the context, in the previous example they are:
1. swam and river
2. cash
Attention in this case is how much each part of the sentence contributes to the meaning of bank. More they contributes higher attention they have.

The concept of context is already present in CNNs: we weight **close** pixels and sum them.
There are two main distinctions with CNN:
- **Attention changes with the input:** in fact we will have different weights with two different sentences. Unlike the CNNs, were the weights are constant
- **Attention refers to the whole sequence, not only a part.** In CNNs a filter includes nearby pixels.

## Attention in RNN
An example of implementation of attention mechanism in RNNs is the one seen in **sequence-to-sequence.**

### Sequence to sequence with no attention
- **Input:** Sequence of words $x_1, x_2,\cdots x_{n}$
- **Output:** Sequence of words $y_1,y_2,\cdots y_m$

The encoder produces a series of states $h_1,h_2,\cdots h_n$ (starting from state $h_0$) in the following way
$$  
h_{i}=f_W(x_i,h_{i-1})  
$$
**The final hidden state $h_n$ is used as a context vector:** at each step the decoder will not only look at the current input $y_t$ and state $s_{t-1}$ but at the context too. Thanks to the context the decoder can understand what word to predict next at timestep $t$.

Here's we can see that the **context in a summary of the input sentence**, at each time $t$ the decoder uses the previous word and the summary of the encoder.

![[09 - From RNNs to Transformers-1788635910957.webp]]

A problem that can rise is when the sentence is extremely long, like in a case of a document.
The summary won't provide enough information to the decoder, but we can notice that some words like "estamos" depends only by two words "we" and "are" not by the whole sentence.

In other words, the amount of context that we need to translate each word changes throughout the decoder and, most importantly, we just need some parts of the sequence, not the whole.

### Sequence to sequence with attention
Where:
- $x_i$ is the i-th input
- $h_i$ is the hidden state at step $i$
- $e_{t, i}$ is the similarity score of the $h_i$ hidden state
- $s_0$ is the initial decoder state ($=h_n$)

![[09 - From RNNs to Transformers-1788636361186.webp|587]]

The final hidden state of the encoder $h_n$ becomes the first state of the decoder $s_0$ (i.e. bread).
We take the first state of the decoder and compute a **similarity score ($e$)** to each of the hidden states of the decoder. 
Each similarity score $(e_{11},e_{12},\cdots e_{1n}​)$ is computer by an MLP and it's a scalar:
$$
e_{t, i} =f(h_i, s_t)
$$
The similarity scores are passed through the softmax function in order to have range from 0 to 1 and make their sum equals to 1.
$$
\sum_i a_{t,i}=1
$$
$$
a_{t,i} \in (0,1)
$$
The context vector is given by

$$  
c_0 =\sum_i h_i\cdot a_{t,i}  
$$

Now, the decoder takes $s_0$ (the initial state), $y_0$ (which in this example is the placeholder word "start"), $c_0$ and computes the state $s_1$.
In general:
$$
s_t=g_U(y_{t-1},s_{t-1},c_{t-1})
$$
where $U$ is the weight matrix of the decoder.
The decoder then computes $y_1$ from the new state $s_1$. In this example $y_1=$ “estamos”.
Since “estamos” = “we are” a possible distribution of the attention weights is the following:
- $a_{11}=0.45$
- $a_{12}=0.45$
- $a_{13}=a_{14}=0.05$

Next, in order to predict the second word, we repeat the same process seen with $s_0$, but with $s_1$.

**We use a different context vector in each time-step of decoder:**
- Input sequence not bottlenecked through single vector
- At each time-step of decoder, context vector looks at different parts of the input

![[09 - From RNNs to Transformers-1788637155984.webp]]
We repeat the process with $s_2,s_3\cdots s_{m}$, until the placeholder word “stop” is produced by the decoder.
What emerges from the previous explanation is that the decoder only needs the hidden state sequence produced by the encoder. Moreover the order of the hidden states is not important.

We could literally compute $h_1,h_2,\cdots$ and forget about $x_1,x_2,\cdots$.

Another point worth mentioning is that no labels are needed to learn the MLP that computes attention: the weights of the MLP are learned by backpropagating. Attention weights instead are computed on-line and are different based on the input sequence.


## General Formulation for Attention
**Tokens:** set of input vectors $x_1, ..., x_n \in R^D$
**Features:** the characteristics of each token.

We can put these tokens together as a matrix where:
- each row is a token
- different columns refer to different features
![[09 - From RNNs to Transformers-1788674574398.webp|146]]

### Attention Coefficients
Now, we want to **transform** our tokens to another set of vectors, that we'll call output tokens $y_1, ..., y_N \in R^D$.
The value of one output token $y_i$ should depend not just on the corresponding input token $x_i$ , but on all the vectors $x_1, ..., x_n$.

A simple idea is to have $y_n$ as the linear combination of the inputs:
$$
y_n = \sum_{m=1}^N a_{nm}x_m \in R^D
$$
where:
- $a_{nm}$ is called **attention weight**
	- Small: the input tokens have little influence on $y_n$
	- Big: the input tokens have huge influence on $y_n$

We can also define a **partition unity**:
- $a_{nm} \ge 0$
- $\sum_{m=1}^N a_{nm} = 1$
In this case if we pay more attention to a input, it will be at the expenses of the other ones.

#### How to Compute the Weights
The user computes a **query** and the system search for the **most similar** key and retrieve the corresponding value.

![[09 - From RNNs to Transformers-1788675374420.webp]]


### Similarity Function
Following what we have seen, we can compute the attention weights based on some similarity functions between one input token and the other ones, there are two ways:
- **hard similarity:** the similarity score can assume 1 or 0, but it is not flexible because this means that we will use only one token
- **soft similarity:** the score in a continuous value between $[0, 1]$

We can use a softmax function:
$$
a_{nm} = \frac{\exp(x^T_n x_m)}{\sum_{i=1}^N \exp(x_n^T x_i)}
$$
In compact notation:
$$
Y = \text{Softmax}[XX^T]X
$$
![[09 - From RNNs to Transformers-1788675892039.webp]]
This process is called **self-attention** because we are using the same sequence $X$ to determine the queries, keys and values.

## Self-Attention
The formula
$$
Y = \text{Softmax}[XX^T]X
$$
is not very useful because the weights are fixed for each input sequence, so there are **learnable parameters**.

To correct this we can map the input tokens using a **learnable linear transformation**:
$$\tilde{X} = X{\color{magenta}W} \quad W \in \mathbb{R}^{D \times D}$$
$$\downarrow$$
$$\boxed{Y = \operatorname{Softmax}[\tilde{X}\tilde{X}^T]\tilde{X} = \operatorname{Softmax}[XW W^T X^T]XW}$$
Now we have the learnable parameters.
The only problem is that the matrix $XW W^T X^T$ is not symmetric, and we need asymmetry because some words have to be always strongly associated with other but not viceversa:
- "Chisel" has to be strongly associated with tools
- But "Tool" only sometimes because there exists a lot of other tools

To fix this we can use a **separate** learnable linear transformation for query, key and value:
$$
Q = X W^{(q)}, \qquad K=X W^{(k)}, \qquad V = X W^{(v)}
$$
The softmax becomes:
$$
Y = \text{Softmax}[QK^T]V
$$
### Recap
Essentially, we are performing a linear combination of the values $V$ using a learned matrix of weights. **Differently from convolutions, these weights are not fixed, but they depend on the input signal $X$.**


### Self-Attention Layer
**Problem:** The gradients of the softmax function become exponentially small for inputs of high magnitude.

We can prevent this by re-scaling the product of queries and keys before the softmax:
$$
Y = \text{Attention}(Q, K, V) = \text{Softmax}  [\frac{Q K^T}{\sqrt{D_k}}  ] V
$$
This is generally calle **self-attention layer**.
![[09 - From RNNs to Transformers-1788677089308.webp|150]]


### General Attention Layer
In the **self-attention layer,** queries, keys and values are generated from the same input sequence.
![[09 - From RNNs to Transformers-1788677409977.webp|160]]

If the query are generated from another input signal, this is called **cross-attention**.
![[09 - From RNNs to Transformers-1788677437343.webp|163]]

## Multi-head Attention







