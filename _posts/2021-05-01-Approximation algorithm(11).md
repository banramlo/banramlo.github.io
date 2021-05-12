---
layout: post
title: Approximation algorithm(11) - Generalized Steiner tree problem
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Generalized Steiner tree problem is a special case of survivable network design problem which all $r_{ij} = 1$.
Now let's define $S_i = \\{S \subseteq V \vert |S \cap \\{s_i, t_i\\}| = 1\\}$.
Notice that $S_i$ is a set of subsets that make proper s-t cut for $s_i, t_i$.
Therefore, problem is like follow.

For given pairs of vetices $s_i, t_i$,<br>
minimize $\sum\limits_{e \in E}c_e x_e$ such that<br>
$\sum\limits_{e \in \delta(S)} x_e \ge 1$, for all $S \subseteq V$ which $S \in S_i$ for some $i$, <br>
$x_e \in \\{0, 1\\}$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.