---
layout: page
title: The relation between OT and SB (Chen and Pavon)
date: 2026-07-16
tags: []
mathjax: true
---

Let $\Omega := C([0,1],\mathbb{R}^n)$ denote the family of $n$-dimensional continuous functions, let $W_x$ denote the Wiener measure on $\Omega$ starting at $x$ at $t=0$, and let:
$$
W:= \int W_x dx
$$
be the stationary Wiener measure. Let $\mathbb{D}$ be the family of distributions on $\Omega$ that are equivalent to $W$. By Girsanov's theorem, under $Q \in \mathbb{D}$, the coordinate process $x(t,\omega) = \omega(t)$ admits the representation:

$$
\begin{align}
    dx(t) = \beta_{+}^{Q}dt  + dw_{+}(t), \quad \beta_{+}^{Q}~\text{is}~\mathcal{F}_t^{+}~\text{adapted}, \\
    dx(t) = \beta_{-}^{Q}dt  + dw_{-}(t), \quad \beta_{-}^{Q}~\text{is}~\mathcal{F}_t^{-}~\text{adapted},
\end{align}
$$

where, $\mathcal{F}_t^{+}$ and $\mathcal{F}_t^{-}$ are $\sigma$-algebras of events observable upto time $t$ and from time $t$ on, respectively. Moreover,

$$
Q\left[\int ||\beta_{+}^{Q}||^2dt < \infty\right] = Q\left[\int ||\beta_{-}^{Q}||^2dt < \infty\right]  = 1.
$$

For $Q,P \in \mathbb{D}$, we define *relative entropy* $H(Q,P)$ of $Q$ with respect to $P$ as :

$$
H(Q,P) = \mathbb{E}_{Q}\left[\ln \frac{dQ}{dP}\right]
$$

It then follows from Girsanov's theorem that:

$$
\begin{align}
    H(Q,P) &= H(q_0,p_0) + \mathbb{E}_{Q}\left[\int_{0}^{1} \frac{1}{2}||\beta_{+}^{Q} - \beta_{+}^{P}||^2 dt\right] \\
    &= H(q_1,p_1) + \mathbb{E}_{Q}\left[\int_{0}^{1} \frac{1}{2}||\beta_{-}^{Q} - \beta_{-}^{P}||^2 dt\right]
\end{align}
$$

The sketch of the proof goes as follows:
$$
\begin{align}
    \frac{dQ}{dW}&= q_0(x_0)\exp\left[\int_{0}^{1}\beta_{+}dx - \int_{0}^{1}\frac{1}{2}||\beta_{+}^{Q}||^2dt\right]
\end{align}
$$

Hence, 
$$
\begin{align}
    \ln \frac{dQ}{dP} &= \ln \frac{q_0(x_0)}{p_0(x_0)} +  
\end{align}
$$

## Schrodinger Bridge Problem 

Now, let $\rho_0$ and $\rho_1$ be two everywhere positive probability densities. Let $\mathbb{D}(\rho_0,\rho_1)$ be the set of distributions in $\mathbb{D}$ having the prescribed marginals at $0$ and $1$. Given $P \in \mathbb{D}$, we consider the following problem:

$$
\text{Minimize}~H(Q,P)~\text{over}~\mathbb{D}(\rho_0,\rho_1)
$$

If there is atleast one $Q$ in $\mathbb{D}(\rho_0,\rho_1)$ such that $H(P,Q) < \infty$, there exists a unique minimizer $Q^*$ in $\mathbb{D}(\rho_0,\rho_1)$ called the Schr$\"{o}$dinger Bridge from $\rho_0$ to $\rho_1$ over $P$. Indeed, let:

$$
P_x^y = P[\cdot \vert x(0) = x, x(1) = y], \quad Q_{x}^{y} = Q[\cdot \vert x(0) = x, x(1) = y]
$$
be the disintegrations of $P$ and $Q$ with respect to the initial and final positions. Let also

$$
\mu^P = P[(x(0),x(1))\in (\cdot)], \quad \mu^Q = Q[(x(0),x(1)), \in (\cdot)]
$$

be the joint initial-final time distributions $P$ and $Q$ respectively. Then, we have:

$$
P = \int P_x^y(\cdot)\mu^P(dx,dy), \qquad Q = \int Q_x^y(\cdot)\mu^Q(dx,dy)
$$

By the multiplication formula:

