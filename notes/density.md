layout: page
title: Density transformation under gaussian pertubation
date: 2025-10-16
tags: [notes,self]
mathjax: true
---

$$
x_{t+1} \gets x_t - \eta \nabla f(x_t)dt + \sqrt{2\eta}dB_t
$$
For a small $\Delta t$,
$$
x_{t+1} \gets \mathcal{N}(x_t - \eta \nabla f(x_t), 2\eta )
$$
$$
\partial_t p = \int \mathcal{N}(x_t - \eta \nabla f(x_t), 2\eta )p(x_t)dx_t

$$