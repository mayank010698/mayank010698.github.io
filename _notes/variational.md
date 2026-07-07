---
layout: page
title: Schrodinger Bridge
date: 2025-10-16
tags: [Controlled-Generation, debugging]
mathjax: true
---

## Schrödinger Bridge Problem

Let \(R\) be a reference path measure on continuous trajectories
\(C([0,1]; \mathbb{R}^d)\), corresponding to the diffusion process
\[
dZ_t = b(Z_t,t)\,dt + dB_t, \qquad Z_0 \sim r_0,
\]
where \(B_t\) is standard Brownian motion and \(r_0\) is the initial
distribution of the reference process.

Given two probability distributions \(p_0\) and \(p_1\) on
\(\mathbb{R}^d\), the Schrödinger Bridge problem consists of finding a
path measure \(P\) such that
\[
P_0 = p_0, \qquad P_1 = p_1,
\]
and minimizing the Kullback–Leibler divergence with respect to the
reference measure \(R\):
\[
\min_{P:\,P_0=p_0,\;P_1=p_1} \mathrm{KL}(P \,\|\, R).
\]

When the reference dynamics is Brownian motion (i.e. \(b \equiv 0\)),
the optimal solution \(P^\star\) is absolutely continuous with respect
to \(R\) and corresponds to a controlled diffusion of the form
\[
dZ_t = u^\star(Z_t,t)\,dt + dB_t,
\]
where the control drift \(u^\star\) is chosen to match the prescribed
initial and terminal distributions \(p_0\) and \(p_1\), while remaining
closest to the reference dynamics in the relative entropy sense.


Questions: 
1. Should the initial distribution for the reference dynamics be $p_0$ as well?
2. If i look at the forward process of diffusion models (OU process, VPSDE, VESDE), then I think the SB will be the one that reaches to $p_noise$ at terminal time T, and then if i look at the reverse process, then SB will be the one that reaches $p_data$ at terminal time 0 and then we can do this recursively.
Alg :

1. Take the forward process : dx_t = -x_t dt + dB_t
2. learn a SB to P_T = noise at time = 1
3. let that process be dx_t = (-x_t + b)dt + dB_t
4. then take the reverse process dx_t = -x_t + \sigma \sigma^T \nabla \log p(x_t)) +dB_t and then learn SB to go to p_{data}
5. and then rinse repeat?