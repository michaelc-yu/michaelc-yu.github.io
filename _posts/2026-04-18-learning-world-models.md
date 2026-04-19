---
layout: post
title: "Learning and Leveraging World Models in Visual Representation Learning"
date: 2026-04-18 12:00:00
description: Notes on IWM vs JEPA, latent prediction, and invariant vs equivariant representations (Garrido et al., arXiv:2403.00504).
tags:
  - ml-paper-summary
  - machine-learning
featured: false
related_posts: false
---

Garrido et al., arXiv:2403.00504

### Paper Abstract

Joint-Embedding Predictive Architecture (JEPA) has emerged as a promising self-supervised approach that learns by leveraging a world model. While previously limited to predicting missing parts of an input, we explore how to generalize the JEPA prediction task to a broader set of corruptions. We introduce Image World Models, an approach that goes beyond masked image modeling and learns to predict the effect of global photometric transformations in latent space. We study the recipe of learning performant IWMs and show that it relies on three key aspects: conditioning, prediction difficulty, and capacity. Additionally, we show that the predictive world model learned by IWM can be adapted through finetuning to solve diverse tasks; a fine-tuned IWM world model matches or surpasses the performance of previous self-supervised methods. Finally, we show that learning with an IWM allows one to control the abstraction level of the learned representations, learning invariant representations such as contrastive methods, or equivariant representations such as masked image modelling.

<div style="height: 2.5rem" aria-hidden="true"></div>

### Pixel-level prediction

Initial versions of world models tried focusing on predicting next frames in a video, at the pixel level. The problem is it's impossible to predict what the next frame is, because you don't know what exactly will happen. For example, if you wanted to predict what happens as you turn the camera around from the professor to a lecture room, the model can't predict how many students are there, or where they're sitting. It can make a guess, but that's all. So what happens is it can predict the average, which leads to the predicted frames being blurry and stuff.

<div style="height: 2.5rem" aria-hidden="true"></div>

### JEPA: predict in representation space

Yann LeCun’s JEPA (joint embedding predictive architecture) avoids raw pixels and instead predicts abstract, high-level representations, emphasizing semantics over fine detail. For example to predict where Jupiter will be in 100 years you don’t need every fact about the planet, you only need to know like three positions and three velocities.

<div style="height: 2.5rem" aria-hidden="true"></div>

### How IWM relates to JEPA

This paper’s method is a specialized JEPA-style setup. Rather than only predicting a missing patch, it predicts the embedding of a transformed view (e.g. brightness, crop, rotation). IWM conditions the predictor on transformation parameters, predicts how transformations change embeddings, and keeps the predictor (world model) for downstream use.

<div style="height: 2.5rem" aria-hidden="true"></div>

### Invariant vs equivariant representations

- Invariant: e.g. rotate the image → embedding stays the same (same class identity).
- Equivariant: e.g. rotate the image → embedding changes in a systematic way that tracks the transformation.

Practical use: invariant models tend to suit linear evaluation (simple linear probes). Equivariant models are emphasized when you want world-model / finetuning behavior that respects structure under transformations.

<div style="height: 2.5rem" aria-hidden="true"></div>

### Notation and training objective

- $$I$$ = original image (e.g. dog in the park).
- $$T$$ = transformation (e.g. dim brightness).
- Target view $$y = T(I)$$.
- Source view $$x$$ = $$y$$ with another transformation applied (additional corruption / view).

Encoder: ViT for both views (same weights).

$$
z_x = \text{encoder}(x), \quad z_y = \text{encoder}(y)
$$

Predictor (world model): MLP takes $$z_x$$ (and conditioning on the transformation, as in the paper) and outputs $$\hat{z}_y$$.

Loss: squared L2 between $$\hat{z}_y$$ and $$z_y$$ (stop-gradient on target branch as in standard JEPA-style training).

<div style="height: 2.5rem" aria-hidden="true"></div>

### Questions

- How do self-supervised vision methods map onto this world model framing?
- Is the decoder / predictor from pretraining what you should call the world model end-to-end, or only the predictor block?
- Could we use video from video games as data?
- Explicit physics rules inside the model?
- Would learning physics rules require a much larger predictor?
