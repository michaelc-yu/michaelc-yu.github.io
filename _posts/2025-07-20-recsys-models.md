---
layout: post
title: Recsys Models
date: 2025-07-20 00:36:00
description: Popular recsys models / algorithms
tags: machine-learning
featured: false
related_posts: false
---

List of common and popular models and algorithms used in recommendation systems.


## <b>Sequential Recommendation</b>

#### <b>GRU4Rec</b>
Many e-commerce recommender systems or news and media sites don't track user behavior across different sessions for privacy concerns, as well as the general difficulty and unreliability of doing so with cookies and browser fingerprinting. This is called session-based recommendation, where you don't know the user's long-term profile — you only observe the current session. GRU4Rec applies the GRU-based RNN for session-based recommendations. The model tries to capture short-term intent within the session.

#### <b>SASRec</b>
Self-Attentive Sequential Recommendation. Proposed in 2018 and one of the first works to apply the Transformer architecture (self-attention) to the sequential recommendation problem. It formulates sequential recommendation as next-item prediction, given a user's past interaction sequence. Self-attention helps capture both short and long-term preferences much better and more parallelizable than previous approaches like RNNs or Markov chains. It outperforms GRU4Rec on several datasets including MovieLens. It uses only the Transformer encoder stack. SASRec can process the entire concatenated history of a user — including multiple sessions — and predict the next item.


<br><br>
## <b>Multi-Armed Bandits / Exploration-Exploitation</b>
Multi-armed bandits (bandits) are a type of reinforcement learning. Their advantages over batch machine learning and A/B testing methods include continuously learning and adapting recommendations without the need for extensive data collection or offline model training. Usually performs better in situations with limited data or in cold-start scenarios.

