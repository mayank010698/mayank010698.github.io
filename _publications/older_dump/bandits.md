---
title: "Max-Quantile Grouped Infinite-Arm Bandits"
authors: "Ivan Lau, Yan Hao Ling, <strong>Mayank Shrivastava</strong>, Jonathan Scarlett"
venue: "ALT, 2022"
year: 2022
url: "https://arxiv.org/abs/2210.01295"
paper_link: "https://neurips.cc/virtual/2024/poster/96942"
tldr: "In this paper, we consider a bandit problem in which there are a number of groups each consisting of infinitely many arms. Whenever a new arm is requested from a given group, its mean reward is drawn from an unknown reservoir distribution (different for each group), and the uncertainty in the arm's mean reward can only be reduced via subsequent pulls of the arm. The goal is to identify the infinite-arm group whose reservoir distribution has the highest (1−α)-quantile (e.g., median if α=1/2), using as few total arm pulls as possible. We introduce a two-step algorithm that first requests a fixed number of arms from each group and then runs a finite-arm grouped max-quantile bandit algorithm. We characterize both the instance-dependent and worst-case regret, and provide a matching lower bound for the latter, while discussing various strengths, weaknesses, algorithmic improvements, and potential lower bounds associated with our instance-dependent upper bounds."
---
