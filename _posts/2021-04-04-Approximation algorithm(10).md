---
layout: post
title: Approximation algorithm(10) - Integer multicommodity flows
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Integer multicommodity flow problem is a problem to avoiding congetstion in edges.
It can be used at the field of circuit design because avoiding critical path is a typical problem in a circuit design.
Now, let's fomulating a problem.

Let's think about a given graph $G = (V,E)$ and pairs of vertices $\\{s_i, t_i\\} \subset V$ for $1 \le i \le k$.
There is a set of paths $P_i$ which consists of feasible paths from $s_i$ to $t_i$.
Let $P = \\{P_1, P_2, \cdots, P_k\\}$.
Then, problem can be described like "Minimize $W$ such that $\sum\limits_{P \in P_i} x_P = 1$, $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$, $x_P \in \\{0, 1\\}$".
Notice that $\sum\limits_{P \in P_i} x_P = 1$ forces to choose one path and $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$ forces to limits each edges to have at most $W$ paths going through.

Now, it can be relaxed to linear programming like follow.
"Minimize $W$ such that $\sum\limits_{P \in P_i} x_P = 1$, $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$, $x_P \ge 0$".
Then we can make a solution that is in $O(\log{n})\operatorname{OPT}$ with high probability by randomized rounding.

Algorithm is like follow.
1. Solve "Minimize $W$ such that $\sum\limits_{P \in P_i} x_P = 1$, $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$, $x_P \ge 0$".
2. Choose $P \in P_i$ with the distribution of $P_i$.

Notice that $\sum\limits_{P \in P_i} x_P = 1$ means that we don't need a normalization for $P \in P_i$.

Now, let's think about the solution $x^\star_P, W^\star$ which is given from the algorithm.
If we define a probability that a number of edges go through edge $e$ is greater than $x$ as $Pr[Y_e > x]$,
$E[Y_e]$ $=$ $\sum\limits_{i = 1}^k \sum\limits_{P \in P_i, e \in P} x^\star_P$ $=$ $\sum\limits_{e \in P} x^\star_P$ $\le$ $W^\star$.
Notice that $x^\star_P$ is an independent random variable.

If we think about chernoff bounds, we can get follow.
$Pr[Y_e \ge (1 + \delta)c\ln{n}W^\star]$ $<$ $e^{-c\ln{n}W^\star\delta^2/3}$.
Notice that $\mu$ $=$ $E[Y_e]$ $\le$ $c\ln{n}W^\star$.

Now, $W^\star \ge 1$ is not a hard assumption from the requirement of problem.
If we select $\delta = 1$, $Pr[Y_e \ge (1 + \delta)c\ln{n}W^\star]$ $<$ $e^{-c\ln{n}W^\star\delta^2/3}$ $=$ $e^{-c\ln{n}/3}$ $=$ $n^{-c/3}$.

As a result, $Pr[\max(Y_e) \ge (1 + \delta)c\ln{n}W^\star]$ $\le$ 
$\sum\limits_{e \in E}Pr[Y_e \ge (1 + \delta)c\ln{n}W^\star]$ $\le$
$\sum\limits_{e \in E}n^{-c/3}$ $=$ $\left\vert E \right\vert n^{-c/3}$ $\le$ $n^{2-c/3}$.
As a result, we can select $c$ to make resonable probability with resonable maximum $Y_e$.

If $W^\star \ge c\ln{n}$, we can select $\delta = \sqrt{\frac{c\ln{n}}{W^\star}} \le 1$ then
$Pr[Y_e \ge (1 + \delta)W^\star]$ $<$ 
$e^{-W^\star\delta^2/3}$ $=$ 
$e^{\frac{-W^\star c\ln{n}}{3W^\star}}$ $=$
$e^{-c\ln{n}/3}$ $=$ $n^{-c/3}$.