#### <b>Epsilon-Greedy</b>
With probability ϵ, you pick a random arm (explore), and with probability 1 - ϵ, you pick the arm with the highest estimated mean reward (exploit). Note: this is not a contextual bandit approach (you don't observe the context for each decision). ϵ parameter controls the balance between exploration and exploitation. A higher ϵ means more exploration, while a lower ϵ means more exploitation. A strategy is to decrease the value of ϵ over time and allow for more exploitation.

#### <b>LinUCB</b>
These algorithms, in each trial, estimate both the mean payoff of each arm as well as a corresponding confidence interval. They then select the arm that achieves the highest upper confidence bound.<br>

$$
\begin{array}{l}
\textbf{For each arm } a: \\
\quad A_a \leftarrow I_d \\
\quad b_a \leftarrow 0_d \\[1em]

\textbf{For each round } t: \\
\quad \text{Observe context vector } x_{a,t} \text{ for all arms } a \\[0.5em]

\quad \textbf{For each arm } a: \\
\quad\quad \hat{\theta}_a \leftarrow A_a^{-1} b_a \\
\quad\quad \text{mean_reward}_a \leftarrow x_{a,t}^\top \hat{\theta}_a \\
\quad\quad \text{uncertainty}_a \leftarrow \sqrt{x_{a,t}^\top A_a^{-1} x_{a,t}} \\
\quad\quad p_a \leftarrow \text{mean_reward}_a + \alpha \cdot \text{uncertainty}_a \\[1em]

\quad \text{Choose arm } a_t \leftarrow \arg\max_a p_a \\[0.5em]

\quad \text{Observe reward } r_t \\[0.5em]

\quad \textbf{Update:} \\
\quad\quad A_{a_t} \leftarrow A_{a_t} + x_{a_t} x_{a_t}^\top \\
\quad\quad b_{a_t} \leftarrow b_{a_t} + r_t x_{a_t} \\
\end{array}
$$

Notes:<br>
-we assume each arm has its own independent linear model<br>
-pₐ is the upper confidence bound for arm a at time t<br>
-$$\alpha$$ is the exploration hyperparameter<br>
-$$x_{a,t}$$ is the context vector given by the problem, per arm, per round. Ex. Arms = articles. At time t, each article has context vector describing user profile, article topic, recency, etc.<br>
-$$x_{a,t}^\top \hat{\theta}_a$$ is best estimate of expected reward for arm a<br>
-$$\sqrt{x_{a,t}^\top A_a^{-1} x_{a,t}}$$ is how uncertain we are about this estimate (standard error)<br>


#### <b>Thompson Sampling</b>


<br><br>
## <b>Wide & Deep Architectures</b>

#### <b>Wide & Deep</b>
Wide & Deep learning jointly trains wide linear models and deep neural networks to combine the benefits of memorization (wide part) and generalization (deep part) for recommender systems.<br>
The wide component's feature crosses are interpretable and effective and capturing known, explicit relationships. However, it also requires manual feature engineering to create effective crossing of features. For example (gender x city) or (item category x user membership).<br>

The deep component is good at learning complex patterns. No manual feature engineering is required here since the model automatically learns through non-linear activation functions.<br>

The output of both wide and deep components can be either scalar (solving binary classification problem like predicting CTR) or a vector of logits (solving a multi-class classification problem).
Outputs of both components are combined to make final predictions. Can combine through either summing or concatenating and then feeding through a final dense layer.


#### <b>DeepFM</b>
Combines factorization machines (FM) for feature interactions (memorizations) and deep neural networks for generalization. Similar to Wide & Deep, but with factorization machines you don't need to manually craft cross features since FM does that for you. So it's just like Wide & Deep with ability to memorize and generalize patterns but wide part is implemented with factorization machine.<br><br>
Factorization machine formula:<br><br>
$$
\hat{y}(x) = w_0 + \sum_{i=1}^{n} w_i x_i + \sum_{i=1}^{n} \sum_{j=i+1}^{n} \langle \mathbf{v}_i, \mathbf{v}_j \rangle x_i x_j
$$
<br><br>
-$$x_i$$ are the input features<br>
-$$w_0$$ is a global bias<br>
-$$\mathbf{v}_i$$ is an embedding vector for feature i<br>
<br>
FMs generalize matrix factorization to work with any kind of feature data, not just user and item IDs. FMs give pairwise interactions.

<br><br>
## <b>Classic Methods</b>

#### <b>Matrix Factorization (MF)</b>
Simple embedding model that decomposes a sparse user-item feedback matrix into the product of two dense lower-dimensional matrices. One is the user embedding matrix and the other is the item embedding matrix. Their product approximates the sparse rating matrix. This way, we can learn embedding for users and items.

#### <b>Neural Collaborative Filtering (NCF)</b>
Replaces the dot product in matrix factorization with neural networks. We concatenate user and item vector embeddings and feed into MLP which predicts something like click or no click, rating, etc. We train on interaction data; the user and item embeddings are learned during training and they are initially random vectors. Using neural networks instead of dot product (as in matrix factorization) allows nonlinearity and ability to model more complex patterns.

<br><br>
## <b>Graph-Based Methods</b>

#### <b>GCN-based RecSys</b>
Graph Convolutional Networks learn from data where the relationships between elements are represented as a graph with nodes and edges. A GCN layer updates each node's representation by aggregating features from its neighbors:

-message passing: each node gathers information from its neighbors

-aggregation: the collected features are combined (usually via summation, mean, or max)

-transformation: the aggregated information is passed through a neural network layer (e.g., a linear transformation + activation)


Stacking multiple convolutions allows information flow across far reaches of graph

<br>
#### <b>PinSage</b>

PinSage, released in 2018, is a highly-scalable (operating on a massive graph with three billion nodes and 18 billion edges) graph convolutional network framework developed and deployed at Pinterest.

While previous GCN approaches use k-hop graph neighborhoods, PinSage defines neighborhoods based on importance of nearby nodes. The neighborhoods of a node are the T nodes that exert the most influence on that node.


##### <b>Key points about PinSage: </b>

-simulate random walks starting from node u and compute the L1-normalized visit count of nodes visited by the random walk. The neighborhood of u is then defined as the top T nodes with the highest normalized visit counts with respect to node u

-stacking multiple convolutions on top of each other to gain more information about the local graph structure around node u. Layer 0 representations are equal to the input node features, and all other layers depend on the output from the previous layer

-train PinSage using max-margin ranking loss: maximize inner product of positive pairs (q, i) (item i is a good recommendation candidate for query q), and minimize inner product of negative pairs, such that inner product of negative examples is smaller than that of positive examples by some pre-defined margin

-train with multiple GPUs via data parallelism and large batch sizes

-use re-indexing technique to create a sub-graph G' = (V', E') containing nodes and their neighborhood which will be involved in the computation of the current minibatch. Otherwise GPU accsesing data in CPU for neighborhood and feature information is slow and inefficient

