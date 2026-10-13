---
layout: page
title: Discrete Langevin 
date: 2026-07-15
tags: [Sampling, Discrete]
mathjax: true
---

### Introduction

### Markov Semigroup Theory

#### Markov Kernel 
Let $(X,\mathcal{A})$ and $(Y,\mathcal{B})$ be measurable spaces. A Markov Kernel with source $(X,\mathcal{A})$ and target $(Y,\mathcal{B})$  is a function $\kappa : B \times X \rightarrow [0,1]$ with the following properties:

1. For every fixed $B_0 \in \mathcal{B}$, the map $x \mapsto \kappa(B_0,x)$ is $\mathcal{A}$-measurable.
2. For every $x_0 \in X$, the map $B \mapsto \kappa(B,x_0)$ is a probability measure on $(Y, \mathcal{B})$. 

Let $P$ be a Markov Kernel. It generates a discrete-time semigroup $(P^n)_{n \in N}$. 

#### Generator
We define the generator of the semigroup to be the operator $\mathcal{L} := P - \text{id}$, acting on $L^2(\pi)$ via $Pf(x) := \int f(x)P(x,dy)$ where $\pi$ is the stationary distribution for $P$. Note that since the operator norm of $P$ is atmost 1, then $P - \text{id}$ is always a negative operator. 

### Reversibility

A discrete time chain with Markov Kernel $P$ is called reversible with respect to $\pi$ if it holds that $\pi(dx)P(x,dy) = \pi(dy)P(y,dx)$ 

### Spectral Gap

The spectral gap of $P$ is the largest $\lambda > 0$ such that for all $f \in L^2(\pi)$ with $\mathbb{E}_{\pi}[f] = 0$, 
$$
\mathcal{E}(f,f):= \langle f, (-\mathcal{L}f)\rangle_{L^2(\pi)} \ge \lambda || f|| ^2_{L^2(\pi)}
$$

Markov Chain can be seen as a gradient descent on the Dirichlet Energy $\mathcal{E}$.

$$
\text{ent}_{\pi}(f) \le \frac{C_{\text{MLSI}}}{2}\mathcal{E}(f,\log f)
$$

Note: In the context of Discrete time Markov Chain, the chain rule fails and different versions of Log Sobolev are not equivalent.

It is trickier to deduce entropy decay from MLSI in discrete time and to avoid this issue we shall work in continuous time. 

TODO:

- [ ] Convert the MALA proof to a discrete proof
- [ ] Write the continuation of the discrete score.      