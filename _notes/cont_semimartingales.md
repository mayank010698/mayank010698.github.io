---
layout: page
title: Continous Semimartingales
date: 2025-10-16
tags: [generative-models, flow-matching]
mathjax: true
---


Defintion (Radon Nikodym Derivative): Let $\mu,\nu$ be measures such that $\mu << \nu$ i.e. $\mu$ is absolutely continous w.r.t. $\nu$. The Radon-Nikodym derivate is given as :
$$
\frac{d\mu}{d\nu} 
$$

A signed measure is the difference of two finite positive measures.

Definition (Finite Variation): Let $T \ge 0$
. A continous function $a : [0,T] \rightarrow \R$
such that $a(0) = 0$ is said to have a finite variation if there exists a signed measure $\mu$ on $[0,T]$ such that $a(t) = \mu([0,t])$
 for every $t \in [0,T]$.

 For $\mu$, the total variation is given as $\abs{\mu} = \mu_{+} + \mu_{-} $. 

 The measure $\abs{\mu}$ is called the total variation of $a$.

 Let $f : [0,T] \rightarrow \R$ be a measurable function such that $ \int_{[0,T]} \abs{f(s)}\abs{\mu}(ds) < \infty$. We set:
 $$
 \int_{0}^{T} f(s)da(s) = \int_{[0,T]} f(s)\mu(ds)
 $$
$$
 \int_{0}^{T} f(s)\abs{da(s)} = \int_{[0,T]} f(s)\abs{\mu}(ds)
 $$

 Proposition 4.2 . For every $t \in (0,T]$, 
 $$
 \int_{0}^T \abs{da(s)} = \{ \sum_{i=1}^p (\abs{a(t_i)} - a(t_{i-1})\}
 $$

 Lemma 4.3 If $f : [0,T] \rightarrow \R$ is a continuous function, and if $0 =  t_0^n < t_1^n < \cdots $ is a sequence of subdivisions of $[0,T]$ whose mesh values tends to $0$, we have
 $$
 \int_{0}^T f(s)da(s) = \lim_{n \rightarrow \infty} \sum_{i=1}^p f(t_{i-1}^n) 
 $$

 Definition 4.4 An adapted process $A = (A_t)_{t \ge 0}$ is called a finite variation process if all its sample paths are finite variation functions on $\R_+$. If in addition the sample paths are nondecreasing functions, the process $A$ is called an increasing process.

 If $A$ is a finite variation process, the process:
 $$
 V_t = \int_{0}^t \abs{dA_s}
 $$
 is an increasing process.

 Remark: For each $\omega$ $V_t(\omega)$ is a sample path, and then I know what above means. 
 $$
 V_t(\omega) = \int_0^t \abs{dA_s(\omega)}
 $$

 Proposition 4.5 Let $A$ be a finite variation process, and let $H$ be a progressive process such that
 $$
 \forall t \ge 0, \forall \omega \in \Omega, \int_{0}^t \abs{H_s(\omega)} \abs{dA_s(\omega)} < infty.
 $$
 Then the process $H \cdot A = ((H\cdot A)_t)_{t\ge 0}$ defined by:
 $$
 (H \cdot A)_t = \int_{0}^t H_s dA_s
 $$
 is also a finite variation proces. 

 A good example is when $A_t = t$.

 Definition An adapted process $M = (M_t)_{t \ge 0}$ with continuous sample paths and such that $M_0 = 0$ a.s. is called a continuous local martingale if there exists a nondecreasing sequence $(T_n)_{n \ge 0}$ of stopping times such that $T_n \uparrow \infty$ and for every $n$ the stopped process $M^{T_n}$ is a **uniformly integrable martingale**.

 Some examples of continous local martingales that are not (true) martingales. 

Proposition 4.7:
- A nonnegative continuous local martingale $M$ such that $M_0 \in L^1$ is a supermartingale. 
- A continuous local martingale $M$ such that there exists a random variable $Z \in L^1$ with $\abs{M_t} \le Z$ for every $t \ge 0$ is a uniformly integrable martingale.

Theorem 4.9 Let $M = (M_t)_{t\ge 0}$ be a continuous local martingale. There exists an increasing process denoted by $(\langle M,M\rangle_t)_{t\ge 0}$ which is unique upto indistinguishability such that $M_t^2 - \langle M,M \rangle_t$ is a continous local martingale. Furthermore, 
$$
\langle M,M \rangle_t = \lim_{n \rightarrow \infty} \sum_{i=1}^{p_n} (M_{t_i^n} - M_{t_{i-1}^n})^2
$$
in probability. For Brownian Motion
$$
\langle B,B \rangle_t = t
$$