As a result, $Pr[\max(Y_e) \ge (1 + \delta)W^\star]$ $=$
$Pr[\max(Y_e) \ge (1 + \sqrt{\frac{c\ln{n}}{W^\star}})W^\star]$ $=$
$Pr[\max(Y_e) \ge W^\star + \sqrt{c\ln{n}W^\star}]$ $\le$
$\sum\limits_{e \in E}Pr[Y_e \ge W^\star + \sqrt{c\ln{n}W^\star}]$ $\le$
$\sum\limits_{e \in E}n^{-c/3}$ $=$
$\left\vert E \right\vert n^{-c/3}$ $\le$
$n^{2-c/3}$

What about the running time of the algorithm?
We can't solve the problem above in polynomial time because the number of paths is NP.
As a result, the number of variable is NP either.
Therefore, we can't even see the whole variable.

Therefore, algorithm above will be changed to equivalent algorithm.

Algorithm is like follow.
1. Solve "Minimize $W$ such that $\sum\limits_{i = 1}^{k} f_i(u, v) + f_i(v, u) \le W$, $\forall (u, v) \in E$ and $f_i$ should be a flow of value 1 from $s_i$ to $t_i$".
2. Choose path $p_i$ by choosing a vertex $u$ that adjacent to $s_i$ with probability $f_i(s_i, u)/\sum\limits_{u : u\text{ is adjacent to }s_i}f_i(s_i, u)$.
3. Extend path $p_i$ by choosing a vertex $u$ that adjacent to the last extended vertex $v$ with probability of $f_i(v, u)/\sum\limits_{u : u\text{ is adjacent to }v}f_i(v, u)$.

Notice that step 1 is equal with "Minimize $W$ such that $\sum\limits_{i = 1}^{k} f_i(u, v) + f_i(v, u) \le W$, $\forall (u, v) \in E$, $\sum_{u : (u,v) \in E, v \neq \\{s_i, t_i\\}}f_i(u, v)$ $=$ $\sum_{w : (v,w) \in E, v \neq \\{s_i, t_i\\}}f_i(v, w)$, $\sum_{v : (s_i, v) \in E}f_i(s_i, v) - \sum_{v : (v, s_i) \in E}f_i(v, s_i)$ $=$ $\sum_{v : (v, t_i) \in E}f_i(v, t_i) - \sum_{v : (t_i, v) \in E}f_i(t_i, v)$ $=$ $1$".
Now it takes polynomial time to solve.
The number of variable is $O(k\left\vert E \right\vert)$.
The number of first contraint "$\sum\limits_{i = 1}^{k} f_i(u, v) + f_i(v, u) \le W$" is at most $O(\left\vert E \right\vert)$.
The number of second contraint "$\sum_{u : (u,v) \in E, v \neq \\{s_i, t_i\\}}f_i(u, v)$ $=$ $\sum_{w : (v,w) \in E, v \neq \\{s_i, t_i\\}}f_i(v, w)$" is at most $O(k\left\vert E \right\vert|V|)$.
The number of Third contraint "$\sum_{v : (s_i, v) \in E}f_i(s_i, v) - \sum_{v : (v, s_i) \in E}f_i(v, s_i)$ $=$ $\sum_{v : (v, t_i) \in E}f_i(v, t_i) - \sum_{v : (t_i, v) \in E}f_i(t_i, v)$ $=$ $1$" is at most $O(k\left\vert E \right\vert)$.

Then, why this algorithm makes the same situation of previous algorithm?
Let's recap the previous algorithm.
"Minimize $W$ such that $\sum\limits_{P \in P_i} x_P = 1$, $\sum\limits_{P: e \in P} x_P \le W$ $\forall e \in E$, $x_P \ge 0$"
If we can select $x_P$, we can make a flow of value $x_P$ following $P$.
Then, we can decompose such flow to paths.
Notice that we can make a flow without cycle that makes better solution than flow with cycle because cycle will not change a value of flow but increase the congestion of edges.
As a result, problem above can be changed to find a flows of paths and it is the same problem we've just changed. 
In other word, proof can be easily summariezed to "New algorithm finds a feasible solution of original algorithm because we can compose flow from paths.
In the same time, old algorithm can find a feasible solution of new algoroithm because we can decompose flow to paths."

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.