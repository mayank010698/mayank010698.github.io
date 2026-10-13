---
layout: page
title: Bregman Divergences
date: 2026-07-16
tags: []
mathjax: true
---

### Exponential Families

Consider a measurable space $(\Omega,\mathcal{B})$ where $\mathcal{B}$ is a $\sigma$-algebra on the set $\Omega$. Let $\mathbb{t}$ be a measurable mapping from $\Omega$ to a set $\mathcal{T} \subseteq \mathbb{R}^d$, where $\mathcal{T}$ may be discrete. Let $p_0: \mathcal{T} \mapsto \mathbb{R}_{+}$ be any function such that if $(\Omega, \mathcal{B})$ is endowed with a measure $dP_0(\omega) = p_0(\mathbb{t}(\omega))d\mathbb{t}(\omega)$, then $\int_{\omega \in \Omega} dP_0(\omega) < \infty$. The measure $P_0$ is a.c. with respect to the Lebesgue measure $d\mathbb{t})\omega$. 

Now $t(\omega)$ is a random variable from $(\Omega, \mathcal{B}, P_0)$ to $(\mathcal{T},\sigma(\mathcal{T}))$ where $\sigma(\mathca{T})$. Let $\Theta$ be defined as the set of all parameters $\theta \in \mathbb{R}^d$ for which:
$$
\int_{\omega \in \Omega} \exp(\langle \theta, t(\omega)\rangle)dP_0(\omega) < \infty 
$$

Based on the definition of $\Theta$, it is possible to define a function $\varphi: \Theta \rightarrow \mathbb{R}$ such that:

$$
\varphi(\theta) = \log \left( \int_{\omega \in \Omega} \exp(\langle \theta, t(\omega) \rangle)dP_)(\omega)\right)
$$


A family of probability distributions $\mathcal{F}_{\varphi}$ parametrized by a $d$-dimensional vector $\theta \in \Theta$ such that the pdf with respect to the measure $dt(\omega)$ can be expressed in the form:

$$
f(\omega; \theta) = \exp(\langle \theta, t(\omega) \rangle - \varPhi(\theta))p_0(t(\omega))
$$

is called an *exponential family* with *natural statistic* $t(\omega)$, natural parameter $\theta$. 