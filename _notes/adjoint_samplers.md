---
layout: page
title: Adjoint Samplers
date: 2026-07-16
tags: [Stochastic Control,]
mathjax: true
---

### Schrodinger Bridge

Let $R$ denote the path law of the reference Brownian Motion:
$$
dX_t = dB_t, \quad X_0 \sim p_0
$$

Amongst all path measures $P$ satisfying:

$$
P_0=p_0, \quad P_1=p_1
$$
the Schrodinger Bridge is:

$$
P^{*} = \argmin_{P: P_0=p_0, P_1=p_1} \text{KL}(P||R)
$$

By using Girsanov's theorem:
$$
KL(P||R) = E_{P^{\theta}}\left[\frac{1}{2}\int_{0}^{1}||u_{\theta}(X_t,t)||^2dt\right] + \text{KL}(P_1 || \mathcal{N}(0,I)) 
$$

The crux of all adjoint-based methods is how can we use this formulation for *sampling* from unnormalized densities i.e. given access to unnormalized densities $\nabla \log p(x)$. 

### Path Integral Sampler

#### Basic Algorithm:

1. Simulate a forward trajectory using the SDE:
   $$
   dX_t = u_{\theta}(X_t,t)dt + dB_t
   $$

2. Backpropagate through the objective function




### KL-Divergence on Path Space

Given two probability measures $P$ and $Q$ defined on a path space $\Omega$, for example

$$
\Omega = C([0,1];\mathbb{R}^d),
$$

the KL divergence between $P$ and $Q$ is defined as

$$
\mathrm{KL}(P||Q)=
\mathbb{E}_{P}
\left[
\log \frac{dP}{dQ}
\right]
=
\int_{\Omega}
\log\left(\frac{dP}{dQ}(\omega)\right)
P(d\omega),
$$

provided that $P\ll Q$.

Here, $\omega\in\Omega$ denotes an entire sample path, and

$$
X_t(\omega)=\omega_t
$$

is the coordinate process.

#### 1. Radon-Nikodym Derivative

Given two measures $\mu$ and $\nu$ such that

$$
\mu\ll \nu,
$$

the Radon--Nikodym theorem states that there exists a measurable function $f$ such that, for every measurable set $A$,

$$
\mu(A)
=

\int_A f(x)\nu(dx).
$$

This function is denoted by

$$
f(x)
=

\frac{d\mu}{d\nu}(x)
$$

and is called the Radon--Nikodym derivative of $\mu$ with respect to $\nu$.

Thus, in the path-space setting,

$$
P(A)
=

\int_A
\frac{dP}{dQ}(\omega),
Q(d\omega),
$$

where $\frac{dP}{dQ}(\omega)$ is now a function of the entire path $\omega$.

---

#### 2. Conditioning on the Endpoints

Define the endpoint random variable

$$
Y(\omega)
=

(X_0(\omega),X_1(\omega)).
$$

Let $P_{01}$ and $Q_{01}$ denote the joint distributions of the endpoints under $P$ and $Q$, respectively:

$$
P_{01}
=

P\circ (X_0,X_1)^{-1},
\qquad
Q_{01}
=

Q\circ (X_0,X_1)^{-1}.
$$

By the disintegration theorem, we can write the path measures as

$$
P(d\omega)
=

P(d\omega\mid x_0,x_1),
P_{01}(dx_0,dx_1),
$$

and similarly,

$$
Q(d\omega)
=

Q(d\omega\mid x_0,x_1),
Q_{01}(dx_0,dx_1).
$$

This notation should be understood through integration. For any measurable function $F:\Omega\to\mathbb{R}$,

$$
\int_{\Omega}
F(\omega)P(d\omega)
=

\int_{\mathbb{R}^d\times\mathbb{R}^d}
\left[
\int_{\Omega}
F(\omega)
P(d\omega\mid x_0,x_1)
\right]
P_{01}(dx_0,dx_1).
$$

The conditional measure $P(d\omega\mid x_0,x_1)$ is therefore the distribution of the entire path conditioned on

$$
X_0=x_0,
\qquad
X_1=x_1.
$$

