---
layout: post
title: Approximation algorithm(11) - survivable network design and iterated rounding
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Survivable network design is a problem that designing a network that can survive from disconnecting some edges.

Let's assume there is a given undirected graph $G = (V,E)$ with costs $c_e \ge 0$ $\forall e \in E$.
Now, let's define connectivity requirments $r_{ij} \in \mathbb{Z}^{+} \cup \\{0\\}$ for all pairs of vertices $i,j \in V$, where $i \neq j$.
Then problem is a find a minimum cost set of edges $F \subset E$ such that $G' = (V,F)$ has $r_{ij}$ distinct paths to connecting $i$ and $j$.

Now, we can do a integer programming relaxation over this and it is like follow.
"Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge \max\limits_{i \in S, j \not\in S} r_{ij}$ $\forall S \subset V$, $x_e \in \\{0,1\\}$" which $\delta(S)$ denotes the set of edges between $S$ and $V - S$.

Now, we can do a linear programming relaxation like we did in the set cover.
"Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge \max\limits_{i \in S, j \not\in S} r_{ij}$ $\forall S \subset V$, $0 \le x_e \le 1$".

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.\