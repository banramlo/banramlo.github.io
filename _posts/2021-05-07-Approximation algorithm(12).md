---
layout: post
title: Approximation algorithm(12) - The uncapacitated facility location problem(2)
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Now, we will re visit the uncapacitated facility location problem.
Before we proceed new methodology for the uncapacitated facility location problem, we can extend some terminology for assignment costs between clients or facilities either.
For example, assignment costs between client $i$ and $j$ can be the shortest path between client $i$ and $j$.
If we define this as so, it is known to reserve the solution because of the metric closure.

Now, let's recap the problem itself.

Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ such that<br>
$\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, <br>
$x_{ij} \le y_i$ $\forall i \in F, j \in D$ and <br>
$x_{ij}, y_i \in \\{0, 1\\}$ $\forall i \in F, j \in D$.

Then, we can do LP-relaxation below.

Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ such that <br>
$\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, <br>
$x_{ij} \le y_i$ $\forall i \in F, j \in D$ and <br>
$x_{ij}, y_i \ge 0$ $\forall i \in F, j \in D$.

With above, LP dual is like follow.

Maximize $\sum\limits_{j \in D} v_j$ such that <br>
$w_{ij} \ge 0$ $\forall i \in F, j \in D$,<br>
$\sum\limits_{j \in D}w_{ij} \le f_i$ $\forall i \in F$,<br>
$v_j - w_{ij} \le c_{ij}$ $\forall i \in F, j \in D$.

Now let's define client $j$ is neighbor of facility $i$ if $v^{\star}_j$ $\ge$ $c_{ij}$ for a given dual solution $v^{\star}, w^{\star}$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.