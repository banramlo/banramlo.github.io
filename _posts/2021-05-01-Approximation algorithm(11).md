---
layout: post
title: Approximation algorithm(11) - Generalized Steiner tree problem
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

There is a problem known as a steiner tree problem.
The steiner tree problem is a problem to find a minimum edge set that connects every vertex in $S \subseteq V$ from a given graph $G = (V,E)$.
Notice that this problem doesn't care whether you included some vertices $v \in V - S$ in the edge set.
Moreover, this problem is a superset of the minimum spanning tree problem and shortest path problem.
We can construct problem like $S = V$ or $S = \\{s, t\\}$ to solve the minimum spanning tree problem and shortest path problem repectively.
Notice that this problem will result in a tree because it will not have a cycle which doesn't make sense for minimum edge set with connectivity.

Generalized steiner tree problem is a problem that requires to connect some pairs of vertices of given graph $G$.
If we select one vertex $v$ and set a required pairs as $v$ and others, it's the steiner tree problem.
Which means Generalized steiner tree problem is a generalized version of steiner tree problem like it named.
At the same time, it's a special case of survivable network design problem which all $r_{ij} = 1$.
Now let's define $\delta(S) = \\{e | e = (u,v) \text{ such that } u \in S, v \in V - S \\}$ and $S_i = \\{S \subseteq V : \left\vert S \cap \\{s_i, t_i\\} \right\vert = 1\\}$.
Notice that $S_i$ is a set of subsets that make a proper s-t cut for $s_i, t_i$.
Therefore, problem can be written as LP follow.

For given pairs of vetices $s_i, t_i$,<br>
minimize $\sum\limits_{e \in E}c_e x_e$ such that<br>
$\sum\limits_{e \in \delta(S)} x_e \ge 1$, for all $S \subseteq V$ which $S \in S_i$ for some $i$, <br>
$x_e \in \\{0, 1\\}$.

Now IP above can be relaxed to LP below.

For given pairs of vetices $s_i, t_i$,<br>
minimize $\sum\limits_{e \in E}c_e x_e$ such that<br>
$\sum\limits_{e \in \delta(S)} x_e \ge 1$, for all $S \subseteq V$ which $S \in S_i$ for some $i$, <br>
$x_e \ge 0$.

Now let's make a dual of primal like follow.

Maximize $\sum\limits_{e \in \delta(S), S \in S_i, \forall i}y_S$ such that<br>
$\sum\limits_{e \in \delta(S)} y_S \le c_e$ for all $e \in E$<br>
$y_S \ge 0$.

Now, let's think it as following for intuition.
$v \in V$ will be spread at the space with edge distance $c_e$.
For any $S \subseteq V$, we can think about some moats surrounding that $S$.
Now, let's think a width of moat for $S$ as $y_S$.
Then it can be thought as maximizing moat's width.
With constraint that moats can't overlapped.

Then we can try algorithm follow.
<div class="alg">
</div>


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.