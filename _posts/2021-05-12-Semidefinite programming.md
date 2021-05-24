---
layout: post
title: Semidefinite programming
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

To show what is semidefinite programming, we need to know what is semidefinite matrix.
Notice that $x$ $=$ $R^{n}$ $=$ $\begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$.

1. $\mathcal{S}_n$ is the set of $n$ by $n$ symmetric matrix.
2. We call symmetric matrix $X$ $\in$ $\mathcal{S}_n$ is positive semidefinite if for all $x \in \mathcal{R}^{n}$, $x^TXx \ge 0$.
3. For $A,B$ $\in$ $\mathcal{S}_n$, inner product of $A,B$ is defined as $\<A,B\> = tr(A^T B)$

Followings are equivalent for $X$ $\in$ $\mathcal{S}_n$.
1. $X$ is positive semidefinitive
2. All eigenvalues of $X$ are non negative
3. $X$ $=$ $V^TV$ for some $V$ $\in$ $R^{n \times n}$
4. $X$ $=$ $\sum\limits_{i = 1}^{n} \lambda_i w_i w_i^T$ for some $\lambda_i \ge 0$ and vector $w_i$ $\in$ $R^n$ such that $w_i^T w_i = 1$ and $w_i^T w_j = 0$ for all $i \neq j$

A semifinite program is a mathematical program where the varaibles form a symmetric matrix and the objective function and constraints are all linear.
Notice that this program is just a linear programming but variables and constraints are semidefinitive matrices.
In other word, it's like follow.

### Semidefinite programming

Minimize or maximize $\sum\limits_{i,j} c_{ij} x_{ij}$<br>
such that $\sum\limits_{i,j} a_{ijk_1} x_{ij}$ $=$ $b_{k_1}$ $\forall k_1$<br>
$\sum\limits_{i,j} a_{ijk_2} x_{ij}$ $\ge$ $b_{k_2}$ $\forall k_2$<br>
$\sum\limits_{i,j} a_{ijk_3} x_{ij}$ $\le$ $b_{k_3}$ $\forall k_3$<br>
$X$ $=$ $\begin{pmatrix} x_{11} & x_{12} & \cdots & x_{1n} \\\ x_{21} = x_{21} & x_{22} & \cdots & x_{2n} = x_{n2} \\\ \vdots & \vdots & \ddots & \vdots\\\ x_{n1} = x_{1n} & x_{n2} = x_{2n} & \cdots & x_{nn} \end{pmatrix}$ is semidefinitive.

The program above is equivalent with the program below if $X = V^TV$.
Notice that If $V$ $=$ $\begin{pmatrix} v_1, v_2, \cdots, v_n\end{pmatrix}$ then $X$ $=$ $\begin{pmatrix} \<v_1, v_1\> & \<v_1, v_2\> & \cdots \\\ \vdots &   & \ddots & \vdots \\\ \cdots & \cdots & \<v_n, v_n\> \end{pmatrix}$.
This program is so-called vector program because it has vector as variables.

### Vector program

Minimize or maximize $\sum\limits_{i,j} c_{ij} \<v_i, v_j\>$<br>
such that $\sum\limits_{i,j} a_{ijk_1} \<v_i, v_j\>$ $=$ $b_{k_1}$ $\forall k_1$<br>
$\sum\limits_{i,j} a_{ijk_2} \<v_i, v_j\>$ $\ge$ $b_{k_2}$ $\forall k_2$<br>
$\sum\limits_{i,j} a_{ijk_3} \<v_i, v_j\>$ $\le$ $b_{k_3}$ $\forall k_3$<br>



{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.