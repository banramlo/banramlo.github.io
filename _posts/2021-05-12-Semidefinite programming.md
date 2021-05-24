---
layout: post
title: Semidefinite programming
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

To show what is semidefinite programming, we need to know what is semidefinite matrix.

1. $S_n$ is the set of $n$ by $n$ symmetric matrix.
2. We call symmetric matrix $X$ $\in$ $S_n$ is positive semidefinite if for all $x \in \mathcal{R}^{n}$, $x^TXx \ge 0$.
3. For $A,B$ $\in$ $S_n$, inner product of $A,B$ is defined as $\<A,B\> = tr(A^T B)$

Followings are equivalent for $X$ $\in$ $S_n$.
1. $X$ is positive semidefinitive
2. All eigenvalues of $X$ are non negative
3. $X$ $=$ $V^TV$ for some $V$ $\in$ $R^{n \times n}$
4. $X$ $=$ $\sum\limits_{i = 1}^{n} \lambda_i w_i w_i^T$ for some $\lambda_i \ge 0$ and vector $w_i$ $\in$ $R^n$ such that $w_i^T w_i = 1$ and $w_i^T w_j = 0$ for all $i \neq j$

A semifinite program is a mathematical program where the varaibles form a symmetric matrix and the objective function and constraints are all linear.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.