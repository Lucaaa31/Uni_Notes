# Recurrent Neural Networks
With feed forward neural networks and CNNs we tackle single-input problems (predict the label for one record or one image). Many forms of data are sequential (videos, text) and so we’d want to expand the scope of our NNs to these kinds of data too.
We cannot expect our model to handle multiple inputs together in a single processing unit:
- This would become closer to a feedforward network, rather than a sequential one
- It would be an architecture highly independent from the input size

Example of the case capitalization. 
103
Main Idea: To have an internal state that is updated as the input sequence is processed
$$h_t = f_W (h_{t-1}, x_t)$$
where:
- $h_t$ is the new state
- $h_{t-1}$ is the old state
- $f_W$ some function with parameters $W$ 
- $x_t$ the input vector at some point

**N.B.:** $f$ and $W$ are the same at every step

once the current state is computed, we extract the output from it, as:
$$y_t = W_{hy} \cdot h_t$$

**The output of a processing unit for sequential data depends on the current input and the previous ones.**

## Vanilla RNN
As function we use $\tanh$ and the **state** consists of a single hidden vector $h$:
$$h_t = \tanh(W_{hh} h_{t-1} + W_{xh} x_t)$$ 

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
Computing gradient of $h_0$ involves many factors of $W$:
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
- **Input:** Sequence of words $x_1, x_2,\cdots x_n$
- **Output:** Sequence of words $y_1,y_2,\cdots y_m$

The encoder produces a series of states $h_1,h_2,\cdots h_n$ (starting from state $h_0$) in the following way
$$h_i = f_W(x_i, h_{i-1})$$
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
$$e_{t, i} = f(h_i, s_t)$$
The similarity scores are passed through the softmax function in order to have range from 0 to 1 and make their sum equals to 1.
$$\sum_i a_{t,i} = 1$$
$$a_{t,i} \in (0,1)$$
The context vector is given by
$$c_0 = \sum_i h_i \cdot a_{t,i}$$ 

Now, the decoder takes $s_0$ (the initial state), $y_0$ (which in this example is the placeholder word "start"), $c_0$ and computes the state $s_1$.
In general:
$$s_t = g_U(y_{t-1}, s_{t-1}, c_{t-1})$$
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
We repeat the process with $s_2,s_3\cdots s_m$, until the placeholder word “stop” is produced by the decoder.
What emerges from the previous explanation is that the decoder only needs the hidden state sequence produced by the encoder. Moreover the order of the hidden states is not important.

We could literally compute $h_1,h_2,\cdots$ and forget about $x_1,x_2,\cdots$.

Another point worth mentioning is that no labels are needed to learn the MLP that computes attention: the weights of the MLP are learned by backpropagating. Attention weights instead are computed on-line and are different based on the input sequence.

## General Formulation for Attention
**Tokens:** set of input vectors $x_1, ..., x_n \in \mathbb{R}^D$
**Features:** the characteristics of each token.

We can put these tokens together as a matrix where:
- each row is a token
- different columns refer to different features
![[09 - From RNNs to Transformers-1788674574398.webp|146]]

### Attention Coefficients
Now, we want to **transform** our tokens to another set of vectors, that we'll call output tokens $y_1, ..., y_N \in \mathbb{R}^D$.
The value of one output token $y_i$ should depend not just on the corresponding input token $x_i$ , but on all the vectors $x_1, ..., x_n$.

A simple idea is to have $y_n$ as the linear combination of the inputs:
$$y_n = \sum_{m=1}^N a_{nm} x_m \in \mathbb{R}^D$$
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
$$a_{nm} = \frac{\exp(x^T_n x_m)}{\sum_{i=1}^N \exp(x_n^T x_i)}$$
In compact notation:
$$Y = \text{Softmax}[XX^T]X$$
![[09 - From RNNs to Transformers-1788675892039.webp]]
This process is called **self-attention** because we are using the same sequence $X$ to determine the queries, keys and values.

## Self-Attention
The formula
$$Y = \text{Softmax}[XX^T]X$$
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
$$Q = X W^{(q)}, \qquad K = X W^{(k)}, \qquad V = X W^{(v)}$$
The softmax becomes:
$$Y = \text{Softmax}[QK^T]V$$

### Recap
Essentially, we are performing a linear combination of the values $V$ using a learned matrix of weights. **Differently from convolutions, these weights are not fixed, but they depend on the input signal $X$.**

