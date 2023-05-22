---
title: "ContraBAR: Contrastive Bayes-Adaptive Deep RL"
date: 2023-05-22
layout: post
use_math: true
---
In this blog post I'll explain my paper _ContraBAR: Contrastive Bayes-Adaptive Deep RL_, written under the supervision of Professor Aviv Tamar.
This post assumes a basic working knowledge of Reinforcement Learning (RL) and Machine Learning.

We'll begin by understanding the potential of Meta RL and fast adaptation in RL. From there we'll continue to _POMDPs_ and _information states_, followed by the algorithmic and theoretic contribution of our paper.

# Meta RL in a nutshell

## Motivation

Those with prior experience with RL know that learning new tasks can be painfully slow. In an ideal world, we would like our agent to learn a policy over many different tasks, and be able to quickly adapt to new unknown tasks. 

## Definition
Meta RL formalizes this idea into a well-defined mathematical framework. We define each task $$\mathcal{T}$$ as an MDP $$(\mathcal{S}, \mathcal{A}, \mathcal{P}, \mathcal{R})$$, and make the following assumptions:

- A prior $$P$$ over the MDPs 
- The state and action spaces are the same for all tasks, but the reward and transition functions are task specific. $$P$$ is a distribution over the transition and reward functions: $$\mathcal{T} \sim P(\mathcal{P},\mathcal{R})$$

In meta RL we wish to find a policy $$\pi$$ that maximizes the following

$$\begin{aligned}
max_{\pi} J(\pi) \mathbb{E}_{\mathcal{R},\mathcal{P}} \mathbb{E}_{\pi} \left[ r(s_t,a_t) \right]
\end{aligned}$$

An optimal solution $$\pi^{*}$$ for the meta RL problem is termed a Bayes-optimal policy.

## Bayes-optimality

We know that the Bayes-optimal policy is the optimal for the meta RL problem, but what are the implications of this? 

Our objective is formulated in such a way that an optimal policy must take into account the prior over the MDPs as well as the stochasticity of the sampled reward and transition functions. This means the policy performs optimal exploration in order to maximize cumulative reward -- essentially a Bayes-optimality adapts quickly. What does this "fast adaptation" look like? Let us illustrate with a simple example.

Imagine we have a set of environments where an agent is in a 2D world. For all the environments, the state and action spaces are both $$\mathbb{R}^2$$. For each specific environment, the reward is sparse and only given at a small radius around a location on a semi-circle of radius $$1$$. Since only the reward function varies, our prior is uniform over the unit circle. We now ask what should Bayes-optimal behavior look like? It turns out that for deterministic domains with a single sparse reward, the Bayes-optimal solution is essentially to search all possible reward locations so as to maximally reduce uncertainty, and then go directly to the goal in subsequent episodes. This means traveling along the semi-circle until the goal is found, and then going directly to the goal. This simplistic environment helps illustrate the sense in which a Bayes-optimal solution "adapts quicklky". Given a new, unknown, the optimal method of exploration is indeed to scan the semi-circle and then go directly to the goal in subsequent episodes.
