---
layout: post
title: LLM optimizations
date: 2024-10-26 23:36:10
description: walkthrough of various model optimization techniques
tags: machine-learning
# categories: machine-learning
typograms: true
featured: false
related_posts: false
---


## Optimizing the attention mechanism

### Multi-query attention

Multi-query attention (MQA) shares the keys and values among all the attention heads. The query vector is still projected multiple times, as before, but there is one set of keys and values for all heads. While the amount of computation done in MQA is identical to multi-head attention, the amount of data (keys, values) read from memory is a fraction of before. When bound by memory-bandwidth, this enables better compute utilization. It also reduces the size of the KV-cache in memory, allowing space for larger batch sizes.

### Grouped-query attention

Grouped-query attention (GQA) projects keys and values to a few groups of query heads. It has more key-value heads than one, but fewer than the number of query heads. It's a balance between multi-head attention and multi-query attention.

{% include figure.liquid loading="eager" path="assets/img/gqa.jpg" %}

### Flash attention

Another way of optimizing the attention mechanism is to modify the ordering of certain computations to take better advantage of the memory hierarchy of GPUs. Neural networks are generally described in terms of layers, and most implementations are laid out that way as well, with one kind of computation done on the input data at a time in sequence. This doesn’t always lead to optimal performance, since it can be beneficial to do more calculations on values that have already been brought into the higher, more performant levels of the memory hierarchy.

Fusing multiple layers together during the actual computation can enable minimizing the number of times the GPU needs to read from and write to its memory and to group together calculations that require the same data, even if they are parts of different layers in the neural network.

One very popular fusion is FlashAttention, an I/O aware exact attention algorithm, as detailed in FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness. Exact attention means that it is mathematically identical to the standard multi-head attention (with variants available for multi-query and grouped-query attention), and so can be swapped into an existing model architecture or even an already-trained model with no modifications.

I/O aware means it takes into account some of the memory movement costs previously discussed when fusing operations together. In particular, FlashAttention using “tiling” to fully compute and write out a small part of the final matrix at once, rather than doing part of the computation on the whole matrix in steps, writing out the intermediate values in between.

{% include figure.liquid loading="eager" path="assets/img/flashattn.jpg" %}

----

## Modifications to model weights

### Quantization
Quantization is the process of reducing the precision of a model's weights and activations. Most models are trained with 32 or 16 bits of precision, where each parameter and activation element takes up 32 or 16 bits of memory - a single-precision floating point. However, most deep learning models can be effectively represented with eight or even fewer bits per value.

### Sparsity
Similar to quantization, it's been shown that many deep learning models are robust to pruning, or replacing certain values that are close to 0 with 0 itself. Sparse matrices are matrices where many of the elements are 0. These can be expressed in a condensed form that takes up less space than a full, dense matrix.

### Distillation
This process involves training a smaller model that's called a student model to mimic the behavior of the larger model (the teacher model). The student model will be trained to mirror the performance of the teacher model, with a loss function that measures the discrepancy between their outputs. DistilBERT compresses a BERT model by 40% while retaining 97% of its language understanding capabilities at a 60% faster speed.



