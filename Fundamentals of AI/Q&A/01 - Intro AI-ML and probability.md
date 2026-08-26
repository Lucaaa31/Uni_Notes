# 1) List and briefly describe the three main ML paradigms
First of all, we need to define what learning means, in order to do this we need 3 main components:
1. Experience E (data)
2. Task T
3. Performance measure P
Having all these definitions we can say that: "An Agent learns when its performance at task T, measured by P, improves with experience E".
We can divide Machine Learning paradigms in:
- **Supervised Learning**
	- We give the models the following input $(x_1, y_2), (x_2, y_2), ..., (x_n, y_n)$ where $x_i$ is the input and $y_i$ is the associated label (the ground truth)
	- The purpose of the program is to learn a function $f : y_i = f(x_i)$
	- The model during the training knows what the result should be by looking to $y_i$
	- The type  of $y_i$ determines the task
		- $y_i$ categorical we are in a classification task
		- $y_i$ numerical we are in a regression task
- **Unsupervised Learning**
	- We give in input only $x_i$ without the labels
	- The purpose of the program is to find hidden structures between the input data
	- Example of this paradigm can be clustering that consists into divide the input data into groups, or dimensionality reduction that aims to reduce the dimensionality of the input data
- **Reinforcement Learning**
	- It is based on a Trial and Error paradigm that has the following rules:
		- Agent and Environment interact at discrete time steps $t = 0, 1, 2, K$
		- The Agent observe the state at step t: $s_t \in S$ and produces an Action in this step: $a_t = A(s_t)$ and gets resulting Reward: $r_{t+1} \in R$ and resulting next state: $s_{t+1}$.
	- RF aims, under these rules, to make the agent in capable of performing specific tasks by giving him iteratively feedbacks based on his actions.
