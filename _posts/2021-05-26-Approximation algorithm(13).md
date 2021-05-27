---
layout: post
title: Approximation algorithm(13) - Buy-at-bulk network design
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

In the real world, it is usually cheaper when you buy some thing in a bulk because of the delivery costs.
Now think about a undirected graph $G$ $=$ $(V,E)$ with length $l_e$ $\ge$ $0$ for all $e$ $\in$ $E$.
This problem requires $k$ pairs of vertices $(s_i,t_i)$ and demand $d_i$.
One thing that makes this problem interesting is that there is a cost function $f(u)$ such that satisfies following.

1. $f(0)$ $=$ $0$
2. $f$ is non-decreasing.
3. $f$ is subadditive which means $f(x + y)$ $\le$ $f(x)$ $+$ $f(y)$ for all $x, y$ $\in$ $\mathbb{N}$

Now problem asks to find $k$ paths from $s_i$ to $t_i$ with capacity $c:E \rightarrow \mathbb{N}$
to minimize $\sum\limits_{e \in E}f(c_e)l_e$
such that for any edge we can send $d_i$ units of commodity from $s_i$ to $t_i$ at the same time without violating $c$.

Notice that this algorithm can be solved in polynomial time if $G$ is a tree.
The reason is like follow.
If you think about any possible path on a tree, there is a unique path from $u$ to $v$ where $u,v$ $\in$ $V$.
Therefore, solution is just find a unique path and set the capacity and that's all.
Notice that this means it's not just in polynomial time but it gives a trivial unique solution.

Now, let's consider the following algorithm.

<div class="alg">
    $\operatorname{for}$ each pair of vertices $u,v$ in $V$
    <div class="alg">
        $P_{uv}$ be the shortest path from $u$ to $v$ in $E$
    </div>
    $d_{uv}$ be the length of shortest path from $u$ to $v$.<br>
    Find a tree metric $(V',T)$ that approximates $d$<br>
    $\operatorname{for}$ each pair of vertices $u,v$ in $V$
    <div class="alg">
        $P'_{uv}$ be the shortest path from $u$ to $v$ in $T'$
    </div>
    $c_e \leftarrow 0$ for all $e$ $\in$ $E$<br>
    $\operatorname{for}$ each pair $s_i, t_i$
    <div class="alg">
        $\operatorname{for}$ each edge in $P'_{s_i t_i}$
        <div class="alg">
            Increase $c_e$ $\leftarrow$ $c_e$ $+$ $d_i$ 
        </div>
    </div>
    return $c$
</div>

First of all, we need to show that there is a tree metric.
Therefore we need to show that $d$ is a pseudometric.
Notice that most of properties are trivial but only triangular inequality need to be more detial.
Now, let's think about any path between $(x,y)$ and $(y,z)$.
Then, $d_{xy}$ $+$ $d_{yz}$ $=$
$\sum\limits_{e \in P_{xy}}l_e$ $+$ $\sum\limits_{e \in P_{yz}}l_e$ $=$
$\sum\limits_{e \in P_{xy} \cup P_{yz}}l\_e$ $\ge$ 
$\sum\limits_{e \in P_{xz}}l_e$ $=$ 
$d_{xz}$.
Notice that concatinating path from $x$ to $y$ and path from $y$ to $z$ is a path from $x$ to $z$.
Which means at least longer or equal than "shortest" path from $x$ to $z$ in other world $P_{xy} \cup P_{yz}$ $\supseteq$ $P_{xz}$.

Now, problem is that we need to go through some $x$ that was not in $V$ but is in $V'$.
Therefore, we need to remove every such vertices.

Here is another algorithm that gives a tree from metric.
<div class="alg">
    $\operatorname{fit}$(V, d)<br>
    Find a tree metric $(V',T)$ that approximates $d$<br>
    <div class="alg">
        $T' \leftarrow T$<br>
        $\operatorname{while}$ $\exists v \in V$ and $v$'s parent $w$ such that $w$ $\in$ $V'$ and $w$ $\not\in$ $V$
        <div class="alg">
            Merge $v$ and $w$ to $v$
        </div>
    </div>
    Multiply the length of every edge of $T'$ by 4<br>
    return $(V, T')$
</div>


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.