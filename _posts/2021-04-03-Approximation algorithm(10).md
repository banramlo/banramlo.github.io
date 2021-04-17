---
layout: post
title: Approximation algorithm(10) - Chernoff bounds
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Let $X_1, X_2, \cdots, X_n$ be $n$ independent varaible which are between $0$ and $1$.
Then, <br>
$Pr[X \ge (1 + \delta)U] < (\frac{e^{\mu}}{(1 + \mu)^{1 + \mu}})^U$,<br>
$Pr[X \le (1 - \delta)L] < (\frac{e^{-\mu}}{(1 - \mu)^{1 - \mu}})^L$.<br>
For $X = \sum\limits_{i=1}^n X_i$, $\mu = E[X]}$, $L \le \mu \le U$ and $\delta > 0$.

If $0 \le \delta < 1$ then <br>
$Pr[X \ge (1 + \delta)u] < (\frac{e^{\mu}}{(1 + \mu)^{1 + \mu}})^U < e^{-U\mu^2/3}$,<br>
$Pr[X \le (1 - \delta)u] < (\frac{e^{-\mu}}{(1 - \mu)^{1 - \mu}})^L < e^{-L\mu^2/2}$<br>

Proof will be updated later.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.