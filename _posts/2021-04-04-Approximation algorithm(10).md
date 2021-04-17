---
layout: post
title: Approximation algorithm(11) - Integer multicommodity flows
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Integer multicommodity flow problem is a problem to avoiding congetstion in edges.
It can be used at the field of circuit design because avoiding critical path is a typical problem in a circuit design.
Now, let's fomulating a problem.

Let's think about a given graph $G = (V,E)$ and pairs of vertices $\\{s_i, t_i\\} \subset V$, for $1 \le i \le k$.
There is a set of paths $P_i$ which consists of feasible paths from $s_i$ to $t_i$.
Let $P = \\{P_1, P_2, \codts, P_k\\}$.
Then, problem can be described like "Minimize $W$ such that $\sum\limits_{P \in P_i} x_P = 1$, $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$, $x_P \in \\{0, 1\\}$".
Notice that $\sum\limits_{P \in P_i} x_P = 1$ forces to choose one path and $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$ forces to limits each edges to have at most $W$ paths going through.

Now, it can be relaxed to linear programming like before.
"Minimize $W$ such that $\sum\limits_{P \in P_i} x_P = 1$, $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$, $x_P \ge 0$".
Then we can make a solution that is in $\operatorname{OPT} + \sqrt{c\opertorname{OPT}\ln{n}}$ with high probability by randomized rounding.

Algorithm is like follow.
1. Solve "Minimize $W$ such that $\sum\limits_{P \in P_i} x_P = 1$, $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$, $x_P \ge 0$".
2. Choose $P \in P_i$ with the distribution of $P_i$.

Notice that $\sum\limits_{P \in P_i} x_P = 1$ means that we don't need a normalization for $P \in P_i$.

Now, let's think about the solution which is given from the algorithm.


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.z