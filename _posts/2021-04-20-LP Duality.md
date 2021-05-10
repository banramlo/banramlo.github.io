---
layout: post
title: LP Duality
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

For any given LP, we can construct LP like one of the format follow.

### Minimization problem
For given vectors
$X = \begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$.

Minimize $C^T X$ such that <br>
    $a_i^T X \ge b_i$ for $i \in C_p$,<br>
    $a_i^T X \le b_i$ for $i \in C_m$,<br>
    $a_i^T X = b_i$ for $i \in C_e$,<br>
    $x_i \ge 0$ for $i \in B_p$,<br>
    $x_i \le 0$ for $i \in B_m$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.