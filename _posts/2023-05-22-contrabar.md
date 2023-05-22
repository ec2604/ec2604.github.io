---
title: "ContraBAR: Contrastive Bayes-Adaptive Deep RL"
date: 2023-05-22
layout: post
use_math: true
---
In this blog post I'll explain my paper _ContraBAR: Contrastive Bayes-Adaptive Deep RL_, written under the supervision of Professor Aviv Tamar.
This post assumes a basic working knowledge of Reinforcement Learning (RL) and Machine Learning.

We'll begin by understanding the potential of Meta RL and fast adaptation in RL. From there we'll continue to _POMDPs_ and _information states_, followed by the algorithmic and theoretic contribution of our paper.

# What is the potential of Meta RL?

For those with prior experience with RL, learning new tasks can be painfully slow. In an ideal world, we would like our agent to learn a policy over many different tasks, and be able to quickly adapt to new unknown tasks. 

Meta RL formalizes this idea into a well-defined mathematical framework. We define each task $\mathcal{T}$ as an MDP $(\mathcal{S}, \mathcal{A}, \mathcal{P}, \mathcal{R})$, and make the following assumptions:

- A prior $P$ over the MDPs 
- The state and action spaces are the same for all tasks, but the reward and transition functions are task specific. $P$ is a distribution over the transition and reward functions: $\mathcal{T} \sim P(\mathcal{P},\mathcal{R})$

In meta RL we wish to find a policy $\pi$ that maximizes the following

$$ 
max_{\pi} J(\pi) \mathbb{E}_{\mathcal{R},\mathcal{P}} \mathbb{E}\_{\pi} \left[ r(s_t,a_t) \right] 
$$

The optimal solution $\pi^{*}$ is termed the Bayes-optimal solution. This solution must take into account the prior over the MDPs as well as the stochasticity of the sampled reward and transition functions. 
