---
layout: post
title: Markov's inequality
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

If $X$ is a nonnegative random variable and $a > 0$ then,
$Pr[X \ge a]$ $\le$ $\frac{E(X)}{a}$.

Proof is like follow.
First of all, $E(X | X \ge a)$ $\ge$ $a$ because condition means that varaible is greater or equal than $a$.
Simillary, $E(X | X < a)$ $\ge$ $0$ because $X$ is a nonnegative random variable.
Then, $E(X)$ $=$ $Pr[X < a] \cdot E(X | X < a)$ $+$ $Pr[X \ge a] \cdot E(X | X \ge a)$ $\ge$ $Pr[X \ge a] \cdot E(X | X \ge a)$ $\ge$ $Pr[X \ge a] \cdot a$.
Therefore, claim holds