### Self-Attention Layer
**Problem:** The gradients of the softmax function become exponentially small for inputs of high magnitude.

We can prevent this by re-scaling the product of queries and keys before the softmax:
$$Y = \text{Attention}(Q, K, V) = \text{Softmax}\left[\frac{Q K^T}{\sqrt{D_k}}\right]V$$
This is generally called **self-attention layer**.
![[09 - From RNNs to Transformers-1788677089308.webp|150]]

### General Attention Layer
In the **self-attention layer,** queries, keys and values are generated from the same input sequence.
![[09 - From RNNs to Transformers-1788677409977.webp|160]]

If the query are generated from another input signal, this is called **cross-attention**.
![[09 - From RNNs to Transformers-1788677437343.webp|163]]

## Multi-head Attention
The attention layer that we have seen in also called **attention head**, with multiple head: every head learn to find a **specific pattern** in the input data.

We cannot have only one head because in a sentence there can be multiple patterns, a single head would have to collapse all the pattern in a single score and it would loose precision.

For specializing every head they have **different weight matrices**. 

### Formula 
Input: $X (N \times D)$

The input has to pass through $H$ different attention heads with
$$Q_h = X W_h^{(q)}, \qquad K_h=X W_h^{(k)}, \qquad V_h = X W_h^{(v)}$$
where $h = 1,..., H$
Then we concatenate the output of the single head and transform it with a linear transformation:
$$Y(X) = \text{concat}[H_1, ..., H_H]W^{(o)}$$
![[09 - From RNNs to Transformers-1788684984975.webp|357]]
The sizes are:
![[09 - From RNNs to Transformers-1788685050907.webp|347]]

### Masked Attention Layer
Consider the word:
	"I swam across the river to get to the other bank."
We can use an attention layer to learn to generate a new word given a sequence of tokens (many-to-one).

We can use parts of the sentence to train the model, in a **self-supervised** way:
- I swam across $\rightarrow$ the
- I swam across the $\rightarrow$ river
- I swam across the river $\rightarrow$ to
- ...
![[09 - From RNNs to Transformers-1788687124960.webp]]

**Problem:** The first output vector “sees” not just the "start" token, but the whole sequence.
**Solution:** mask with a $− \infty$ the pre activations from the product $QK^T$ corresponding to the future tokens, so that their weights become $0$.

### Positional Embedding
Self-attention layer is Permutation Equivariant, it processes the sets of vectors but it does not know the order. In sentences the orders matter though.

To keep track of the ordering in the input sequence, we add at each input embedding $x_n$ a vector $r_n$. called positional embedding:
1. We can concatenate $r_n$ to $x_n$
2. We can add them $\tilde x_n = x_n + r_n$

---
# Transformers
## Transformer Blocks
Now we will see every component that composes the Transformation block.

### Post-Norm Transformer
#### Multi-head Self Attention Layer
![[09 - From RNNs to Transformers-1788696387073.webp]]

#### Skip-connection
In order to improve the training efficiency.
![[09 - From RNNs to Transformers-1788696427244.webp]]

#### Normalization Layer
The skip-connection is generally followed by a Normalization Layer
	- For each row it computes the mean $\mu_i$ and the std $\sigma_i$
$$\mu_i = \frac{\sum_j y_{i, j}}{D} \qquad \sigma_i = \sqrt{\frac{\sum_j (y_{i, j} - \mu_i)}{D}}$$
![[09 - From RNNs to Transformers-1788696446593.webp]]

### MLP Layer
This attention mechanism creates linear combinations of the value vectors, the non-linearity is introduced by the softmax function. 

**Limited expressivity:** The output vectors are constrained in a subspace of the input vectors

To improve the representational capability we process independently each vector using a shared **MLP** (e.g. two FC layers with GeLUs):
- This allows to process sequences of **variable length**
![[09 - From RNNs to Transformers-1788699924158.webp|263]]

#### Another Skip + Normalization
This is the final form of a Transformer Block:
![[09 - From RNNs to Transformers-1788699963027.webp]]

### Properties
- The name "transformer" is because it transform a set of vectors $X$ into another one $\tilde X$
- Self-attention is the only interaction between vectors:
	- Norm e MLP can be parallelized
- Since normalization is applied **after** the skip-connection, this is sometimes called **post-norm transformer**

### Pre-Norm Transformer
A variant of the previous one where the normalization in applied **inside the residual connections**. It is reported to give more stable training. 
![[09 - From RNNs to Transformers-1788700258366.webp|185]]

## Transformer Architectures
A Transformer model is a kind of architecture that relies on **Transformer Blocks as the main structure.**
Transformer architectures have been originally developed for language tasks, but have been since expanded to other kinds of data

Transformer architectures may be grouped in three kinds of meta-structures:
- Decoder
- Encoder
- Encoder-Decoder

### Transformer Decoder
It is applied in the **one-to-many problems**.
	One-to-many: take a single input and generate a word sequence as output
It is commonly used on **generative applications**,
	e.g. Generative Pretrained Transformers models (GPT). These are autoregressive models in which the conditional distribution $p(x_n | x_1, ..., x_{n-1})$ is expressed using a transformer.

Let's see how it works for a generative task by steps:
Input: a sequence of words $x_1, ..., x_n$. 
Can also contain **special words** like "start" or "pad" .

#### 1) Create the embeddings
We can use a one-hot encoding, the problems are that:
- If all the possible words is very large (e.g. all the dictionary words) we have a problem of dimensionality
- This representation does not reflect the semantic similarity between inflexible

The solution consists into using a **learnable linear projection** with a matrix of size ($\# \text{Possible words} \times \text{Dimension of the embeddings}$), so the words are embedded as:
$$e_i = E x_i$$
![[09 - From RNNs to Transformers-1788703177904.webp]]

#### 2) A Sequence of Transformer Layers
Each layer produces a sequence of $n$ words, with the same dimension as the input embeddings.
![[09 - From RNNs to Transformers-1788703266002.webp]]

#### 3) Linear + Softmax Layer
The outputs of the last transformer layer are parsed in a linear + softmax layer that produces the vectors $y_1, ..., y_{N+1}$.

A **cross-entropy loss** is attached to each output $y_i$, using as label the input token $x_{i+1}$.
![[09 - From RNNs to Transformers-1788704605764.webp]]

#### 4) Output
The generic vector $y_i$ , being the result of a softmax, can be interpreted as a probability distribution over the vocabulary. We can sample this distributions in various ways:
- **Greedy Search:** For each $y_i$, select the word with maximum likelihood (the model is deterministic)
- **Extensive Search:** maximize the joint distribution over all tokens (Very expensive)$$p(y_i, ..., y_n) = \prod_{n=1}^N p(y_n | y_1, ..., y_{n-1} )$$
- **Beam Search:** we select a parameter $B$ that is the B most probable words, the model calculates the probability between all the possible successive words and keeps the best B and and do the same with each word
- **Exploration and exploitation:** we sample not only the most probable words, but also some words sampled with uniform probability over the dictionary

### Transformer Encoder
A Transformer encoder follows a similar architecture, transforming a set of vectors to a different set.
Its purpose is to analyze and comprehend the input and transform it into a more informative representation by mapping it into another space.

#### How it works
Here's an example where the input tokens are a sentence, and some of the words have been censored with a special word "mask". The task of the model is to predict the masked words.

In this case we use non-masked transformer layers. In the decoder we had to generate a sequence and the masked transformer was needed to hide the next words. In this case we can use the whole sequence to predict the missing word.

Only the outputs related to the missing words are the ones that make the loss.

![[09 - From RNNs to Transformers-1788705417280.webp|492]]

Only the outputs corresponding to the masked words contribute to the loss.

Another observation is that here, since we use normal attention layers, the property of **permutation invariance** holds. The following example makes the issue clear.

Suppose we have two sentences:
- “The food was good, not bad at all”
- “The food was bad, not good at all”

If we had to classify the sentences with respect to the **positivity** we would get two completely different results (the first is a positive comment, the second is negative).

But if we apply an attention layer we would get the same exact result, because the two sentences are the permutation of the same words.

To avoid this, in this case, it’s imperative to use the **positional embedding**.

### Transformer Encoder-Decoder
1. An encoder encodes information from the input tokens
2. A decoder, produces a new sequence using the context from all the encoded inputs in an autoregressive manner
3. The connection between encoder and decoder blocks is given by cross-attention layers
![[09 - From RNNs to Transformers-1788706315397.webp|463]]

More in details:
![[09 - From RNNs to Transformers-1788706511439.webp|239]]

---

## Visual Transformers


---

## Swim Transformers