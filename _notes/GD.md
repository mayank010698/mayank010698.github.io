layout: page
title: Gradient Descent
date: 2026-07-07
tags: [Geometry, Optimisation]
mathjax: true
---
> **Key Question**: We keep hearing modern optimisers like Adam, Muon differ in the geometry in which they carry out their optimisation, but what does it mean? What is the right geometry for Path-space optimization? 

### Vector spaces

To deal with this question, we need to start with the definition of a vector space. A vector space $V$ is a set of vectors such that:

1. For $x,y \in V$, $x + y, x-y \in V$.
2. For any $\alpha \in \mathbb{R}$, $\alpha x \in V$.

A canonical example of a inner product is for $V = \mathbb{R}^d$:

$$
\langle x,y \rangle := \sum_{i} x_i y_i = x^{\top}y.
$$

We also need  an inner product: $\langle \cdot, \cdot \rangle: V \times V \rightarrow \mathbb{R}$:

1. $\langle \alpha x + \beta y, z \rangle = \alpha \langle x,z \rangle + \beta \langle x,z \rangle$
2. $\langle x, y \rangle = \langle y,x \rangle$.
3. $\langle x,x \rangle \ge 0$.


Two vectors $x, y \in V$ are called orthogonal if $\langle x,y \rangle = 0$. The inner product also defines a norm on $V$ via: $|| x|| = \langle x,x \rangle$ (Check that it satisfies all properties of norm).

#### Basis and Orthogonal basis 

The notion of an inner product lets us define what is known as a basis and projections. A subset $B \subseteq V$ is called a basis of $V$ if $\forall x \in V$ we can write $x$ as :

$$
 x= \sum_{i=1}^{\infty} \alpha_i b_i, \quad b_i \in B, \alpha_i \in \mathbb{R}.
$$

A basis $\mathcal{O}$ is called orthogonal if for all $x,y \in \mathcal{O}$, $\langle x,y \rangle = 0$

### Multivariable Calculus

Now that we have defined a vector space, we can define functions on vector space. Consider a differentiable function $f: V \rightarrow \mathbb{R}$. Given a vector $\varepsilon \in V$, the directional derivative $df_x(\varepsilon): V \rightarrow \R$ of $f$ along $\varepsilson $ is given by:

$$
df_x(\varepsilon) := \lim_{h \rightarrow 0} \frac{f(x + \varepsilon h) - f(x) }{h}
$$

> **For readers familiar with functional analysis**: 
> 
> The set of all $DF: = \{ df_x[\varepsilon]:  V \rightarrow \R\}$ is the set of linear functionals defined on $V$. Thus by Riesz representation theorem, there exists a unique function $f^* \in V$ such that:
>$$
    df[\varepsilon]= \langle f^{*},\varepsilon\rangle 
>$$ 
>The unique $f^{*}$ is the gradient.

#### Primer : Gradient 

In the case when the vector space is finite dimensional, i.e. $V \subseteq \R^d$, we also define what is known as a partial derivative: Given a vector $x = [x_1,x_2, \cdots, x_d]  \in \mathbb{R}^d$:

$$
\frac{\partial f}{\partial x_i} = \lim_{h \rightarrowtail 0} \frac{f(x_1, \cdots x_i + h, \cdots, x_d) - f(x_1, \cdots, x_i, \cdots, x_d)}{h}
$$

Now, informally we can see that there is a connection between the above definition of partial derivatives and directional derivatives we defined before:

$$
\begin{align*}
    f(x + \varepsilon h) &= f(x_1 + \varepsilon_1 h,\cdots, x_d + \varepsilon_d h)\\
&= (f(x_1 + \varepsilon_1 h, \cdots, x_d + \varepsilon h_d) - f(x_1, \cdots ,x_d + \varepsilon h_d)) \nonumber \\
&+ f(x_1, \cdots, x_d + \varepsilon h_d)) \\
&= \varepsilon_1 h \frac{\partial f}{\partial x_1} \bigg \vert_{x = x + \varepsilon h}+ f(x_1, x_2 + \varepsilon_2 h, \cdots, x_d + \varepsilon_d h)
\end{align*}
$$

Expanding this recursively we can get:
$$
df_{x}(\varepsilon) = \sum_{i=1}^d \varepsilon_{i}h \frac{\partial f}{\partial x_i} = \langle \nabla f,  \varepsilon \rangle 
$$

where  $\nabla f = [\frac{\partial f}{\partial x_1},\cdots, \frac{\partial f}{\partial x_d}]$ is called the **gradient**.

> Note : In defining the gradient we used the regular Eucliean inner product : $\langle u, v \rangle = \sum_{i} u_iv_i$ only as an example. In what follows, our goal is to show that "something" like $(*)$ holds for any inner product that is what people usually mean by *"geometry"*. 

