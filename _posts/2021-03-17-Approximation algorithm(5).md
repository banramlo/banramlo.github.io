---
layout: post
title: Approximation algorithm(5) - K-center
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

We can imagine some clustering problem with $k$-centers.
For $G = (V,E)$ with distance $d_{ij} \ge 0$ between each pair of vertices $i,j \in V$.
We assume that $d_{ii} = 0, d_{ij} = d_{ji}, d_{ik} + d_{kj} \ge d_{ij}$ for each $i,j,k \in V$.
Now, problem is find $S \subset V$ which $|S| = k$.
Which makes $max(d(j,S))$ smallest.
It's like making clusters efficiently.

There is a $2$-approximation algorithm for this.
Let's define $d(i,S) = \min_{j \in S} d_{ij}$.
<div class="algorithm">
    Pick arbitrary $i \in V$<br>
    $S \leftarrow$ {$i$}<br>
    $\operatorname{while}$ $|S| < k$ $\operatorname{do}$
    <div class="algorithm">
    $j \leftarrow \operatorname{argmax}_{j \in V} d(j,S)$<br>
    $S \leftarrow S \cup$ {$j$}
    </div>
</div>

It's running time is trivial to be in polynomial time.
Now, let's define that $O$ $=$ {$e_1, e_2, \cdots, e_k$} is an optimal solution, $S^O$ $=$ {$S_1, S_2, \cdots, S_k$} as each cluster and $r^{\star}$ as its radius.
Then two points in each $S_i$ can't be greater than $2r^{\star}$ by triangle inequality.

Now, let's think about the soltuion of an approximation algorithm $A$.
Between the progress, if $A \neq O$ then in some moment algorithm needs to choose two points from some $S_i$.
However, it means that farest points are already bounded in $2r^{\star}$ from the fact above.
As a result, it can't make bigger radius than $2r^{\star}$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.