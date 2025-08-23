---
layout: post
title: "Score Matching (Hyvärinen, 2005)"
date: 2025-08-22
tags: [paper-notes]
math: true
---
Inline: $x \in \R^d$, $\E[X]$, $\norm{x}_2$, $\inner{x}{y}$.

Display:
$$
\grad f(x) = A^\top (Ax - b), \quad
\int f(x)\, \d x
$$

Aligned:
\[
\begin{aligned}
\min_x \quad & f(x) \\
\text{s.t.} \quad & x \in \R^d
\end{aligned}
\]