> Given a vector space $V$ with an inner product $\langle \cdot, \cdot \rangle$ and a continuos function $f$, we sought out to write the directional derivative in terms of inner product with the canonical basis of the vector space and that is the notion of the ``gradient". 

Given a vector space $V$ and it's orthogonal basis $E$ we can write any vector $\varepsilon$ as :
$$
\varepsilon = \sum_{i=1}^{|B|} \langle \varepsilon, e_i\rangle e_i
$$

Now in general since $df_{x}(\varepsilon)$ is a linear functional from **Riesz representation theorem** we have:
$$
    df_{x}(\varepsilon) = \langle \nabla f(x), \varepsilon \rangle, \forall \varepsilon \in V.
$$

Now that we have defined what a gradient is and how is it is connected to the directional derivative (decrease in a specific direction $\varepsilon$), we will see how the gradient is the direction of the "steepest descent".

### Steepest Descent:
For small h, we can use Taylor's expansion we can write:
$$
f(x + \varepsilon h) = f(x) + h df_{x}(\varepsilon) + o(h)
$$
From $(*)$, we can write this as :
$$
f(x + \varepsilon h) = f(x) + h \langle \nabla f(x), \varepsilon \rangle + o(h^2) $$

The problem of steepest descent is given by:
$$
\argmin_{||\varepsilon|| \le 1} f(x + \varepsilon h) = h \langle \nabla f(x), \varepsilon \rangle \\
\varepsilon^{*} = \argmin_{||\varepsilon|| \le 1} \langle
    \nabla f(x), \varepsilon
 \rangle =  - \frac{\nabla f(x)}{||f(x)||}. 
$$

> Remark: The direction of steepest descent depends on the geometry of the space $V$. A reader like me would ask why are we considering a unit norm ball? On one hand, if we dont impose any constraint we can make the objective arbitrary small by choosing a scale $c$ that makes the objective go to $-\infty$. Other way is to choose some other constraint:
> $$
    \varepsilon^* = \argmin_{B(\varepsilon) \le  1} \langle \nabla f(x), \varepsilon \rangle
>$$ 
The choice of (B) determines what “unit movement” means. The above minimization problem with a hard constraint can also be formulated as a Lagrangian:

$$
 \varepsilon^*, \lambda^* = \argmin_{\varepsilon} \max_{\lambda} \langle \nabla f(x), \varepsilon \rangle + \lambda (B(\varepsilon) - 1)
$$
using a Dual function approach. For many choices of (B), the hard-constrained steepest descent problem does not need to be solved from scratch. It is the problem of minimizing a linear functional over a unit ball, and its solution is characterized by the dual norm:

$$\min_{B(\varepsilon)\le 1}\langle g,\varepsilon\rangle
=-B^*(g),
$$

where $B^*$ is the dual norm. The minimizer is the direction that achieves this dual norm. For the Euclidean norm, this gives

$$
\varepsilon^* = 
-\frac{g}{||g||_2}.
$$

For an ellipsoidal norm $B(\varepsilon)^2=\varepsilon^\top G\varepsilon$, this gives

$$
\varepsilon^* = -\frac{G^{-1}g}{\sqrt{g^\top G^{-1}g}}.
$$

For a more general $B$ look at Mirror Descent.

Thus the hard problem is mostly useful for defining the notion of steepest direction. The soft-penalty version is often used algorithmically because it directly returns the update scale:

$$
\Delta^*
= \arg\min_\Delta

\langle g,\Delta\rangle
+
\frac{1}{2\eta}B(\Delta)^2
$$

The soft penalty version of the same problem is known as the proximal gradient descent.

#### Proximal Gradient Descent