---

#### 3. Chain Rule for KL Divergence

The corresponding Radon--Nikodym derivative factorizes as

$$
\frac{dP}{dQ}(\omega)
=

\frac{dP_{01}}{dQ_{01}}(X_0(\omega),X_1(\omega))

\frac{
dP(\cdot\mid X_0,X_1)
}{
dQ(\cdot\mid X_0,X_1)
}(\omega).
$$

Therefore,

$$
\log\frac{dP}{dQ}(\omega)
=

\log
\frac{dP_{01}}{dQ_{01}}(X_0,X_1)
+
\log
\frac{
dP(\cdot\mid X_0,X_1)
}{
dQ(\cdot\mid X_0,X_1)
}(\omega).
$$

Taking expectation with respect to $P$ gives the path-space chain rule

$$
\begin{aligned}
\mathrm{KL}(P||Q)
&=
\mathrm{KL}(P_{01}||Q_{01})
+
\mathbb{E}_{(X_0,X_1)\sim P_{01}}
\left[
\mathrm{KL}
\left(
P(\cdot\mid X_0,X_1)
||
Q(\cdot\mid X_0,X_1)
\right)
\right].
\end{aligned}
$$



1. From Girsanov theorem, the function $D_t = \frac{dP}{dQ}$ is well defined and is $D_t = \mathbb{E}[D_{\infty} \vert \mathcal{F}_t]$.

From Girsanov's theorem:

$$
\log \frac{dP}{dQ}\bigg\vert_t = \int_{0}^{t}u(X_s,s)dB_s  - \frac{1}{2}\int_{0}^{t}||u(X_s,s)||^2ds
$$

### Reward tilted path-measure

A simple idea to sample from $p(x)$ is the following:

1. Generate a sample path: $X \sim Q(X)$.
2. Importance weighting: $w(X) = \frac{p(X_T)}{q(X_T)}$.
3. There is an interpretation in terms of path-reweighing:
   $$
    \begin{align}
    \tilde{Q}(dw) &= \frac{p(\omega_T)}{q(\omega_T)}Q(dw) \\
    \frac{\tilde{Q}(dw)}{Q(dw)} &= \frac{p(\omega_T)}{q(\omega_T)} 
    \end{align}
    $$


4. **Perils**: The variance of the importance weights, $\frac{p(\omega_T)}{q(\omega_T)}$ can be high leading to *poor samples*. Note that the importance weights $w(x) = \frac{p(x)}{q(x)}$ are itself random variables and hence:
   $$
    \mathbb{E}\left[w(x)\right] = \int \frac{p(x)}{q(x)}dQ(x) = 1
   $$
   $$
   \text{Var}(w(x)) = \chi^2(p || q)
   $$

   In the one-sided SB, they coincide exactly.


#### Semimartingale

A semimartingale is a stochastic process that can be decomposed as a sum of a martingale and process with finite variation.





### Path Integral

Path Integral is the value function:
$$
V(x) &= \mathbb{E}\left[ \int_{t}^{1} g(x_t)\vert X_t = x\right]
$$


####
Coupling technique: Two chains driven by the same B.M.:

$$
\begin{align}
    x_{t+1} &= x_{t} + b(x_{t},t)h + z_t \\
    \hat{x}_{t+1} &= \hat{x}_{t} + \hat{b}(\hat{x}_{t},t)h + z_t \\
    || x_{t+1} - \hat{x}_{t+1} || &= || (x_{t} + b(x_{t},t)h) - ( \hat{x}_{t} + \hat{b}(\hat{x}_{t},t)h)||\\
    &\le || x_t - \hat{x}_t || + || b(x_t,t) - \hat{b}(\hat{x_t},t)||\Delta t \\
    &\le || x_t - \hat{x}_t ||+ || b(x_t,t) - \hat{b}(x_t,t)||\Delta t \nonumber \\
    &\qquad  + || b(x_t,t) - b(\hat{x}_t,t)||\Delta t \\
    &= 
\end{align}
$$

It depends on the lipschitz constant of the control and approximation error.