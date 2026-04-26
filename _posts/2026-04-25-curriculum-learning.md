---
layout: post
title: "Curriculum Learning"
date: 2026-04-25 17:31:00
description: Notes on Curriculum Learning (Bengio et al., 2009).
tags:
  - ml-paper-summary
  - machine-learning
featured: false
related_posts: false
---

Bengio et al., 2009

### Paper Abstract

Humans and animals learn much better when the examples are not randomly presented but organized in a meaningful order which illustrates gradually more concepts, and gradually more complex ones. Here, we formalize such training strategies in the context of machine learning, and call them “curriculum learning”. In the context of recent research studying the difficulty of training in the presence of non-convex training criteria (for deep deterministic and stochastic neural networks), we explore curriculum learning in various set-ups. The experiments show that significant improvements in generalization can be achieved. We hypothesize that curriculum learning has both an effect on the speed of convergence of the training process to a minimum and, in the case of non-convex criteria, on the quality of the local minima obtained: curriculum learning can be seen as a particular form of continuation method (a general strategy for global optimization of non-convex functions).

<div style="height: 2.5rem" aria-hidden="true"></div>

### Curriculum Learning

Curriculum learning can be viewed as a sequence of training criteria. At each stage, the model is trained on a reweighted version of the data distribution:

- early stage: higher probability on easier examples or simpler concepts
- middle stages: gradually increase probability of harder examples
- final stage: remove the reweighting and train on the target distribution

So the curriculum is about how sampling weights evolve over time.


Curriculum strategies can help in many settings. However, some curricula are likely better than others, some could be neutral or unhelpful depending on task and data, and better ordering or weighting rules could likely improve results more.


The paper proposes several explanations for the gains:

- faster online training from both optimization and statistical perspectives
- less wasted effort on noisy or very hard examples before the model is ready
- better guidance through parameter space toward minima with stronger generalization

This framing connects curriculum learning to continuation methods: solve an easier version first, then smoothly move toward the full objective.

<div style="height: 2.5rem" aria-hidden="true"></div>

### Newer works

Many newer works have investigated curriculum learning further. Curriculum Learning for LLM Pretraining (Elgaar et al.) (https://arxiv.org/pdf/2601.21698) pretrain models under three linguistically motivated curricula - Age-of-Acquisition (words/concepts humans learn earlier vs. later), Word Frequency (common language first), and Verb Variation (VV) (increasing syntactic/semantic complexity) - and compare each against random ordering.

They found that curriculum helps smaller models more than larger ones. An interpretation could be that bigger models have enough capacity to absorb randomness; smaller ones benefit from a smoother learning trajectory? 

It's interesting that the paper frames curriculum learning less as a knowledge acquisition trick and more as an optimization technique. The benefit is not that the model learns fundamentally different knowledge, but that the ordering of examples reduces gradient noise and makes training updates more coherent.

This also explains why smaller models benefit more. If a model has limited capacity, noisy updates can destabilize representation learning more. Larger models, by contrast, can absorb that stochasticity because they have enough redundancy and flexibility in their parameters.


<div style="height: 2.5rem" aria-hidden="true"></div>

### Questions

- What is the right notion of difficulty for creating the curriculum?
- What about curriculum learning in multimodal or agentic settings?