$$
\argmin_{\varepsilon} \langle \nabla f(x), \varepsilon \rangle + \frac{1}{2\eta} B(\varepsilon) = \argmin_{x} f(x) + \frac{1}{2\eta} B(x - x')
$$

> **Proximal Policy Optimization**: People familiar with Reinforcement Learning (RL) have heard of Proximal Policy Optimization (PPO). In flavor of the notation there the space is a space of possible policies $\pi: (S \times \mathcal{A}) \rightarrow \mathbb{R}_{+}$ is not a vector space (since sum of two probability distributions is not a probability distribution in general) and there is no notion of inner product. Instead we have a space of distributions with bounded second moment $\mathcal{P}_2\mathbb(\R^d)$ and distance metrics like KL-divergence,$W_2$ which may not be induced by a inner product.  We will deal with gradient descent in such spaces in the Gradient Descent in metric spaces section.

#### Mirror Descent

In the proximal step, we can have unusual choices of the ball $B$. One such choice is what is known as Bregman Divergence (Banerjee et. al.):

> **Bregman Divergence** : For a convex function $\phi$, Bregman Divergence is the difference $B(x,y)$ is the difference between the value $B(y)$ and it's linearised approximation from $x$:
> $$
        D_{\phi}(x,y) = \phi(y) - (\phi(x) + \langle \nabla \phi(x),y-x\rangle)
> $$ 


The proximal gradient descent gives:
$$
    x_{t+1} = \argmin_{x} f(x) + \frac{1}{2\eta}D_{\phi}(x,x_t)
$$

leading to interesting geometrical choices.

### Preconditioner
Up until now, we have assumed we have a inner product on the space and motivated choosing directions that adhere to the intrinsic inner product. This is good, but in most cases our goal is to achieve the smallest $f$ globally, while $\nabla f$ is a local update i.e.:
$$
    x^* = \argmin_{x} f(x)
$$
To achieve this, we can manipulate the local geometry such that we can reach the optima faster? In other words,the choice of inner product space is an artificial choice we make to see the local space more "nicely" and can be adaptive. 

A breakthrough in Machine Learning is the adaptive geometry, i.e. instead of having a euclidean or a fixed we construct an adaptive notion of the geometry. AdaGrad (Adaptive Gradient) estimates the local curvature. 
#### Generalized Minimising Movement 
#### (Optional) Gradient descent in metric spaces

This extends it to spaces without inner product utilising the notion of metric derivatives


### Good Optimizers for Path-Space Optimization

We can now return to the motivating question: what would it mean to design a good optimizer for path space?

Consider the controlled SDE

$$
dX_t = b_\theta(X_t,t)\,dt + \sigma(t)\,dB_t,
\qquad X_0 \sim \mu,
\qquad t\in[0,1].
$$

For each parameter $\theta$, this SDE induces a probability measure

$$
P_\theta \in \mathcal P(C([0,1],\mathbb R^d))
$$

over continuous paths. Thus there are two spaces involved:

$$
\theta \in \mathbb R^m
$$

and

$$
P_\theta \in \mathcal P(C([0,1],\mathbb R^d)).
$$

Ordinary optimizers such as SGD, Adam, or Muon operate on the parameter space $\mathbb R^m$. But the object we actually care about may be the induced path measure $P_\theta$. This raises the question: should the optimizer measure movement in parameter space, or in path-measure space?

A simple path-space objective is

$$
\max_\theta
\left\{
\mathbb E_{P_\theta}[r(X_1)]
-
\operatorname{KL}(P_\theta\|P_0)
\right\},
$$

where $P_0$ is a reference path measure, such as Brownian motion. Equivalently, we can minimize

$$
\min_\theta
\left\{
-\mathbb E_{P_\theta}[r(X_1)]
+
\operatorname{KL}(P_\theta\|P_0)
\right\}.
$$

This objective is naturally expressed in terms of path measures, not just parameters. Therefore, an optimizer that is geometrically natural for $\theta$-space may not be geometrically natural for $P_\theta$-space.

One possibility is to use a proximal update directly on path measures:

$$
P_{k+1}
=
\arg\min_P
\left\{
\mathcal F(P)
+
\frac{1}{\eta}D(P\|P_k)
\right\},
$$

where $D$ measures movement between path measures. If $D$ is KL divergence, this becomes a path-space analogue of mirror descent or proximal optimization. If $D$ is induced by an action or Wasserstein-type geometry, we obtain a different notion of movement.

This perspective suggests that a good optimizer for path space should not only adapt learning rates in parameter coordinates. It should respect the geometry of trajectories: time coupling, endpoint constraints, stochasticity, and the cost of changing an entire path distribution.

Recent work such as Proximal Diffusion Neural Sampler moves in this direction. It frames diffusion-based neural sampling as a stochastic optimal control problem on path measures and applies a proximal point method on the space of path measures. Its proximal steps create a sequence of easier subproblems, gradually moving the learned path measure toward the desired target and improving exploration across modes.

However, this still leaves an optimizer-design question. Many practical algorithms instantiate the path-space proximal step through a particular training objective and then optimize the neural parameters using standard optimizers such as Adam. This separates the geometry of the outer path-measure problem from the geometry of the inner parameter update.

A natural question is whether one can design adaptive optimizers whose preconditioning is itself path-aware. For example, instead of treating discretized time points as unrelated coordinates, the optimizer could use a temporal/operator geometry such as

$$
\langle u,v\rangle_{H^1}
=
\int_0^1 \langle u_t,v_t\rangle\,dt
+
\lambda\int_0^1 \langle \dot u_t,\dot v_t\rangle\,dt.
$$

Such a geometry would penalize nonsmooth changes across time and couple neighboring time slices. This would be the path-space analogue of moving from Euclidean gradient descent to preconditioned or adaptive gradient descent.
#### Questions that came in my mind when writing this:

Is BD a norm?
Connections to newton step
Furhter the set ball $B$ infact may not be induced by the inner product, can be a pseudonorm (Bregman, adaptive (based on gradient))








