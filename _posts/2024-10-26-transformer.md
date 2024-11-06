---
layout: post
title: transformer architecture
date: 2024-10-26 23:36:11
description: in-depth walkthrough of transformer architecture
tags: machine-learning
# categories: machine-learning
featured: false
related_posts: false
---

The transformer architecture is made up of an encoder block and a decoder block.

The encoder's role is to process the input sequence in its entirety and distill it into a condensed vector representation. The encoder consits of a series of N=6 identical layers, each containing two principle sub-layers: a multi-head self-attention mechanism and a position-wise fully connected feed-forward network.

The decoder's role is output generation. It does this sequentially (next token prediction). The decoder also consists of N=6 identical layers. However, it includes an additional third sub-layer that facilitates multi-head attention over the encoder's output. 
<br><br>

#### Encoder In-Depth
- each word/token in the input sentence is converted into a dense vector using token embeddings
- positional encoding is added to each embedding to provide positional information. The Transformer architecture doesn't account for sequence order, so this is necessary.
- each token embedding is linearly transformed into a query, key, and value vector through learned weight matrices. We have three parameter matrices W, K, V that we will train during training time and multiplying each of these by the input token will let us get the query, key, and value vectors.
- the model computes the dot product between each token's query vector and each token's key vector in the sequence to calculate attention scores. This score gives a similarity score that reflects how much attention the two tokens should give each other.
- we follow with a scaling and applying a softmax to these scores and then multiplying by the value vectors to get a weighted sum of all value vectors
<br><br>

<div style="text-align: center;">
    {% include figure.liquid loading="eager" path="assets/img/attention.jpg"%}
    self-attention mechanism diagram - depicts steps 3, 4, and 5 above
</div>
<br>

- we then pass this output through a feed-forward neural network
- we stack 6 or more of these encoder layers together to form the entire encoder block
<br><br>

#### Decoder In-Depth
- convert the input tokens (which are the output tokens of the encoder so far) into token embeddings
- similar to the encoder block, we also apply positional encoding to the token embeddings
- the self-attention mechanism is applied with a masking mechanism to maintain that the decoder can only attend to earlier positions in the output sequence
- a second attention mechanism: now the Q vector comes from the decoder's previous layer but K and V come from the encoder's output. This attention mechanism allows the decoder to focus on relevant parts of the input sentence.
- the output of the encoder-decoder attention goes through a feed-forward network
- we stack 6 or more of these decoder layers together to form the entire decoder block
<br><br>

<div style="text-align: center;">
    {% include figure.liquid loading="eager" path="assets/img/transformer.jpg" width="80%"%}
    Transformer architecture from the paper "Attention Is All You Need" (Vaswani et al., 2017).
</div>

<br><br>