$$
\frac{dQ}{dP} = \frac{d\mu^Q}{d\mu^P}((x(0),x(1)))\frac{dQ_{x(0)}^{x(1)}}{dP_{x(0)}^{x(1)}}, \qquad Q~\text{a.s.},
$$


we get:
$$
\begin{align}
    H(Q,P)&= \mathbb{E}_Q\left[\ln \frac{dQ}{dP}\right] = \E_Q\left[\ln \frac{d\mu_Q}{d\mu_P}\right] + \E_Q\left[
        \frac{dQ_{x(0)}^{x(1)}}{dP_{x(0)}^{x(1)}}
    \right]
\end{align}
$$
This is the sum of two non-negative quantities. The second becomes zero if and only if:
$$
Q_x^y = P_x^y, \quad \mu^Q~\text{a.s.}
$$

Thus, as already observed by Schr$\"{o}$dinger, the problem reduces to minimizing:

$$
\int \left(\ln \frac{d\mu^Q}{d\mu^P}\right)d\mu^Q
$$

subject to linear constraints:
$$
\mu^Q(dx \times \mathbb{R}^n) = \rho_0(x)dx, \quad \mu^Q(\R^n \times dy) = \rho_1(y)dy
$$

By the results of Beurlin-Jamison-F$\"{o}$llmer, this problem has a solution $\mu^*$ and, 

$$
Q^* = \int P_x^y(\cdot) \mu^*(dx,dy)
$$

## Stochastic Optimal Control Formulation

Consider now the case where (the coordinate process under) $P$ is a Markov diffusion with forward drift field $b_{+}^P(x,t)$ and transition density $p(x,\sigma,\tau, y)$. The one-time density $\rho(x,t)$ of $P$ is a weak solution of the Fokker-Planck equation:

$$
\frac{\partial \rho}{\partial t} + \nabla \cdot \left(b_{+}^P\rho\right) - \frac{1}{2}\Delta \rho = 0.
$$

Moreover, forward and backward drifts are related through the Nelson's relation:

$$
b_{-}^P(x,t) = b_{+}^P(x,t) - \nabla \ln \rho(x,t)
$$

Then $Q^*$ is also Markovian with forward drift field:

$$
b_{+}^{Q^*}(x,t) = b_{+}^P(x,t) + \nabla \ln \varphi(x,t)
$$

where the (everywhere positive) function $\varphi$ solves together with another function $\hat{\varphi}$ the system, 

$$
\begin{align}
    \varphi(t,x) &= \int p(t,x,1,y)\varphi(1,y)dy\\
\hat{\varphi}(t,x) &= \int p(0,y,t,x)\hat{\varphi(0,y)}dy
\end{align}
$$

with boundary conditions

$$
\begin{align}
    \varphi(x,0)\hat{\varphi}(x,0) = \rho_0(x), \quad \varphi(x,1)\hat{\varphi}(x,1) = \rho_1(x).
\end{align}
$$

Moreover, the one-time density $\tilde{\rho}$ of $Q^{*}$ satisfies the factorization:

$$
\tilde{\rho}(x,t) = \varphi(x,t)\hat{\varphi}(x,t), \quad \forall t \in [0,1].
$$


Let us give an elementary derivation. Let $\varphi(x,t)$ be any positive, space-time harmonic function, namely $\varphi$  satisfies on $\mathbb{R}^n \times [0,1]$

$$
\begin{align}
    \frac{\partial \varphi}{\partial t} + b_{+}^P\cdot \nabla \varphi + \frac{1}{2}\Delta \varphi = 0.
\end{align}
$$

It follows that $\ln \varphi$ satisfies:

$$
\begin{align}
    \frac{\partial \ln \varphi}{\partial t} + b_{+}^P\cdot \nabla \ln \varphi  + \frac{1}{2}\Delta \ln \varphi = -\frac{1}{2}|| \nabla \ln \varphi||^2
\end{align}
$$

Observe that, in view of this, this is equivalent to minimizing over $\mathbb{D}(\rho_0,\rho_1)$ the functional:

$$
\begin{align}
    I(Q) = \mathbb{E}_Q\left[\int_0^1 \frac{1}{2}||\beta_{+}^Q - b_{+}^P(x,t)||^2dt - \ln \varphi(x(1),1) + \ln \varphi(x(0),0)\right]
\end{align}
$$

THis follows from the fact that $H(Q,P)$ and 35 differ from a quantity which is constant over $\mathbb{D}(\rho_0,\rho_1)$. Observe that under $Q$, by Ito's rule:

