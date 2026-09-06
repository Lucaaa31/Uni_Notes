# Introduction
## Centralized Learning
The typical workflow of a centralized training of DNNs:
1. **Collect data** in a single place
2. Choose a suitable **model architecture** for the tasks
3. Choose a **loss function**
4. Select an **optimization algorithm**
5. Then the training phase:
	1. Sampling of the data
	2. Forward Pass
	3. Loss Calculation
	4. Backward pass
	5. Optimization step
	6. Repeat 

### Constraints
Computational cons:
- The model does not fit the accelerator's memory
- The model fits the accelerator memory, but not with an adequate batch size
- The model fits the accelerator with adequate batch size, but training is slow

Location cons:
- Data is massively decentralized
- Training resources are decentralized

## Distributed Training
We use the **model parallelism** that is useful when model does not fit the single accelerator.
There are two types of splitting:
- **Vertical splitting:** split each layer in a different accelerator
- **Horizontal splitting:** consists into splitting the layer of the model and distribute it in multiple accelerator

### Data Parallelism
It consists into splitting data across devices.

It is useful when:
- The selected batch size doesn’t fit a single accelerator
- We aim to speed up training
![[11 - Federated Learning-1788711843029.webp|82|121x313]]It is done with this steps:
1. Replicate the model on each accelerator
2. Make each replica process a different chunk of data
3. Aggregate the results during the backward phase
4. Distribute the gradients

---
# Federated Learning
Federated learning is a machine learning setting where multiple entities (clients) **collaborate in solving a machine learning problem**, under the coordination of a central server or service provider.
Each **client’s raw data is stored locally and not exchanged or transferred**, the only information that each client sends to the server are **focused updates** (e.g. gradients or model parameters) that the server will **aggregate** with the ones that the other clients sent. 

All the devices (clients) that help to train a specific model are part of his **federation**.

Three different types of FL:
- **Horizontal FL:** clients all hold the same features, but different data
- **Vertical FL:** clients hold different features of the same samples
- **Hybrid FL:** a combination of vertical and horizontal

Approaches related to FL:
- **Split Learning:** split the execution of a model on a per-layer basis between the clients and the server
![[11 - Federated Learning-1788717366085.webp]]
## The Federated Averaging Algorithm (FedAVG)
It uses an horizontal FL setting.

The learning objective is formalized as finite-sum optimization problem, where each function is the local clients’ loss function with only access to that client’s stochastic samples:
$$
\arg\min_{\theta \in \mathbb{R}^d} \left[ f(\theta) := \frac{1}{|\mathcal{S}|} \sum_{i \in \mathcal{S}} \left( f_i(\theta) := \mathbb{E}_{d_i \sim \mathcal{D}_i} [f_i(\theta; d_i)] \right) \right]
$$where:
- $S$ is the set of all clients in the federation
- $\theta$ are the model parameters
- $D_i$ is the data distribution of the i-th client
- $d_i$ is a batch of the sample from $D_i$

### Key characteristics
- **Partial Participation:** only a small fraction $C$ of clients is selected
- **Local Training:** the selected clients train in $J$ steps on batches sampled from their local dataset
- **Aggregation Step:** the server waits all the selected client, then in aggregates the result (synchronous averaging)


## FedOpt Algorithm
Introduces a general framework for federated optimization using server and client optimizers, proposing the first methods for FL using adaptive server optimization.

The main changes are:
- **Notion of pseudo-gradient:** the update sent by the clients are interpreted as gradient
- **Use of two different optimizers (ServerOpt and ClientOpt):** this is useful because it introduce the possibility of using two different Learning Rates
- **Recovers FedAvg as special case:** if the two optimizers are SGD the algorithm is equivalent to FedAvg

### Key Challenges in FL
Statistical heterogeneity:
- **Local datasets are not i.i.d.**. This leads to **client drift** that make the convergence of the FedAvg more difficult
- **Increased number of rounds** to reach target accuracy


### SCAFFOLD
The Stochastic Variance Reduction is a solution that try to remove the problem of the client drift.

The idea is to estimate the update direction for the server model $c$ and the update direction for each client $c_i$ (control variance).

It calculates the **estimate of the client-drift and use it to correct the local update**:
$$
c-c_i
$$
This method is much **heavier w.r.t. FedAvg in terms of data transferred** and it is difficult to use with a lot of clients and low participation because **clients need to store their control variance over rounds** so it is difficult to coordinates the clients.
