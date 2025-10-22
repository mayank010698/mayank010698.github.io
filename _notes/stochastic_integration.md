---
layout: page
title: Stochastic Integration
date: 2025-10-16
tags: [generative-models, flow-matching]
mathjax: true
---


Let $M$ be a continous local martingale with $M_0 \in L^2$.

We write $\mathbb{H}^2$ for the space of all continous martingales $M$ which are bounded in $L^2$ and such that $M_0=0$. Equivalently, $M$ is a continous local martingale such that $\E[\langle M,M\rangle_{\infty}] < \infty$. 

We define a symmetric bilinear form on $\mathcal{H}^2$ via the formula:
$$
(M,N)_{\mathbb{H}^2} - \E[\langle M,N \rangle_{\infty}] = \E[M_{\infty}N_{\infty}]
$$
which yields a norm on $\H^2$




