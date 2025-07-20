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

#### <b>SASRec</b>

#### <b>GRU4Rec</b>


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



## <b>Wide & Deep Architectures</b>

#### <b>Wide & Deep</b>

#### <b>DeepFM</b>


## <b>Classic Methods</b>

#### <b>Matrix Factorization (MF)</b>

#### <b>Neural Collaborative Filtering (NCF)</b>


## <b>Graph-Based Methods</b>

#### <b>PinSage</b>

#### <b>GCN-based RecSys</b>


