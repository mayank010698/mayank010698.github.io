---
layout: page
title: IGW curves
date: 2026-07-16
tags: []
mathjax: true
---

By Lemma $3.2$, the $W_2$ geodesic is a valid $\texttt{IGW}$-Lipschitz curve connecting $\mu_0,\mu_1$, hence $\texttt{Lip}_{\texttt{IGW}}([0,1],\mathcal{P}_2(\mathbb{R}^d)) \neq \phi$ and $d_{\texttt{IGW}} < \infty$ always. Consider now a sequence of curves $\{\rho_n\}_{n\in N}$, reparametrized to be uniformly Lipschitz with constant $\text{Lip}(\rho_n) = \mathcal{l}_{\text{IGW}}(\rho_n) < 2d_{\text{IGW}}(\mu_0,\mu_1)$, such that $l$

<!-- In mathematical analysis, **semicontinuity**, is a property of extended real-valued functions that is weaker than continuity. An extended real-valued function, f is upper (respectively, lower) semincontious at point $x_0$, if, roughly speaking, the function values for arguments near $x_0$ are not much higher then $f(x_0)$. Briefly, a function on a domain $X$ is lower semi-continous if its epigraph $\{(x,t) \in X \times \mathbb{R}: t \ge f(x)\}$ is closed on $X \times \mathbb{R}$, and upper semi-continous. -->

### Lower semicontinuity:

A function $f: X \rightarrow \mathbb{R}$ is called lower semi-continous at a point $x_0 \in X$ if for every real $y < f(x_0)$ there exists a neighborhood $U$ of $x_0$ such that $f(x) > y$ for all $x \in U$. Equivalently, $f$ if lower semi-continuous at $x_0$ if and only if, 
$$
\liminf_{x \rightarrow x_0} f(x)\ge f(x_0)
$$ 
where $\liminf$ is the limit inferior of the function $f$ at point $x_0$.


**Lemma 4.1**: For any $\mu \in \mathcal{P}_2(\mathbb{R}^d)$, we have:

(i): $\forall r > 0$, $B_{IGW}(\mu,r)$ is weakly compact, i.e. any sequence has a weakly convergent subsequence.

(ii): $IGW(\mu, \nu)$ is weakly l.s.c. in $\nu$.