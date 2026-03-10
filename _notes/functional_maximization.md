layout: page
title: Misc
date: 2025-10-16
tags: [Controlled-Generation, debugging]
mathjax: true
---

The goal is to understand how functional maximization work.

For example the question, given a function $r(x)$, how to find the distribution that maximizes:
$$
\E_{x \sim p(x)}[r(x)]
$$

This is a constrained optimization problem. Since $p(x) \ge 0, $,$\int p(x) = 1$. In the case of discrete measures, it's a linear program with linear constraints. To solve it using gradient descent. For the continuous case, I know langevin dynamics which is the steepest descent in the KL metric. 
$$
dx_t = - \nabla r(x) dt + \sqrt{2}dB_t
$$
samples from $$x \sim p(x) = \exp(r(x))$$, but is this is the best distribution? From the philosophy of gradient descent, doing gradient descent on this objective, should give the maxima right? 
Ideally, to maximize this objective, I should be sampling from the mode of the distribution i.e $p(x) = \delta(\max_{x}r(x))$. 

What about there is a constraint i.e. $\max(\E[r(x)])$ with the maximum variance? (this should be done to cover all modes) or $\max(\E[r(x)])$ with least KL divergence to a base distribution what is the target distribution then?

oh i see now, 

$$
 \int r(x)p(x) + D_{KL}(p(x) \log \frac{p(x)}{q(x)})
$$

Differentiate 
$$
-r(x) + \log p(x) /q(x) + 1 = 0 
$$

For some reason, i ignored $\nabla p(x)$ and further derivatives maybe i was doing partial with p, then i can get $p(x) = q(x) \exp(-r(x))$, so the right distribution is the tilted distribution under reverse KL ?

okay this is p clear, now the question is actually in the adjoint matching paper i am not doing it for the final p_1 but for the KL between path measures and the reward at the end:

$$
p = \max_{p(X)} \E[r(X_1)] - D_{KL}p(X^u || X_b)
$$

in this case,

$$
p(X) = p_{base}(X)exp(r(X_1))
$$

is over path measures

$$p(X_1 \in x_1) = \int p_{base}(X ) \exp(r(X_1))$$

Now, i want to write the value function and initial point bias


I got this:

if the goal was to do the following variational optimization:

$$
\max_{p(X)} \E[r(X_1)] - D_{KL}(p(X)||p_{base}(X))
$$

then the solution would have been:
$$
p(X)\propto p_{base}(X)\exp(r(X_1))
$$

But here the caveat is, we are searching over all distributions p(X)

$$
p(X_0) = p_0 
$$
and hence 

the optimal distribution will have the solution constrained at $X_0$.

How do I formulate the new question?

one is to set the $Law(x_0) = p_0$, which is set by $\E[\phi(X_0)] = \int \phi(X_0)p_0(x_0)$. Other way is to solve the problem for each conditional $X_0$ separately. But we know the solution for each sub-problem separately, how do i take the $\max$ from outside $\E_{x_0 \sim p_0}$ to inside?

The optimal distribution for each subproblem  conditioned on X is :

$p(X | X_0) \propto p_{base}(X | X_0)\exp(r(X_1))$

but my objective function was 
$$
\max \E_{x \sim p_0}[ r(X_1)] - D_{KL}(p(X |X_0)|| p_{base}(X)

$$

not each subproblem

so looks like for each subproblem we are solving for $q(x \vert x_0)$ and then combining them using $p(x_0)$

Now i need to figure out