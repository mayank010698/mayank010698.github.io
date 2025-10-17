---
layout: page
title: Data Assimilation
date: 2025-10-16
tags: [generative-models, flow-matching]
mathjax: true
---


We are given a set of observations $$\{o_{t}\}_{t=1}^T$$, a forecast model $p(\x_t \vert \x_{t-1})$, and a forward model $p(o_{t} \vert \x_t)$. Our goal is to infer the latent posterior $p(\x_t \vert o_{1:t})$ for all $t \in [1,T]$. 


There are two related problems:
\begin{align}
    \text{Filtering:}\quad& p(\x_t \vert o_{1:t}) \\
    \text{Prediction:}\quad& p(\x_t \vert o_{1:t-1})
\end{align}
which are related by:
\begin{align}
    p(\x_t \vert o_{1:t}) &= \frac{p(o_t \vert \x_t)p(\x_t \vert o_{1:t-1})}{\int p(o_t \vert \x_t)p(\x_t \vert o_{1:t-1})\, d\x_t} \\
    p(\x_t \vert o_{1:t-1}) &= \int p(\x_t \vert \x_{t-1})p(\x_{t-1} \vert o_{1:t-1})\, d\x_{t-1}
\end{align}


The standard approach is the Sequential Importance Resampling (SIR) algorithm, described below:
\begin{algorithm}[H]
    \caption{Sequential Importance Resampling (SIR)}
    \begin{algorithmic}
        \STATE \textbf{Initialize} a set of $n$ particles $\{\x_{0,i}\}_{i=1}^{n}\sim p_0(\x)$ and weights $\{w_{0,i}=\tfrac{1}{n}\}_{i=1}^{n}$. \\
        \FOR{$t=1\cdots T$}
            \STATE Sample from proposal $\x_{t,i}\sim \textcolor{ blue}{q_{\phi}(\x_t \mid \x_{t-1,i},\, o_t)}$.
            \STATE Update (unnormalized) weights:
            \[
                \tilde{w}_{t,i} \;=\; w_{t-1,i}\;
                \frac{p(o_t \mid \x_{t,i})\, p(\x_{t,i} \mid \x_{t-1,i})}
                     {q_{\phi}(\x_{t,i} \mid \x_{t-1,i},\, o_t)}.
            \]
            \STATE Normalize: $w_{t,i} \leftarrow \tilde{w}_{t,i}\Big/\sum_{j=1}^n \tilde{w}_{t,j}$.
            \STATE \textbf{Resample:} $\{\x_{t,i}\}_{i=1}^n \sim \sum_{j=1}^n w_{t,j}\,\delta(\x-\x_{t,j})$, and set $w_{t,i}\leftarrow \tfrac{1}{n}$.
        \ENDFOR
    \end{algorithmic}
\end{algorithm}









